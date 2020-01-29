class: center, middle

# <span class="luigi">Luigi</span><br/>for Thrilling Data Pipelines

---

## Luigi overview

- A library developed by Spotify to manage data pipelines using python
- Object-oriented task definitions
- Some helpers to connect to databases like Redshift, Hadoop, etc.

---

## Parts of luigi

- Tasks: these are written as python classes
- Targets: these represent task inputs and outputs. e.g. a file on S3 is an S3Target, a row in a Redshift table is a RedshiftTarget
- Scheduler: makes sure identical tasks don't run simultaneously, provides a visualization of tasks
- Workers: these run tasks

---

## What's missing?

- Nothing inside luigi actually _triggers_ the tasks. To trigger tasks, we use cron.
- cron is a service that runs things according to a schedule, so it's the easiest way to trigger tasks
- If cron tries to schedule a task that is already running (or has already finished), the luigi scheduler will not allow it to run again.
- There are other ways to schedule tasks, e.g. using Amazon SQS

---

## Example tasks

```python
class FileIsOnS3(luigi.ExternalTask):
    path = luigi.Parameter()

    def output(self):
        return S3Target(path=self.path)

class DownloadFile(luigi.Task):
    path = luigi.Parameter()

    def requires(self):
        return FileIsOnS3(path=self.path)

    def run(self):
      o = urlparse(self.path, allow_fragments=False)
      s3 = boto3.client('s3')
      with open('local-file.txt', 'wb') as f:
          s3.download_fileobj(o.netloc, o.path.lstrip('/'), f)
```

---

## Example task<strike>s</strike>, simplified

```python
class DownloadFile(luigi.Task):
    path = luigi.Parameter()

    def input(self):
        return S3Target(self.path)  # This takes the place of the ExternalTask defined above

    def run(self):
      o = urlparse(self.path, allow_fragments=False)
      s3 = boto3.client('s3')
      with open('local-file.txt', 'wb') as f:
          s3.download_fileobj(o.netloc, o.path.lstrip('/'), f)
```

---

## Example task, even better

```python
class DownloadFile(luigi.Task):
    path = luigi.Parameter()

    def input(self):
        return S3Target(self.path)

    def output(self):
        return LocalTarget(path='local-file.txt')

    def run(self):
      o = urlparse(self.path, allow_fragments=False)
      s3 = boto3.client('s3')
      with self.output().open('wb') as f:
          s3.download_fileobj(o.netloc, o.path.lstrip('/'), f)
```

---

## Parameters

- Parameters determine the uniqueness of a task. So `DownloadFile(path=s3://some-bucket/some-key)` is different than `DownloadFile(path=s3://some-bucket/some-other-key)`, and the scheduler will know the difference.
- Types of parameters include strings (`Parameter`), `DateParameter`, `DateHourParameter`, `IntParameter`, `FloatParameter`, etc.

## Inputs and outputs

- Inputs are what a task needs in order to run. Usually, they're files on S3 (outputted by other tasks), database rows created by other tasks, etc. In our example, we were able to replace a simple task with an input.
- Outputs are what a task must produce in order to be considered done. Generally, they're what the luigi scheduler checks for to determine whether a task has already run. A task that defines an output but doesn't actually produce it will not be considered finished.

---

## Requirements

- Inputs and outputs can also be defined implicitly, as requirements.
- So if task `B` defines task `A` as a requirement, task `B` will implicitly have an `input` that is task `A`'s `output`.

---

## Multiple requirements

- If a task has multiple requirements, they should each be _yielded_ rather than returned, e.g.

```python
class ATaskWithMultipleRequirements(luigi.Task):

    def requires(self):
        yield RequiredTaskA()
        yield RequiredTaskB()
        yield RequiredTaskC()
```

```python
class AnotherTaskWithMultipleRequirements(luigi.Task):

    def requires(self):
        yield RequiredTask(path='s3://some-bucket/some-key')
        yield RequiredTask(path='s3://some-bucket/some-other-key')
        yield RequiredTask(path='s3://some-bucket/some-third-key')
```

---

This pattern also allows building requirements in loops, but be careful with dynamic dependencies.

```python
class AThirdTaskWithMultipleRequirements(luigi.Task):

    def requires(self):
        paths = ['s3://some-bucket/some-key', 's3://some-bucket/some-other-key', 's3://some-bucket/some-third-key']
        for path in paths:
            yield RequiredTask(path=path)
```

If your dependencies are dynamic, they should be yielded inside `run()` instead of `requires()`:

```python
class AFourthTaskWithMultipleRequirements(luigi.Task):

    def run(self):
        paths = get_requirements_somehow()
        for path in paths:
            yield RequiredTask(path=path)
```

## Where to find help

- [Luigi docs](https://luigi.readthedocs.io/en/stable/index.html)
- [Luigi on github](https://github.com/spotify/luigi) links to quite a few blog posts and writeups for how other companies have used luigi

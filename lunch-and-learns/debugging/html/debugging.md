class: center, middle

# Debugging: Schr&ouml;dinger&apos;s Code
### Erin Nichols and Chase Felker

---

## Debugging is a skill!
![xkcd](https://imgs.xkcd.com/comics/debugging.png)

---

## War Stories

- Django 1.5 &rarr; 1.7, Python 2.6 &rarr; 2.7.9
--

- MySlate is blowing up in NewRelic
--

- Can&apos;t add rows to MySQL database after 2147483647
--

- Apple News launch

---

## So, you have a bug

### Where to look
- Can you reproduce the bug?
- Is it broken on stage, dev, or local?
--


### Logs
- Do NewRelic, Splunk, or Elk have logs?
- Can you find logs on the instance (typically in `/var/log`)?
- Set debug level to `debug` or `info` to see logs locally

---

## Reading Logs
- Stacktrace / traceback
- Django html error messages


```
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/data/django/my_slate/lib/python2.7/site-packages/requests/api.py", line 69, in get
    return request('get', url, params=params, **kwargs)
  File "/data/django/my_slate/lib/python2.7/site-packages/requests/api.py", line 50, in request
    response = session.request(method=method, url=url, **kwargs)
  File "/data/django/my_slate/lib/python2.7/site-packages/requests/sessions.py", line 465, in request
    resp = self.send(prep, **send_kwargs)
  File "/data/django/my_slate/lib/python2.7/site-packages/requests/sessions.py", line 573, in send
    r = adapter.send(request, **kwargs)
  File "/data/django/my_slate/lib/python2.7/site-packages/requests/adapters.py", line 370, in send
    timeout=timeout
  File "/data/django/my_slate/lib/python2.7/site-packages/requests/packages/urllib3/connectionpool.py", line 544, in urlopen
    body=body, headers=headers)
  File "/data/django/my_slate/lib/python2.7/site-packages/requests/packages/urllib3/connectionpool.py", line 344, in _make_request
    self._raise_timeout(err=e, url=url, timeout_value=conn.timeout)
  File "/data/django/my_slate/lib/python2.7/site-packages/requests/packages/urllib3/connectionpool.py", line 314, in _raise_timeout
    if 'timed out' in str(err) or 'did not complete (read)' in str(err):  # Python 2.6
TypeError: __str__ returned non-string (type Error)
```
https://github.com/shazow/urllib3/issues/556

---

## But it works for me locally&hellip;

### What is different?
--

- code
--

- user data
--

- browser
--

- operating system
--

- Do you know _exactly_ what the user did to encounter the bug?
  - If possible, look over the user&rsquo;s shoulder

---

## Code can live in many places:
- Application files (.js, .py, .java, .scss)
--

- Configurations (properties files, .cfg files, environment/user vars, OSGI)
--

- Databases (MySQL, redis)
--

- Memory
--

- Compiled code (.pyc, .class, .jar, .css)
--

- Web browser: localStorage, cache, cookies
--

- Remote host: optimize.ly, third party javascript, external services (Janrain, Braintree)

---

## It&rsquo;s not the code!
- Browser
--

- Operating system
--

- Web servers
--

- CDNs
--

- SSL

---

## Heisenbug
&ldquo;The act of observation changes that which is being observed&rdquo;
- Logging changes the bug
- Exception raised in except/catch block
--


## Schr&ouml;dinbug
&ldquo;If you never open the box, the cat is simultaneously dead and alive&rdquo;

- It&apos;s only there if you&rsquo;re looking at it
- &ldquo;How did this ever work?&rdquo;

---

## Lessons
- Try different environments (dev/stage/prod/local, browsers)
--

- Don&rsquo;t just google for the stated problem (&ldquo;CSRF bug django&rdquo;)
--

- Do all of your in-office users have some characteristic that readers do not have?
--

- Ad blockers - &ldquo;that was the end of javascript&rdquo;
--

- Run early and often, make incremental git commits
--

- Keep track of what you&rsquo;ve tried
--

- Break things down into the simplest parts

--

### Question your expectations!
--

- Is my code even running???

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

- MySlate is blowing up in NewRelic (S+ podcast feed)
--

- Can&apos;t add rows to MySQL database after 2147483647
--

- Debugging Apple News launch (SSL)

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
- Application files (.js, .py, .java)
--

- Configurations (properties files, .cfg files, environment vars, OSGI)
--

- Databases (MySQL, redis, etc.)
--

- Memory
--

- Compiled code (.pyc, .class, jar, .css, etc.)
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

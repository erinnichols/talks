class: center, middle

# PullRequestBot 2.0

---

# Pieces

* AWS Lambda (nodejs)
* AWS API Gateway
* Apex ([apex.run](http://apex.run))
* Slack app / API
* Github webhooks / API
* MySlate users API

---

# Lambda &amp; nodejs

* __Lambda__ because it is &ldquo;Serverless&rdquo;, so I wouldn&rsquo;t have to worry about piggybacking on one of our macroservices
* __nodejs__ because we&rsquo;d just been to a weeklong nodejs training, so I wanted to retain what I&rsquo;d learned
* __I learned__:
  * javascript is not and never will be object-oriented.
  * there are libraries for Github and Slack written in nodejs, but if you want something compatible and actually promise-based, you often have to write it yourself

---

# API Gateway

* API Gateway is AWS&rsquo;s interface to Lambda. If you want a url for your Lambda function, this is how to get it.
* It also creates a cool way of injecting environment variables / secrets and using them with Lambda
* __I learned__:
  * API Gateway lets you create different &ldquo;stages&rdquo; for your API, so you can provide different environment variables to dev/stage/prod.
  * The testing interface is useful but super clunky.

---

# Apex

* http://apex.run/
* Apex is basically a build tool for Lambda that allows you to develop locally, push, and view logs
* Almost no clicking through AWS&rsquo;s &ldquo;UI&rdquo;, no pasting into textareas.
* I used a babel-webpack2 example because I didn&rsquo;t know what I was doing and thought I had to package all my javascript.
* Then I never changed it
* __I learned__: Apex is really essential for developing &amp; testing Lambda functions with any efficiency.

---

# Slack app &amp; API

* I created a [Slack app](https://api.slack.com/apps/A4YTWTUTT)
* Slack apps provide some tools for interacting with Slack, including the API.
* The app can also post as __PullRequestBot2.0__
* __I learned__: Slack&rsquo;s API is really dumb. It takes JSON payloads but only as urlencoded query strings. It doesn&rsquo;t include useful endpoints for looking up channels by name, looking up users by name, etc. I stuck a caching layer around it because it requires so much iteration over lists.

---

# Github webhooks &amp; API

* Github has pretty good [API docs](https://developer.github.com/v3/), but their sample payloads aren&rsquo;t always up-to-date.
* It also has a great [testing interface for webhooks](https://github.com/organizations/slategroup/settings/hooks/13184300)
* __I learned__ how to take credit for other people&rsquo;s commits &#128520;

---

# MySlate users API

* I used Django Rest Framework to make a quick API interface to the SlackUser model so people can record their notification preferences
* It also looks people up in the Slack API so we don&rsquo;t have to do that every time

---

# Example: Create &amp; interact with a PR

---

# _`#TODO`_

* Climb out from under babel-webpack2
* ?

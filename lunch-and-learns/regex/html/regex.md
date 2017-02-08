class: center, middle

# Regular expressions in 20 minutes
### `/Reg(exp?|ular Expression)?/gi`

.footnote[.red.bold[*] Condensed and adapted from Lea Verou's [/Reg(exp){2}lained/](https://www.youtube.com/watch?v=EkluES9Rvak)]

---

# What are regular expressions?

* Regular expressions are a way to describe a set of strings (concisely)
* They're implemented efficiently

---
# Why use regex

* Validation (as in [html5 forms](validation.html), for example)
* Matching
* Substitution
* For powerful find &amp; replace. Use them in your text editor or IDE to easily update code.
* Processing xml with Java (if you thought regular expressions were ugly, try looking at them in Java)

--

```Java
return str.replaceAll("\\\\", "\\\\\\\\").replaceAll("\"", "\\\\\"").replaceAll("[\\r\\n]", "");
```

* The `String.replaceAll` method in java takes regular expressions, and requires an extra layer of escaping, so this code replaces 4 literal backslashes with 2 literal backslashes, and one double quote with a... quintuple-escaped double quote?!!!!!!

--

## ?!!!!!!

---

# How to read a regular expression

* (In most languages) regular expressions start and end with a slash `/`
* But in python, usually they're denoted with an r before a string, e.g. `r'.*?'`
* Between the slashes is the pattern to match
* After the slashes, there are flags
  * `g` for global (match as many times as possible)
  * `i` for case-insensitive
  * `m` for multi-line (matches across newlines)

---

# Types of characters in regular expressions

* Literal characters
* Capturing groups (if you want to use parts of the match later) are between `()`
* Character sets e.g. `[a-z0-9-_]`
* Quantifiers e.g. `{12}` `{,2}` `{3,}`
* Meta-characters
  * `.` matches pretty much any character (except line breaks)
  * `*` matches 0 to infinity times
  * `+` matches 1 to infinity times
  * `?` marks the preceding character or group as optional
  * `^` matches the beginning of a line (when at the beginning), or negates a character set
  * `$` matches the end of a line
  * `|` or
  * `\` escapes special characters

---

# Greediness

By default, regular expressions are greedy. They match as much of the string as possible. Certain characters can cause a regular expression not to be greedy, or to "give back" if it matches a shorter string.

## Example: Strip tags from html.

For this example, I want to start with a block of html and remove the tags, so I'm left with just the text. First, I'll write a regular expression to match a start or end tag.

???
https://leaverou.github.io/regexplained/

---

### html:

```html
Here is some text. <p>Here's a paragraph.</p>
```
--
### regular expression:

```javascript
/<.+\/?>/g
```
--

* `<` matches a literal `<`

--

* `.` matches any character

--

* `+` matches it 1 or more times

--

* `\/` is escaped, so it will match a literal slash, to match self-closing tags

--

* `?` makes the slash optional

--
## Will it work?

---

### html:

```html
Here is some text. <p>Here's a paragraph.</p>
```

---

### Nope! Our regex is going to match _everything_ between the first `<` and the last `>`

```html
Here is some text. `<p>Here's a paragraph.</p>`
```

### So, our regex is greedy. How can we make it lazy (not greedy)?

--

The key is adding a `?` after the `.+`. When we add a `?`, our regex will match the shortest string possible, rather than the longest string possible. So the regular expression:

--

```javascript
/<.+?\/?>/g
```

Will lazily match start tags _or_ end tags.

```html
Here is some text. `<p>`Here's a paragraph.`</p>`
```

---

# But _should_ we use regular expressions to match html?

Short answer: only if we know what html we're expecting.

Regular expressions can only parse regular languages, and html is not a regular language.

See the [famous StackOverflow post](http://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags).

---

# Regular expressions in Slate code

```python
url(r'feeds/third-party/', include('my_slate.api.feeds.urls')),
(r'^ingest_article/(?P<articleId>[^/]+)/(?P<update>[^/]+)', views.add_article_to_cache),
(r'^ingest_article/(?P<articleId>[^/]+)', views.add_article_to_cache),
```




![Good news, everyone!](https://media.giphy.com/media/3o7abA4a0QCXtSxGN2/giphy.gif)

We live in the future.

[regex 101](https://regex101.com) exists.

Test your regular expressions in realtime in one of four flavors, and get a breakdown of what matches and why.

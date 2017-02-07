class: center, middle

# Regular expressions in 20 minutes
### `/Reg(exp?|ular Expression)?/gi`

---

# Why use regex

* Validation (I hear html5 forms support regular expression validation)
* Matching
* Substitution
* For powerful find &amp; replace while writing code
* Processing xml in Java (just kidding!)

--

```Java
return str.replaceAll("\\\\", "\\\\\\\\").replaceAll("\"", "\\\\\"").replaceAll("[\\r\\n]", "");
```

---

# How to read a regular expression

* (In most languages) regular expressions start and end with a slash `/`
* Between the slashes is the pattern to match
* After the slashes, there are flags
  * `g` for global (match as many times as possible)
  * `i` for case-insensitive
  * `m` for multi-line (matches across newlines)

---

# Types of characters in regular expressions

* Literal characters
* Capturing groups (if you want to "capture" parts of the pattern later) are between `()`
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

## Exapmle: Strip tags from html.

For this example, I want to start with a block of html and remove the tags, so I'm left with just the text. First, I'll write a regular expression to match a start or end tag.

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

* `\/` is escaped, so it will match a literal slash, so it will match self-closing tags

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

# Good news!
[![Good news, everyone!](http://vignette2.wikia.nocookie.net/en.futurama/images/a/ad/GoodNewsEveryone.jpg/revision/latest?cb=20090731021518)]

We live in the future.

[regex 101](https://regex101.com) exists.

Test your regular expressions in realtime in one of four flavors, and get a breakdown of what matches and why.

---
layout: page
title: Javascript, HTML, & CSS
section: code-style
permalink: /code-style/js-html-css/
---

## Reference Code Standard

Google’s Javascript style guide on Github: [https://google.github.io/styleguide/jsguide.html](https://google.github.io/styleguide/jsguide.html)

Google also provides an HTML/CSS guide: [https://google.github.io/styleguide/htmlcssguide.html](https://google.github.io/styleguide/htmlcssguide.html)

## Exceptions to the Standard

The one exception is that preexisting libraries included in your applications should not be changed to fit the standard.  Hopefully you’re extending a library and able to craft the extension in a standardized format.

## Implementing the Standard

Some IDE’s provide linting support built into their package system.  For command-line you’ll need to install the package with administrative/root privilege

```
npm install jshint
```

## Linux/Mac shell

which tidy # It may exist on your system by default, e.g. Mac OSX

```
sudo apt-get install tidy # or other relevant package manager CLI

npm install csslint # Optional
```

Enforcement would require this script installed as part of a project.  You could also integrate this into your testing or CI environment, but in a traditional sense, we want to prevent the style issues from getting past the engineer’s code branch.

## Appendix: Sample Implementation



To help enforce the standard, you can hook into Git.  This can easily be applied to any language enforcement tooling.

Here’s an example pre-commit hook script:

Reference: https://gist.github.com/allan-simon/aeebc350a98035ac0257

```
#!/bin/sh

files=$(git diff --cached --name-only --diff-filter=ACM | grep "\.js$")
if [ "$files" = "" ]; then
    exit 0
fi

pass=true

echo "\nValidating JavaScript:\n"

for file in ${files}; do
    result=$(jshint ${file} | grep "${file} is OK")
    if [ "$result" != "" ]; then
        echo "\t\033[32mJSHint Passed: ${file}\033[0m"
    else
        echo "\t\033[31mJSHint Failed: ${file}\033[0m"
        pass=false
    fi
done

echo "\nJavaScript validation complete\n"

if ! $pass; then
    echo "\033[41mCOMMIT FAILED:\033[0m Your commit contains files that should pass JSHint but do not. Please fix the JSHint errors and try again.\n"
    exit 1
else
    echo "\033[42mCOMMIT SUCCEEDED\033[0m\n"
fi
```

---
### Document Revision History

| Version | Revision date | Revision description   |
|---------|---------------|------------------------|
| 1.0     | 27-Apr-2018   | Initial public release |
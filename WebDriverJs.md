# Quick Start Guide

```
npm install selenium-webdriver
```

```javascript
// google_search.js
var webdriver = require('selenium-webdriver'),
    By = webdriver.By,
    until = webdriver.until;

var driver = new webdriver.Builder()
    .forBrowser('firefox')
    .build();

driver.get('http://www.google.com/ncr');
driver.findElement(By.name('q')).sendKeys('webdriver');
driver.findElement(By.name('btnG')).click();
driver.wait(until.titleIs('webdriver - Google Search'), 1000);
driver.quit();
```

```
node google_search
```

# API Documentation

The JavaScript API documentation is published with each release and available [here](http://seleniumhq.github.io/selenium/docs/api/javascript/). Please file [bug reports](https://github.com/SeleniumHQ/selenium/issues) for any missing or unclear information in the API docs.

* For general setup, see the [main landing page](http://seleniumhq.github.io/selenium/docs/api/javascript/)
* For configuring and creating new WebDriver sessions, use the [Builder](http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/builder_exports_Builder.html) class
* For browser-specific configuration, refer to the relevant browser sub-modules:
  - [selenium-webdriver/chrome](http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/chrome.html)
  - [selenium-webdriver/edge](http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/edge.html)
  - [selenium-webdriver/firefox](http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/firefox/index.html)
  - [selenium-webdriver/opera](http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/opera.html)
  - [selenium-webdriver/phantomjs](http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/phantomjs.html)

## Understanding the Promise Manager

WebDriver's JavaScript API is entirely asynchronous and every command results in a promise. Promise-heavy APIs will be a lot easier to work with once Node supports [ES2017's async functions](http://www.2ality.com/2016/02/async-functions.html), but in the meantime, WebDriverJS uses a custom promise library with a promise manager called the ControlFlow. The ControlFlow implicitly synchronizes asynchronous actions, making it so you only have to register a promise callback when you want to catch an error or access a return value. Detailed information on the ControlFlow and WebDriverJS's [promise library](http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/lib/promise.html) is included with the API docs.
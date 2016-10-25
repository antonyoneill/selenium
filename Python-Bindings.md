# Introduction

The python bindings for Selenium 2 are now available. The bindings include the full functionality of Selenium 1 and 2 (WebDriver). The package currently supports the Remote, Firefox, Chrome and IE protocols natively.

_note_ : Currently Selenium only supports Python 2.6, 2.7, 3.2, 3.3


If using selenium 1, before attempting to run a test, be sure to [download the Selenium Server jar](http://selenium-release.storage.googleapis.com/index.html) file, and run it via
```
java -jar selenium-server-standalone.jar
```
in your terminal/cmd prompt, before attempting to run a test. Selenium 2 however, does not require the jar file.

# Installation

Currently, there are two versions of Selenium available for use.

## Latest Official Release
The first version, is the latest official release, which is available on the Python Package Index http://python.org/pypi/selenium. To use this version, in your terminal type:

`[sudo] easy_install selenium` or `[sudo] pip install selenium`


## Development Version

The second version, is the current code from trunk. To use this, checkout the trunk repository at https://github.com/SeleniumHQ/selenium.
After the download is completed, cd to the root of the downloaded directory via terminal/cmd prompt. Perform the following commands:

```
./go py_prep_for_install_release
python setup.py install
```

Upon completion, the package should be installed successfully.

One advantage of using trunk as of writing, is the reorganization of the package. Previously, to initialize a browser you had to perform,

```python
from selenium.firefox.webdriver import WebDriver
driver = WebDriver()
```

This has been changed, so now all that is required is:

```python
from selenium import webdriver
driver = webdriver.Firefox()
```

## Tests

When developing Selenium, it is recommended you run the tests before and after making any changes to the code base. To perform these tests, you will first need to install [Tox](http://tox.readthedocs.io/).

By default, running `tox` will attempt to execute all of the defined environments. This means the tests for python 2.7 and 3.5 will run for each of the supported drivers. This is most likely not what you want, as some drivers will not run on certain platforms. It is therefore recommended that you specify the environments you wish to execute. To list all environments available, run `tox -l`, and to execute a single environment, use `tox -e`.

As an example, this command will run the tests for Firefox against python 2.7:

```
tox -e py27-firefox
```

The tests are executed using [pytest](http://docs.pytest.org/), and you can pass positional arguments through Tox by specifying `--` before them. In addition to other things, this allows you to filter tests. For example, to run a single test file:

```
tox -e py27-firefox -- py/test/selenium/webdriver/common/visibility_tests.py
```

To run a single test, you can use the keyword filter, such as:

```
tox -e py27-firefox -- -k testShouldShowElementNotVisibleWithHiddenAttribute
```

### Expected Failures

Unfortuantely, there will be some tests that are expected to fail due to known issues. You can mark these tests using the [standard pytest methods](http://docs.pytest.org/en/latest/skipping.html#mark-a-test-function-as-expected-to-fail), however this will mark the tests for all drivers. If a test is only expected to fail in a subset of drivers, you can extend the xfail mark with the name of the driver. For example, to mark a test as expected to fail in Chrome and Firefox (but pass using any other driver):

```python
import pytest

@pytest.mark.xfail_chrome
@pytest.mark.xfail_firefox
def test_something(driver):
   assert something is True
```

All of the same arguments from pytest's [xfail mark](http://docs.pytest.org/en/latest/skipping.html#mark-a-test-function-as-expected-to-fail) are available to these extended marks. Wherever possible you should provide a `reason` with a reference to the raised issue/bug. If the test raises an unexpected exception you should also provide the `raises` argument, as this will still cause a failure if the test starts failing for another reason.

If the expected failure is dependent on the platform, you should also include the `condition` argument so that the test will be allowed to pass on other environments. For example, to mark a test as expected to fail when run against Firefox on macOS:

```python
import sys
import pytest
from selenium.common.exceptions import WebDriverException

@pytest.mark.xfail_firefox(
    condition=sys.platform == 'darwin',
    reason='https://myissuetracker.com/issue?id=1234',
    raises=WebDriverException
def test_something(driver):
   assert something is True
```

You should avoid using [imperative xfail](http://docs.pytest.org/en/latest/skipping.html#imperative-xfail-from-within-a-test-or-setup-function) as these will never allow the test an opportunity to unexpectedly pass (when the issue is resolved).

We also recommend against using [skip](http://docs.pytest.org/en/latest/skipping.html#marking-a-test-function-to-be-skipped) unless there is good reason. If your test failure causes a hang or some other undesirable side-effect you can pass `run=False` to the xfail mark.

To run expected failures locally, pass the `--runxfail` command line option to pytest. If you want to run all expected failures for a specific driver you can do this by filtering on the xfail mark:

```
tox -e py27-firefox -- -m xfail_firefox --runxfail
```

# Usage

Depending on the driver you wish to utilize, importing the module is performed by entering the following in your python shell:

Selenium 1:

```python
from selenium import selenium
```

Selenium 2:

```python
from selenium import webdriver
```

## API Documentation / Viewing Available Functionality

To read the API documentation of the Python Bindings go to the [Python bindings API doc page](http://selenium.googlecode.com/svn/trunk/docs/api/py/index.html).

Alternatively use your python shell to view all commands available to you, after importing perform:

Selenium 1:

```python
dir(selenium)
```

Selenium 2:

```python
dir(webdriver)
```

To view the docstrings (documentation text attached to a function or method), perform

```python
print('functionname'.__doc__)
```

where `functionname` is the function you wish to view more information on. For example,

```python
print(selenium.open.__doc__)
```

## Comparison with Java Bindings

Here is a summary of the major differences between the python and Java bindings.

### Function Names

Function names separate compound terms with underscores, rather than using Java's camelCase formatting. For example, in python `title` is the equivalent of `getTitle()` in Java.

### Flatter Structures

To reflect pythonic behavior of flat object hierarchies the python bindings e.g. `find_element_by_xpath("//h1")` rather than `findElement(By.xpath("//h1"));` but it does give you the freedom of doing `find_element(by=By.XPATH, value='//h1')`

# Browser Support

All of the browsers supported by the Java implementation of Selenium are available in the Python bindings. For example:

### Selenium 1 - Internet Explorer

```python
from selenium import selenium
selenium = selenium("localhost", 4444, "*iexplore", "http://google.com/")
selenium.start()
```

### Selenium 1 - Firefox

```python
from selenium import selenium
selenium = selenium("localhost", 4444, "*firefox", "http://google.com/")
selenium.start()
```

### Selenium 2 - Firefox

```python
from selenium import webdriver
driver = webdriver.Firefox()
```

### Selenium 2 - Chrome

```python
from selenium import webdriver
driver = webdriver.Chrome()
```

### Selenium 2 - Remote

```python
from selenium import webdriver
driver = webdriver.Remote(browser_name="firefox", platform="any")
```
### Selenium 2 - IE

```python
from selenium import webdriver
driver = webdriver.Ie()
```

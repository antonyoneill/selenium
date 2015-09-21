- [Why am I getting a NoSuchElementException](#org.openqa.selenium.NoSuchElementException)?
- [Why am I getting a StaleElementReferenceException](#org.openqa.selenium.StaleElementReferenceException)

### org.openqa.selenium.NoSuchElementException

A NoSuchElementException appears when the element you are attempting to find does not satisfy your selector strategy.  
*Consider the following:*

```html
<a id="my-link" href="...">My Link</a>
```

```java
// works
driver.findElement(By.id("my-link"));
driver.findElement(By.linkText("My Link"));

// throws org.openqa.selenium.NoSuchElementException
driver.findElement(By.linkText("My link")); // lowercase "L"
```

### org.openqa.selenium.StaleElementReferenceException

A StaleReferenceException is thrown when an element that was identified previously has been mutated in the DOM and you attempted to interact with it after the mutation.  
*Consider the following:*

```html
<a id="my-link" onclick="this.href='new'" href="...">Link Text</a>
```

```java
@FindBy(id = "my-link")
WebElement link;

link.click(); // works
link.click(); // throws because element has been mutated
```

### org.openqa.selenium.UnsupportedCommandException
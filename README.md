# Webium

Webium is a Page Object pattern implementation library for Python (http://martinfowler.com/bliki/PageObject.html).

It allows you to extend WebElement class to your custom controls like Link, Button and group them as pages.

Main classes are:

- webium.Find
- webium.Finds
- webium.BasePage

Basic usage example:

```python

from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webelement import WebElement
from webium.controls.link import Link
from webium.driver import get_driver
from webium import BasePage, Find, Finds


class GooglePage(BasePage):
    input = Find(by=By.NAME, value='q')
    button = Find(by=By.NAME, value='btnK')

    def __init__(self):
        super(GooglePage, self).__init__(url='http://www.google.com')


class ResultItem(WebElement):
    link = Find(Link, By.XPATH, './/h3/a')


class ResultsPage(BasePage):
    stat = Find(by=By.ID, value='resultStats')
    results = Finds(ResultItem, By.XPATH, '//div/li')


if __name__ == '__main__':
    home_page = GooglePage()
    home_page.open()
    home_page.input.send_keys('Page Object')
    home_page.button.click()
    results_page = ResultsPage()
    print 'Results summary: %s' % results_page.stat.text
    for item in results_page.results:
        print item.link.text
    get_driver().quit()
```

More usage details are available here: http://wgnet.github.io/webium/.
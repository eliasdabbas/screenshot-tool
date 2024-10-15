# Screenshot taker tool


- Create a file and call it something like `screenshot_tool.py` using the code below.
- Make sure to add the URLs for which you want screenshots under the `start_urls` attribute before saving the file.
- From the command line run the following:

```bash
scrapy runspider screenshot_tool.py
```
- The screenshots will be saved in the same folder using the last directory of each URL with a `.png` extension.

`screenshot_tool.py`
```python
from scrapy import Spider, Request
from scrapy_playwright.page import PageMethod
class ScreenshotSpider(Spider):
    name = 'screenshot_spider'
    start_urls = [
        # Add your URLs here:
        'https://example.com',
    ]
    custom_settings = {
        "DOWNLOAD_HANDLERS": {
            "http": "scrapy_playwright.handler.ScrapyPlaywrightDownloadHandler",
            "https": "scrapy_playwright.handler.ScrapyPlaywrightDownloadHandler",
    }, 'TWISTED_REACTOR': "twisted.internet.asyncioreactor.AsyncioSelectorReactor",
        'USER_AGENT': "Mozilla/5.0 (iPhone; CPU iPhone OS 17_7 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.0 Mobile/15E148 Safari/604.1"
    }
    def start_requests(self):
        for url in self.start_urls:
            yield Request(
                url=url,
                meta={
                    "playwright": True,
                    "playwright_page_methods": [
                        PageMethod("screenshot", path=url.strip('/').split('/')[-1].replace('.', '_') + '.png', full_page=True),
                    ],
                },
            )

    def parse(self, response, **kwargs):
        screenshot = response.meta["playwright_page_methods"][0]
```

## Requirements

```
scrapy
scrapy-playwright
```
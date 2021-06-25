# 官方Demo学习
## 下载官方demo
官方网上的demo提供的地址如下：  [quotebot](https://github.com/scrapy/quotesbot)

## 学习demo
我们可以看到spiders目录下有2个py爬虫文件一个是css的，一个是xpath的，我们这里只看css的吧 。
代码如下：
``` python
# -*- coding: utf-8 -*-
import scrapy


class ToScrapeCSSSpider(scrapy.Spider):
    name = "toscrape-css"
    start_urls = [
        'http://quotes.toscrape.com/',
    ]

    def parse(self, response):
        for quote in response.css("div.quote"):
            yield {
                'text': quote.css("span.text::text").extract_first(),
                'author': quote.css("small.author::text").extract_first(),
                'tags': quote.css("div.tags > a.tag::text").extract()
            }

        next_page_url = response.css("li.next > a::attr(href)").extract_first()
        if next_page_url is not None:
            yield scrapy.Request(response.urljoin(next_page_url))
```
我们可以看到这个爬虫设置了name,start_urls。
在parse方法里面，前面的for循环是解析页面，提取item,后面获取超链接然后请求下一页。

貌似官方样例就只能分析这么多。

官方的这个demo很基础
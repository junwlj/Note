pip freeqze > requirments.txt

scrapy 本身是去重的

hasattr()：

getattr()：



```
scrapy shell https://www.dushu.com/book/1117.html
2019-04-25 09:23:30 [scrapy.utils.log] INFO: Scrapy 1.6.0 started (bot: scrapybot)
2019-04-25 09:23:30 [scrapy.utils.log] INFO: Versions: lxml 4.2.5.0, libxml2 2.9.8, cssselect 1.0
.3, parsel 1.5.1, w3lib 1.20.0, Twisted 19.2.0, Python 3.7.1 (default, Dec 10 2018, 22:54:23) [MS
C v.1915 64 bit (AMD64)], pyOpenSSL 18.0.0 (OpenSSL 1.1.1a  20 Nov 2018), cryptography 2.4.2, Pla
tform Windows-10-10.0.17134-SP0
2019-04-25 09:23:30 [scrapy.crawler] INFO: Overridden settings: {'DUPEFILTER_CLASS': 'scrapy.dupe
filters.BaseDupeFilter', 'LOGSTATS_INTERVAL': 0}
2019-04-25 09:23:30 [scrapy.extensions.telnet] INFO: Telnet Password: da922b60a68c4ddd
2019-04-25 09:23:30 [scrapy.middleware] INFO: Enabled extensions:
['scrapy.extensions.corestats.CoreStats',
 'scrapy.extensions.telnet.TelnetConsole']
2019-04-25 09:23:30 [scrapy.middleware] INFO: Enabled downloader middlewares:
['scrapy.downloadermiddlewares.httpauth.HttpAuthMiddleware',
 'scrapy.downloadermiddlewares.downloadtimeout.DownloadTimeoutMiddleware',
 'scrapy.downloadermiddlewares.defaultheaders.DefaultHeadersMiddleware',
 'scrapy.downloadermiddlewares.useragent.UserAgentMiddleware',
 'scrapy.downloadermiddlewares.retry.RetryMiddleware',
 'scrapy.downloadermiddlewares.redirect.MetaRefreshMiddleware',
 'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware',
 'scrapy.downloadermiddlewares.redirect.RedirectMiddleware',
 'scrapy.downloadermiddlewares.cookies.CookiesMiddleware',
 'scrapy.downloadermiddlewares.httpproxy.HttpProxyMiddleware',
 'scrapy.downloadermiddlewares.stats.DownloaderStats']
2019-04-25 09:23:30 [scrapy.middleware] INFO: Enabled spider middlewares:
['scrapy.spidermiddlewares.httperror.HttpErrorMiddleware',
 'scrapy.spidermiddlewares.offsite.OffsiteMiddleware',
 'scrapy.spidermiddlewares.referer.RefererMiddleware',
 'scrapy.spidermiddlewares.urllength.UrlLengthMiddleware',
 'scrapy.spidermiddlewares.depth.DepthMiddleware']
2019-04-25 09:23:30 [scrapy.middleware] INFO: Enabled item pipelines:
[]
2019-04-25 09:23:30 [scrapy.extensions.telnet] INFO: Telnet console listening on 127.0.0.1:6023
2019-04-25 09:23:30 [scrapy.core.engine] INFO: Spider opened
2019-04-25 09:23:31 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.dushu.com/book/111
7.html> (referer: None)
2019-04-25 09:23:31 [py.warnings] WARNING: e:\software\anaconda\lib\site-packages\IPython\core\in
teractiveshell.py:917: UserWarning: Attempting to work in a virtualenv. If you encounter problems
, please install IPython inside the virtualenv.
  warn("Attempting to work in a virtualenv. If you encounter problems, please "

[s] Available Scrapy objects:
[s]   scrapy     scrapy module (contains scrapy.Request, scrapy.Selector, etc)
[s]   crawler    <scrapy.crawler.Crawler object at 0x000002362AD82A20>
[s]   item       {}
[s]   request    <GET https://www.dushu.com/book/1117.html>
[s]   response   <200 https://www.dushu.com/book/1117.html>
[s]   settings   <scrapy.settings.Settings object at 0x000002362AD82BE0>
[s]   spider     <DefaultSpider 'default' at 0x2362b0812e8>
[s] Useful shortcuts:
[s]   fetch(url[, redirect=True]) Fetch URL and update local objects (by default, redirects are f
ollowed)
[s]   fetch(req)                  Fetch a scrapy.Request and update local objects
[s]   shelp()           Shell help (print this help)
[s]   view(response)    View response in a browser
In [1]: request.status
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-1-d6e73de85c85> in <module>
----> 1 request.status

AttributeError: 'Request' object has no attribute 'status'

In [2]: response.status
Out[2]: 200

In [3]: request.url
Out[3]: 'https://www.dushu.com/book/1117.html'

In [4]: request.xpath('//title/tex()').extract_first()
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-4-f76ca5f6d1dd> in <module>
----> 1 request.xpath('//title/tex()').extract_first()

AttributeError: 'Request' object has no attribute 'xpath'

In [5]: response.css('.page').css('a')
Out[5]: 
[<Selector xpath='descendant-or-self::a' data='<a href="/book/1117_2.html">2</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_3.html">3</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_4.html">4</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_5.html">5</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_6.html">6</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_7.html">7</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_8.html">8</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_9.html">9</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_10.html">10</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_11.html">11</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_12.html">12</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a href="/book/1117_13.html">13</a>'>,
 <Selector xpath='descendant-or-self::a' data='<a class="disabled" href="/book/1117_2.h'>]

In [6]: response.xpath('//title/tex()').extract_first()
---------------------------------------------------------------------------
XPathEvalError                            Traceback (most recent call last)
e:\software\anaconda\lib\site-packages\parsel\selector.py in xpath(self, query, namespaces, **kwa
rgs)
    237                              smart_strings=self._lxml_smart_strings,
--> 238                              **kwargs)
    239         except etree.XPathError as exc:

src/lxml/etree.pyx in lxml.etree._Element.xpath()

src/lxml/xpath.pxi in lxml.etree.XPathElementEvaluator.__call__()

src/lxml/xpath.pxi in lxml.etree._XPathEvaluatorBase._handle_result()

XPathEvalError: Invalid expression

During handling of the above exception, another exception occurred:

ValueError                                Traceback (most recent call last)
<ipython-input-6-210cc4bda077> in <module>
----> 1 response.xpath('//title/tex()').extract_first()

e:\software\anaconda\lib\site-packages\scrapy\http\response\text.py in xpath(self, query, **kwarg
s)
    117
    118     def xpath(self, query, **kwargs):
--> 119         return self.selector.xpath(query, **kwargs)
    120
    121     def css(self, query):

e:\software\anaconda\lib\site-packages\parsel\selector.py in xpath(self, query, namespaces, **kwa
rgs)
    240             msg = u"XPath error: %s in %s" % (exc, query)
    241             msg = msg if six.PY3 else msg.encode('unicode_escape')
--> 242             six.reraise(ValueError, ValueError(msg), sys.exc_info()[2])
    243
    244         if type(result) is not list:

e:\software\anaconda\lib\site-packages\six.py in reraise(tp, value, tb)
    690                 value = tp()
    691             if value.__traceback__ is not tb:
--> 692                 raise value.with_traceback(tb)
    693             raise value
    694         finally:

e:\software\anaconda\lib\site-packages\parsel\selector.py in xpath(self, query, namespaces, **kwa
rgs)
    236             result = xpathev(query, namespaces=nsp,
    237                              smart_strings=self._lxml_smart_strings,
--> 238                              **kwargs)
    239         except etree.XPathError as exc:
    240             msg = u"XPath error: %s in %s" % (exc, query)

src/lxml/etree.pyx in lxml.etree._Element.xpath()

src/lxml/xpath.pxi in lxml.etree.XPathElementEvaluator.__call__()

src/lxml/xpath.pxi in lxml.etree._XPathEvaluatorBase._handle_result()

ValueError: XPath error: Invalid expression in //title/tex()

In [7]: response.xpath('//title/text()').extract_first()
Out[7]: '\r\n\t童话世界 - 读书网|dushu.com\r\n'

In [8]: repsonse.css('.page')
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-8-9a3f27b2f59c> in <module>
----> 1 repsonse.css('.page')

NameError: name 'repsonse' is not defined

In [9]: respsonse.css('.page')
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-9-eb9eec2bdb0d> in <module>
----> 1 respsonse.css('.page')

NameError: name 'respsonse' is not defined

In [10]: response.css('.page')
Out[10]: [<Selector xpath="descendant-or-self::*[@class and contains(concat(' ', normalize-space(
@class), ' '), ' page ')]" data='<div class="page">\t\r\n   <div class="page'>]

In [11]: response.css('.pages a').xpath('./@href').extract()
Out[11]: 
['/book/1117_2.html',
 '/book/1117_3.html',
 '/book/1117_4.html',
 '/book/1117_5.html',
 '/book/1117_6.html',
 '/book/1117_7.html',
 '/book/1117_8.html',
 '/book/1117_9.html',
 '/book/1117_10.html',
 '/book/1117_11.html',
 '/book/1117_12.html',
 '/book/1117_13.html',
 '/book/1117_2.html']

In [12]: from scrapy.linkextractors import LinkExtractor

In [13]: from scrapy.linkextractors import LinkExtractor

In [14]: extractor = LinkExtractor(allow=r'/book/\w+\.html')

In [15]: links = extractor.extract_links(response)

In [16]: len(links)
Out[16]: 44

In [17]: links
Out[17]: 
[Link(url='https://www.dushu.com/book/1117.html', text='注册', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1077.html', text='文学', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1078.html', text='小说', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1081.html', text='传记', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1079.html', text='青春文学', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1082.html', text='艺术', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1163.html', text='散文随笔', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1222.html', text='收藏/鉴赏', fragment='', nofollow=False)
,
 Link(url='https://www.dushu.com/book/1003.html', text='人文社科', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1004.html', text='经济管理', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1094.html', text='自我实现/励志', fragment='', nofollow=Fa
lse),
 Link(url='https://www.dushu.com/book/1005.html', text='生活时尚', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1007.html', text='教育/教材', fragment='', nofollow=False)
,
 Link(url='https://www.dushu.com/book/1112.html', text='考试', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1008.html', text='少儿/童书', fragment='', nofollow=False)
,
 Link(url='https://www.dushu.com/book/1114.html', text='低幼读物', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1115.html', text='启蒙认知', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1116.html', text='科普文化', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1118.html', text='神话传说', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1119.html', text='寓言幽默', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1120.html', text='传统文化', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1121.html', text='文学天地', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1122.html', text='才艺课堂', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1123.html', text='游戏乐园', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1124.html', text='动漫世界', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1125.html', text='励志与成长', fragment='', nofollow=False
),
 Link(url='https://www.dushu.com/book/1126.html', text='生活知识', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1127.html', text='综合读物', fragment='', nofollow=False),

 Link(url='https://www.dushu.com/book/1117_1_0.html', text='可阅读', fragment='', nofollow=False
),
 Link(url='https://www.dushu.com/book/1117_2_0.html', text='可购买', fragment='', nofollow=False
),
 Link(url='https://www.dushu.com/book/1117_0_1.html', text='出版日期 ', fragment='', nofollow=Fa
lse),
 Link(url='https://www.dushu.com/book/1117_0_2.html', text='出版日期 ', fragment='', nofollow=Fa
lse),
 Link(url='https://www.dushu.com/book/1117_2.html', text='2', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_3.html', text='3', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_4.html', text='4', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_5.html', text='5', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_6.html', text='6', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_7.html', text='7', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_8.html', text='8', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_9.html', text='9', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_10.html', text='10', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_11.html', text='11', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_12.html', text='12', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_13.html', text='13', fragment='', nofollow=False)]

In [18]: links[0].url
Out[18]: 'https://www.dushu.com/book/1117.html'

In [19]: links[0].text
Out[19]: '注册'

In [20]: links[2].text
Out[20]: '小说'

In [21]: extractor = LinkExtractor(allow=r'/book/\d+_\d+\.html')

In [22]: links = extractor.extract_links(response)

In [23]: len(links)
Out[23]: 12

In [24]: links
Out[24]: 
[Link(url='https://www.dushu.com/book/1117_2.html', text='2', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_3.html', text='3', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_4.html', text='4', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_5.html', text='5', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_6.html', text='6', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_7.html', text='7', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_8.html', text='8', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_9.html', text='9', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_10.html', text='10', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_11.html', text='11', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_12.html', text='12', fragment='', nofollow=False),
 Link(url='https://www.dushu.com/book/1117_13.html', text='13', fragment='', nofollow=False)]

In [25]: fetch('https://www.dushu.com/book/')
2019-04-25 09:48:16 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.dushu.com/book/>
(referer: None)

In [26]: extractor = LinkExtractor(restrict_xpaths='//div[@class="sub-catalog margin-big-bottom
    ...: "]')

In [27]: links = extractor.extract_links(response)

In [28]: for link in links:
    ...:     print(link.url, link.text)
    ...: 
https://www.dushu.com/book/1001.html 古籍/国学
https://www.dushu.com/book/1002.html 文学艺术
https://www.dushu.com/book/1003.html 人文社科
https://www.dushu.com/book/1004.html 经济管理
https://www.dushu.com/book/1005.html 生活时尚
https://www.dushu.com/book/1006.html 科学技术
https://www.dushu.com/book/1007.html 教育/教材/教辅
https://www.dushu.com/book/1008.html 少儿
https://www.dushu.com/book/1009.html 工具书

In [29]: extractor = LinkExtractor(restrict_css='.sub-catalog')

In [30]: links = extractor.extract_links(response)

In [31]: for link in links:
    ...:     print(link.url, link.text)
    ...: 
https://www.dushu.com/book/1001.html 古籍/国学
https://www.dushu.com/book/1002.html 文学艺术
https://www.dushu.com/book/1003.html 人文社科
https://www.dushu.com/book/1004.html 经济管理
https://www.dushu.com/book/1005.html 生活时尚
https://www.dushu.com/book/1006.html 科学技术
https://www.dushu.com/book/1007.html 教育/教材/教辅
https://www.dushu.com/book/1008.html 少儿
https://www.dushu.com/book/1009.html 工具书

In [32]: extractor = LinkExtractor(allow=r'/book/\d+/')

In [33]: links = extractor.extract_links(response)

In [34]: print(links[0].url, links[0].text)
https://www.dushu.com/book/13469598/ 01植物大战僵尸2机器人漫画…


```


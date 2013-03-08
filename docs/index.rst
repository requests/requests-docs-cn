.. Requests documentation master file, created by
   sphinx-quickstart on Sun Feb 13 23:54:25 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Requests: HTTP for Humans
=========================

发行版本 v\ |version|. (:ref:`安装 <install>`)

Requests 是用Python语言编写，采用 :ref:`Apache2 Licensed <apache2>` 开源协议的 HTTP 库。

Python 标准库中的 **urllib2** 模块提供了大部分你需要的HTTP功能,但是它的API彻底地被 **破坏** 了。它是专为不同时间-不同网络而写的，这需要大量的工作（甚至是方法覆盖）去执行简单的任务。    
     
至少在Python中，事情不应该是这样的。    

::

    >>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
    >>> r.status_code
    200
    >>> r.headers['content-type']
    'application/json; charset=utf8'
    >>> r.encoding
    'utf-8'
    >>> r.text
    u'{"type":"User"...'
    >>> r.json()
    {u'private_gists': 419, u'total_private_repos': 77, ...}

参见 `未使用 Requests 的相似代码 <https://gist.github.com/973705>`_.

Requests 取代了 Python HTTP/1.1 以外的一切工作——— 让你与Web服务无缝结合. 无需在你的 URL 或者已编码的 POST 数据中手动添加查询字符串。搭载着嵌入在 Requests `urllib3 <https://github.com/shazow/urllib3>`_,Keep-alive（持久链接） 和 HTTP 连接池已全部实现自动化。


Testimonials
------------

Her Majesty's Government, Amazon, Google, Twilio, Mozilla, Heroku, PayPal, NPR, Obama for America, Transifex, Native Instruments, The Washington Post, Twitter, SoundCloud, Kippt, Readability, and Federal US Institutions use Requests internally. It has been downloaded over 2,000,000 times from PyPI.

**Armin Ronacher**
    Requests is the perfect example how beautiful an API can be with the
    right level of abstraction.

**Matt DeBoard**
    I'm going to get @kennethreitz's Python requests module tattooed
    on my body, somehow. The whole thing.

**Daniel Greenfeld**
    Nuked a 1200 LOC spaghetti code library with 10 lines of code thanks to
    @kennethreitz's request library. Today has been AWESOME.

**Kenny Meyers**
    Python HTTP: When in doubt, or when not in doubt, use Requests. Beautiful,
    simple, Pythonic.


支持的功能
---------------

Requests 完全满足如今网络的需求。

- 国际化域名和 URLs
- Keep-Alive & 连接池
- 持续性的 Cookie 会话
- 类浏览器式的 SSL 加密认证
- 基本/精简式的身份认证
- 优雅的键/值 Cookies
- 自动解压
- Unicode 编码的响应主体
- 多段文件上传
- 连接超时
- 支持 ``.netrc`` 
- 适用于 Python 2.6—3.3
- 安全的线程使用


用户指南
----------

这部分文档主要介绍了 Requests 的背景，然后对于 Requests 的应用做了一步一步的要点介绍。

.. toctree::
   :maxdepth: 2

   user/intro
   user/install
   user/quickstart
   user/advanced
   user/authentication


社区指南
-----------------

这部分文档主要详细地介绍了Requests的社区支持情况

.. toctree::
   :maxdepth: 1

   community/faq
   community/out-there.rst
   community/support
   community/updates

API 文档
-----------------

如果你正在寻找一个特殊函数，类或者方法的话，这部分文档刚好满足你。

.. toctree::
   :maxdepth: 2

   api


贡献者指南
-----------------

如果你对这个项目有兴趣，想做一点贡献，请参考下面文档

.. toctree::
   :maxdepth: 1

   dev/philosophy
   dev/internals
   dev/todo
   dev/authors

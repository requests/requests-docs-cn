.. Requests documentation master file, created by
   sphinx-quickstart on Sun Feb 13 23:54:25 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Requests: HTTP for Humans
=========================

发行版本 v\ |version|. (:ref:`安装 <install>`)

Requests 是使用 :ref:`Apache2 Licensed <apache2>` 许可证的 HTTP 库。用 Python 编写，真正的为人类着想。

Python 标准库中的 **urllib2** 模块提供了你所需要的大多数 HTTP 功能，但是它的 API 太渣了。它是为另一个时代、另一个互联网所创建的。它需要巨量的工作，甚至包括各种方法覆盖，来完成最简单的任务。    
     
在Python的世界里，事情不应该这么麻烦。    

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

Requests 使用的是 urllib3，因此继承了它的所有特性。Requests 支持 HTTP 连接保持和连接池，支持使用 cookie 保持会话，支持文件上传，支持自动确定响应内容的编码，支持国际化的 URL 和 POST 数据自动编码。现代、国际化、人性化。


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


功能特性
---------------

Requests 完全满足如今网络的需求。

- 国际化域名和 URLs
- Keep-Alive & 连接池
- 持久的 Cookie 会话
- 类浏览器式的 SSL 加密认证
- 基本/摘要式的身份认证
- 优雅的键/值 Cookies
- 自动解压
- Unicode 编码的响应体
- 多段文件上传
- 连接超时
- 支持 ``.netrc`` 
- 适用于 Python 2.6—3.4
- 线程安全


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
   dev/todo
   dev/authors

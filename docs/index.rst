.. Requests documentation master file, created by
   sphinx-quickstart on Sun Feb 13 23:54:25 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Requests: 让 HTTP 服务人类
=========================

发行版本 v\ |version|. (:ref:`安装说明 <install>`)

.. image:: https://img.shields.io/pypi/l/requests.svg
    :target: https://pypi.python.org/pypi/requests

.. image:: https://img.shields.io/pypi/wheel/requests.svg
    :target: https://pypi.python.org/pypi/requests

.. image:: https://img.shields.io/pypi/pyversions/requests.svg
    :target: https://pypi.python.org/pypi/requests

.. image:: https://codecov.io/github/requests/requests/coverage.svg?branch=master
    :target: https://codecov.io/github/requests/requests
    :alt: codecov.io

.. image:: https://img.shields.io/badge/Say%20Thanks!-🦉-1EAEDB.svg
    :target: https://saythanks.io/to/kennethreitz

Requests 唯一的一个\ **非转基因**\的 Python HTTP 库，人类可以安全享用。

**警告**：非专业使用其他 HTTP 库会导致危险的副作用，包括：安全缺陷症、冗余代码症、重新发明轮子\
症、啃文档症、抑郁、头疼、甚至死亡。

看吧，这就是 Requests 的威力：
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

Requests 允许你发送\ **纯天然，植物饲养**\的 HTTP/1.1 请求，无需手工劳动。你不需要手动为
URL 添加查询字串，也不需要对 POST 数据进行表单编码。Keep-alive 和 HTTP 连接池的功能是
100% 自动化的，一切动力都来自于根植在 Requests 内部的 `urllib3 <https://github.com/shazow/urllib3>`_。

用户见证
------------

Twitter、Spotify、Microsoft、Amazon、Lyft、BuzzFeed、Reddit、NSA、女王殿下的政府、\
Amazon、Google、Twilio、Mozilla、Heroku、PayPal、NPR、Obama for America、\
Transifex、Native Instruments、Washington Post、Twitter、SoundCloud、Kippt、Readability、\
以及若干不愿公开身份的联邦政府机构都在内部使用。

**Armin Ronacher**
    Requests 是一个完美的例子，它证明了通过恰到好处的抽象，API 可以写得多么优美。

**Matt DeBoard**
    我要想个办法，把 @kennethreitz 写的 Python requests 模块做成纹身。一字不漏。

**Daniel Greenfeld**
    感谢 @kennethreitz 的 Requests 库，刚刚用 10 行代码炸掉了 1200 行意大利面代码。\
    今天真是爽呆了！

**Kenny Meyers**
    Python HTTP: 疑惑与否，都去用 Requests 吧。简单优美，而且符合 Python 风格。


功能特性
---------------

Requests 完全满足今日 web 的需求。

- Keep-Alive & 连接池
- 国际化域名和 URL
- 带持久 Cookie 的会话
- 浏览器式的 SSL 认证
- 自动内容解码
- 基本/摘要式的身份认证
- 优雅的 key/value Cookie
- 自动解压
- Unicode 响应体
- HTTP(S) 代理支持
- 文件分块上传
- 流下载
- 连接超时
- 分块请求
- 支持 ``.netrc``

Requests 支持 Python 2.6—2.7以及3.3—3.7，而且能在 PyPy 下完美运行。

用户指南
----------

这部分文档是以文字为主，从 Requests 的背景讲起，然后对 Requests 的重点功能做了逐一的介绍。

.. toctree::
   :maxdepth: 2

   user/intro
   user/install
   user/quickstart
   user/advanced
   user/authentication

社区指南
----------

这部分文档也是文字为主，详细介绍了 Requests 的生态和社区。

.. toctree::
   :maxdepth: 1

   community/faq
   community/recommended
   community/out-there
   community/support
   community/vulnerabilities
   community/updates
   community/release-process

API 文档/指南
----------------

如果你要了解具体的函数、类、方法，这部分文档就是为你准备的。

.. toctree::
   :maxdepth: 2

   api

贡献指南
---------

如果你要为项目做出贡献，请参考这部分文档。

.. toctree::
   :maxdepth: 3

   dev/contributing
   dev/philosophy
   dev/todo
   dev/authors

没有别的指南了，你现在要靠自己了。

祝你好运。

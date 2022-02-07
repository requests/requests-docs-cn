.. Requests documentation master file, created by
   sphinx-quickstart on Sun Feb 13 23:54:25 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Requests: 让 HTTP 服务人类
============================

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


**Requests** 一个优雅、简单的 Python HTTP 库，为人类所建。

-------------------
**看吧，Requests的力量**::

    >>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
    >>> r.status_code
    200
    >>> r.headers['content-type']
    'application/json; charset=utf8'
    >>> r.encoding
    'utf-8'
    >>> r.text
    '{"type":"User"...'
    >>> r.json()
    {'private_gists': 419, 'total_private_repos': 77, ...}

参见 `未使用 Requests 的相似代码 <https://gist.github.com/973705>`_.


**Requests** 允许你极其简单地发送 HTTP/1.1 请求。
你不需要手动为URL 添加查询字符串（quering string），也不需要对 POST 数据进行form-encode。
Keep-alive 和 HTTP 连接池的功能是100% 自动化的，感谢 `urllib3 <https://github.com/shazow/urllib3>`_。

功能特性
---------------

Requests 准备好面对现在的互联网。

- Keep-Alive & 连接池
- 国际域名和 URLs
- 持久的带 Cookie 的Session
- 浏览器式 SSL 认证
- 自动化内容解码
- 基本/摘要式的身份认证
- 优雅的 key/value Cookies
- 自动解压
- Unicode 响应体
- HTTP(S) 代理支持
- 文件分块上传
- 流下载
- 连接超时
- 分块请求
- 支持 ``.netrc``
 
Requests 官方支持 Python 2.7和3.5以上版本，而且能在 PyPy 下良好运行。


用户指南
----------

这部分文档是以文字为主，从 Requests 的背景信息讲起，然后对 Requests 的重点功能做了逐一的介绍。

.. toctree::
   :maxdepth: 2

   user/install
   user/quickstart
   user/advanced
   user/authentication

社区指南
-------

这部分文档也是文字为主，详细介绍了 Requests 的生态和社区。

.. toctree::
   :maxdepth: 2

   community/recommended
   community/faq
   community/out-there
   community/support
   community/vulnerabilities
   community/release-process

   .. toctree::
   :maxdepth: 1

   community/updates

API 文档/指南
----------------

如果你要了解具体的函数、类、方法，这部分文档就是为你准备的。

.. toctree::
   :maxdepth: 2

   api

贡献者指南
---------

如果你要为项目做贡献，这部分文档就是为你准备的。

.. toctree::
   :maxdepth: 3

   dev/contributing

   dev/authors

没有别的指南了，你现在要靠自己了。

祝你好运。

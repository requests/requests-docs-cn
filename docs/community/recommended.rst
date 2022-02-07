.. _recommended:

推荐的库和扩展
===================================

Requests 拥有很多强大有用的第三方扩展。这里概述了其中最好的几个。

Certifi CA Bundle
-----------------

`Certifi`_ 是一个精心准备的根证书集合，用来验证 SSL 证书的可信任度，同时还会验证
TLS 主机的身份。这是一个从 Requests 项目中剥离出来的项目。

.. _Certifi: http://certifi.io/en/latest/

CacheControl
------------

`CacheControl`_ 这个扩展能为 Requests 添加完整的 HTTP 缓存功能。这样你的 web
请求的效率会高很多，这个扩展适合在需要大量 web 请求的场合使用。

.. _CacheControl: https://cachecontrol.readthedocs.io/en/latest/

Requests-Toolbelt
-----------------

`Requests-Toolbelt`_ 使一些有用工具的集合，有的用户会觉得它们很有用，但 Requests
中不适合将它们包进去。Requests 的核心组积极维护着这个库，它反映了社区用户最想要的\
一些功能。
.. _Requests-Toolbelt: http://toolbelt.readthedocs.io/en/latest/index.html

Requests-Threads
----------------

`Requests-Threads`_ is a Requests session that returns the amazing Twisted's awaitable Deferreds instead of Response objects. This allows the use of ``async``/``await`` keyword usage on Python 3, or Twisted's style of programming, if desired.

.. _Requests-Threads: https://github.com/requests/requests-threads


Requests-OAuthlib
-----------------

`requests-oauthlib`_ 使得从 Requests 自动进行 OAuth 跳转变成可能。这对很多使用
OAuth 进行验证的网站都很有用。它还提供了很多小配置，用来对非标准的 OAuth 提供者进行处理。

.. _requests-oauthlib: https://requests-oauthlib.readthedocs.io/en/latest/


Betamax
-------

`Betamax`_ 记录了你的 HTTP 交互，这样就不需要 NSA 费心了。这是一个专为 Python-Requests
设计的模拟录像机。

.. _betamax: https://github.com/sigmavirus24/betamax




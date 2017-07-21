.. _authentication:

身份认证
==============

本篇文档讨论如何配合 Requests 使用多种身份认证方式。

许多 web 服务都需要身份认证，并且也有多种不同的认证类型。
以下，我们会从简单到复杂概述 Requests 中可用的几种身份认证形式。


基本身份认证
------------

许多要求身份认证的web服务都接受 HTTP Basic Auth。这是最简单的一种身份认证，并且 Requests
对这种认证方式的支持是直接开箱即可用。


以 HTTP Basic Auth 发送请求非常简单：

::

    >>> from requests.auth import HTTPBasicAuth
    >>> requests.get('https://api.github.com/user', auth=HTTPBasicAuth('user', 'pass'))
    <Response [200]>


事实上，HTTP Basic Auth 如此常见，Requests 就提供了一种简写的使用方式：

::

    >>> requests.get('https://api.github.com/user', auth=('user', 'pass'))
    <Response [200]>


像这样在一个元组中提供认证信息与前一个 ``HTTPBasicAuth`` 例子是完全相同的。


netrc 认证
~~~~~~~~~~~~~~~~~~~~

如果认证方法没有收到 ``auth`` 参数，Requests 将试图从用户的 netrc
文件中获取 URL 的 hostname 需要的认证身份。The netrc file overrides raw HTTP authentication headers
set with `headers=`.

如果找到了 hostname 对应的身份，就会以 HTTP Basic Auth 的形式发送请求。


摘要式身份认证
---------------------

另一种非常流行的 HTTP 身份认证形式是摘要式身份认证，Requests 对它的支持也是开箱即可用的：

::

    >>> from requests.auth import HTTPDigestAuth
    >>> url = 'http://httpbin.org/digest-auth/auth/user/pass'
    >>> requests.get(url, auth=HTTPDigestAuth('user', 'pass'))
    <Response [200]>


OAuth 1 认证
----------------------

Oauth 是一种常见的 Web API 认证方式。 ``requests-oauthlib``
库可以让 Requests 用户简单地创建 OAuth 认证的请求：

::
    >>> import requests
    >>> from requests_oauthlib import OAuth1

    >>> url = 'https://api.twitter.com/1.1/account/verify_credentials.json'
    >>> auth = OAuth1('YOUR_APP_KEY', 'YOUR_APP_SECRET',
    ...               'USER_OAUTH_TOKEN', 'USER_OAUTH_TOKEN_SECRET')

    >>> requests.get(url, auth=auth)
    <Response [200]>

关于 OAuth 工作流程的更多信息，请参见 `OAuth`_ 官方网站。
关于 requests-oauthlib 的文档和用例，请参见 GitHub 的 `requests_oauthlib`_ 代码库。

OAuth 2 and OpenID Connect Authentication
-----------------------------------------

The ``requests-oauthlib`` library also handles OAuth 2, the authentication mechanism
underpinning OpenID Connect. See the `requests-oauthlib OAuth2 documentation`_ for
details of the various OAuth 2 credential management flows:

* `Web Application Flow`_
* `Mobile Application Flow`_
* `Legacy Application Flow`_
* `Backend Application Flow`_

其他身份认证形式
--------------------

Requests 的设计允许其他形式的身份认证用简易的方式插入其中。开源社区的成员\
时常为更复杂或不那么常用的身份认证形式编写认证处理插件。其中一些最优秀的已被\
收集在 `Requests organization`_ 页面中，包括:

- Kerberos_
- NTLM_

如果你想使用其中任何一种身份认证形式，直接去它们的 GitHub 页面，依照说明进行。

新的身份认证形式
-------------------

如果你找不到所需要的身份认证形式的一个良好实现，你也可以自己实现它。Requests 非常易于添加你\
自己的身份认证形式。

要想自己实现，就从 :class:`AuthBase <requests.auth.AuthBase>` 继承一个子类，并实现 ``__call__()`` 方法：

::

    >>> import requests
    >>> class MyAuth(requests.auth.AuthBase):
    ...     def __call__(self, r):
    ...         # Implement my authentication
    ...         return r
    ...
    >>> url = 'http://httpbin.org/get'
    >>> requests.get(url, auth=MyAuth())
    <Response [200]>

当一个身份认证模块被附加到一个请求上，在设置 request 期间就会调用该模块。因此 ``__call__``
方法必须完成使得身份认证生效的所有事情。一些身份认证形式会额外地添加钩子来提供进一步的功能。

你可以在 `Requests organization`_ 页面的 ``auth.py`` 文件中找到更多示例。

.. _OAuth: http://oauth.net/
.. _requests_oauthlib: https://github.com/requests/requests-oauthlib
.. _requests-oauthlib OAuth2 documentation: http://requests-oauthlib.readthedocs.io/en/latest/oauth2_workflow.html
.. _Web Application Flow: http://requests-oauthlib.readthedocs.io/en/latest/oauth2_workflow.html#web-application-flow
.. _Mobile Application Flow: http://requests-oauthlib.readthedocs.io/en/latest/oauth2_workflow.html#mobile-application-flow
.. _Legacy Application Flow:  http://requests-oauthlib.readthedocs.io/en/latest/oauth2_workflow.html#legacy-application-flow
.. _Backend Application Flow:  http://requests-oauthlib.readthedocs.io/en/latest/oauth2_workflow.html#backend-application-flow
.. _Kerberos: https://github.com/requests/requests-kerberos
.. _NTLM: https://github.com/requests/requests-ntlm
.. _Requests organization: https://github.com/requests

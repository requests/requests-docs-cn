.. _authentication:

身份认证
==============

本篇文档讨论如何配合Requests使用多种身份认证方式。

许多web服务都需要身份认证，并且也有多种不同的认证类型。
以下，我们会从简单到复杂概述Requests中可用的几种身份认证形式。


基本身份认证
------------

许多要求身份认证的web服务都接受HTTP Basic Auth。这是最简单的一种身份认证，并且Requests
对这种认证方式的支持是直接开箱即可用。


以HTTP Basic Auth发送请求非常简单::

    >>> from requests.auth import HTTPBasicAuth
    >>> requests.get('https://api.github.com/user', auth=HTTPBasicAuth('user', 'pass'))
    <Response [200]>


事实上，HTTP Basic Auth如此常见，Requests就提供了一种简写的使用方式::

    >>> requests.get('https://api.github.com/user', auth=('user', 'pass'))
    <Response [200]>


像这样在一个元组中提供认证信息与前一个 ``HTTPBasicAuth`` 例子是完全相同的。


netrc Authentication
~~~~~~~~~~~~~~~~~~~~

If no authentication method is given with the ``auth`` argument, Requests will
attempt to get the authentication credentials for the URL's hostname from the
user's netrc file.

If credentials for the hostname are found, the request is sent with HTTP Basic
Auth.


摘要式身份认证
---------------------

另一种非常流行的HTTP身份认证形式是摘要式身份认证，Requests对它的支持也是开箱即可用的::

    >>> from requests.auth import HTTPDigestAuth
    >>> url = 'http://httpbin.org/digest-auth/auth/user/pass'
    >>> requests.get(url, auth=HTTPDigestAuth('user', 'pass'))
    <Response [200]>


OAuth 1 Authentication
----------------------

A common form of authentication for several web APIs is OAuth. The ``requests-oauthlib``
library allows Requests users to easily make OAuth authenticated requests::

    >>> import requests
    >>> from requests_oauthlib import OAuth1

    >>> url = 'https://api.twitter.com/1.1/account/verify_credentials.json'
    >>> auth = OAuth1('YOUR_APP_KEY', 'YOUR_APP_SECRET',
                      'USER_OAUTH_TOKEN', 'USER_OAUTH_TOKEN_SECRET')

    >>> requests.get(url, auth=auth)
    <Response [200]>

For more information on how to OAuth flow works, please see the official `OAuth`_ website.
For examples and documentation on requests-oauthlib, please see the `requests_oauthlib`_
repository on GitHub


其他身份认证形式
--------------------

Requests被设计成允许其他形式的身份认证非常容易快速地插入其中。开源社区的成员
时常为更复杂或不那么常用的身份认证形式编写认证处理插件。其中一些最优秀的已被
收集在 `Requests organization`_ 页面中，包括:

- Kerberos_
- NTLM_

如果你想使用其中任何一种身份认证形式，直接去它们的GitHub页面，依照说明进行。

新的身份认证形式
-------------------

如果你找不到所需要的身份认证形式的一个良好实现，你也可以自己实现它。Requests非常易于添加你自己的身份认证形式。

要想自己实现，就从 :class:`requests.auth.AuthBase` 继承一个子类，并实现 ``__call__()`` 方法::

    >>> import requests
    >>> class MyAuth(requests.auth.AuthBase):
    ...     def __call__(self, r):
    ...         # Implement my authentication
    ...         return r
    ...
    >>> url = 'http://httpbin.org/get'
    >>> requests.get(url, auth=MyAuth())
    <Response [200]>

当一个身份认证模块被附加到一个请求上，在设置request期间就会调用该模块。因此 ``__call__`` 方法必须完成使得身份认证生效的所有事情。一些身份认证形式会额外地添加钩子来提供进一步的功能。

可以在 `Requests organization`_ 页面的 ``auth.py`` 文件中找到示例。

.. _OAuth: http://oauth.net/
.. _requests_oauthlib: https://github.com/requests/requests-oauthlib
.. _Kerberos: https://github.com/requests/requests-kerberos
.. _NTLM: https://github.com/requests/requests-ntlm
.. _Requests organization: https://github.com/requests



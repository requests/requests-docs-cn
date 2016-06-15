.. _api:

开发接口
===================

.. module:: requests

这部分文档包含了 Requests 所有的接口。对于 Requests 依赖的外部库部分，我们在这里介绍最重要的部分，并提供了规范文档的链接。


主要接口
--------------

Requests所有的功能都可以通过以下 7 个方法访问。
它们全部都会返回一个 :class:`Response <Response>` 对象的实例。

.. autofunction:: request

.. autofunction:: head
.. autofunction:: get
.. autofunction:: post
.. autofunction:: put
.. autofunction:: patch
.. autofunction:: delete


异常
----------

.. autoexception:: requests.RequestException
.. autoexception:: requests.ConnectionError
.. autoexception:: requests.HTTPError
.. autoexception:: requests.URLRequired
.. autoexception:: requests.TooManyRedirects
.. autoexception:: requests.ConnectTimeout
.. autoexception:: requests.ReadTimeout
.. autoexception:: requests.Timeout


请求会话
----------------

.. _sessionapi:

.. autoclass:: Session
   :inherited-members:


低级类
-------------------

.. autoclass:: requests.Request
   :inherited-members:

.. autoclass:: Response
   :inherited-members:


更低级的类
-------------------------

.. autoclass:: requests.PreparedRequest
   :inherited-members:

.. autoclass:: requests.adapters.HTTPAdapter
   :inherited-members:

身份验证
--------------

.. autoclass:: requests.auth.AuthBase
.. autoclass:: requests.auth.HTTPBasicAuth
.. autoclass:: requests.auth.HTTPProxyAuth
.. autoclass:: requests.auth.HTTPDigestAuth



编码
---------

.. autofunction:: requests.utils.get_encodings_from_content
.. autofunction:: requests.utils.get_encoding_from_headers
.. autofunction:: requests.utils.get_unicode_from_response


.. _api-cookies:

Cookie
-------

.. autofunction:: requests.utils.dict_from_cookiejar
.. autofunction:: requests.utils.cookiejar_from_dict
.. autofunction:: requests.utils.add_dict_to_cookiejar

.. autoclass:: requests.cookies.RequestsCookieJar
   :inherited-members:

.. autoclass:: requests.cookies.CookieConflictError
   :inherited-members:



状态吗查询
------------------

.. autoclass:: requests.codes

::

    >>> requests.codes['temporary_redirect']
    307

    >>> requests.codes.teapot
    418

    >>> requests.codes['\o/']
    200



~~~~~~~~~~~~~~~~~~~

.. autoclass:: requests.Request
   :inherited-members:

.. autoclass:: Response
   :inherited-members:



迁移到 1.x
----------------

本节详细介绍 0.x 和 1.x 的主要区别，减少升级带来的一些不便。


API 改变
~~~~~~~~~~~

* ``Response.json`` 现在是可调用的并且不再是响应体的属性。

  ::

      import requests
      r = requests.get('https://github.com/timeline.json')
      r.json()   # This *call* raises an exception if JSON decoding fails

* ``Session`` API 也发生了变化. Sessions 对象不在需要参数了。
  ``Session`` is also now capitalized,但为了向后兼容，它仍然能被小写的 ``session`` 实例化。

  ::

      s = requests.Session()    # formerly, session took parameters
      s.auth = auth
      s.headers.update(headers)
      r = s.get('http://httpbin.org/headers')

* 除了'response'，所有的请求挂钩已被移除。

* 认证助手已经被分解成单独的模块了. 参见
  requests-oauthlib_ and requests-kerberos_.

.. _requests-oauthlib: https://github.com/requests/requests-oauthlib
.. _requests-kerberos: https://github.com/requests/requests-kerberos

* 流请求的参数已从 ``prefetch`` 改变到
  ``stream`` ，并且逻辑也被颠倒。除此之外, ``stream`` 现在对于原始响应读取也是必需的。

  ::

      # in 0.x, passing prefetch=False would accomplish the same thing
      r = requests.get('https://github.com/timeline.json', stream=True)
      for chunk in r.iter_content(8192):
          ...

* requests 方法的``config`` 参数已全部删除。 现在配置这些选项都在 ``Session`` ，比如 keep-alive 和最大数目的重定向。 详细程度选项应当由配置日志来处理。

  ::

      import requests
      import logging

      # Enabling debugging at http.client level (requests->urllib3->http.client)
      # you will see the REQUEST, including HEADERS and DATA, and RESPONSE with HEADERS but without DATA.
      # the only thing missing will be the response.body which is not logged.
      try: # for Python 3
          from http.client import HTTPConnection
      except ImportError:
          from httplib import HTTPConnection
      HTTPConnection.debuglevel = 1

      logging.basicConfig() # you need to initialize logging, otherwise you will not see anything from requests
      logging.getLogger().setLevel(logging.DEBUG)
      requests_log = logging.getLogger("requests.packages.urllib3")
      requests_log.setLevel(logging.DEBUG)
      requests_log.propagate = True

      requests.get('http://httpbin.org/headers')


许可
~~~~~~~~~

有一个关键的与 API 无关的区别是开放许可从 ISC_ 许可 变更到 `Apache 2.0`_ 许可.  Apache 2.0
license 确保了对于requests的贡献也被涵盖在 Apache 2.0 许可内.

.. _ISC: http://opensource.org/licenses/ISC
.. _Apache 2.0: http://opensource.org/licenses/Apache-2.0

Migrating to 2.x
----------------


Compared with the 1.0 release, there were relatively few backwards
incompatible changes, but there are still a few issues to be aware of with
this major release.

For more details on the changes in this release including new APIs, links
to the relevant GitHub issues and some of the bug fixes, read Cory's blog_
on the subject.

.. _blog: http://lukasa.co.uk/2013/09/Requests_20/


API Changes
~~~~~~~~~~~

* There were a couple changes to how Requests handles exceptions.
  ``RequestException`` is now a subclass of ``IOError`` rather than
  ``RuntimeError`` as that more accurately categorizes the type of error.
  In addition, an invalid URL escape sequence now raises a subclass of
  ``RequestException`` rather than a ``ValueError``.

  ::

      requests.get('http://%zz/')   # raises requests.exceptions.InvalidURL

  Lastly, ``httplib.IncompleteRead`` exceptions caused by incorrect chunked
  encoding will now raise a Requests ``ChunkedEncodingError`` instead.

* The proxy API has changed slightly. The scheme for a proxy URL is now
  required.

  ::

      proxies = {
        "http": "10.10.1.10:3128",    # use http://10.10.1.10:3128 instead
      }

      # In requests 1.x, this was legal, in requests 2.x,
      #  this raises requests.exceptions.MissingSchema
      requests.get("http://example.org", proxies=proxies)


Behavioural Changes
~~~~~~~~~~~~~~~~~~~~~~~

* Keys in the ``headers`` dictionary are now native strings on all Python
  versions, i.e. bytestrings on Python 2 and unicode on Python 3. If the
  keys are not native strings (unicode on Python2 or bytestrings on Python 3)
  they will be converted to the native string type assuming UTF-8 encoding.

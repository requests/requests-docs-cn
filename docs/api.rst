.. _api:

开发接口
===================

.. module:: requests

这部分文档包含了 Requests 所有的接口。对于 Requests 依赖的外部库部分，我们在这里介绍最重要\
的部分，并提供了规范文档的链接。


主要接口
--------------

Requests 所有的功能都可以通过以下 7 个方法访问。它们全部都会返回一个
:class:`Response <Response>` 对象的实例。

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


API 变化
~~~~~~~~~~~

* ``Response.json`` 现在是可调用的并且不再是响应体的属性。

  ::

      import requests
      r = requests.get('https://github.com/timeline.json')
      r.json()   # 如果 JSON 解码失败，该调用会引发异常。

* ``Session`` API 也发生了变化. Sessions 对象不再需要参数了。
  ``Session`` 现在是大写的了，但为了向后兼容，它仍然能被小写的 ``session`` 实例化。

  ::

      s = requests.Session()    # 过去会话需要接收参数
      s.auth = auth
      s.headers.update(headers)
      r = s.get('http://httpbin.org/headers')

* 除了'response'，所有的请求挂钩已被移除。

* 认证助手已经被分解成单独的模块了. 参见
  requests-oauthlib_ and requests-kerberos_.

.. _requests-oauthlib: https://github.com/requests/requests-oauthlib
.. _requests-kerberos: https://github.com/requests/requests-kerberos

* 流请求的参数已从 ``prefetch`` 改变到 ``stream`` ，并且逻辑也被颠倒。除此之外，
  ``stream`` 现在对于原始响应读取也是必需的。

  ::

      # 在 0.x 中，传入 prefetch=False 会达到同样的结果
      r = requests.get('https://github.com/timeline.json', stream=True)
      for chunk in r.iter_content(8192):
          ...

* requests 方法的 ``config`` 参数已全部删除。 现在配置这些选项都在 ``Session`` ，比如
  keep-alive 和最大数目的重定向。 详细程度选项应当由配置日志来处理。

  ::

      import requests
      import logging

      # 启用调试于 http.client 级别 (requests->urllib3->http.client)
      # 你将能看到 REQUEST，包括 HEADERS 和 DATA，以及包含 HEADERS 但不包含 DATA 的 RESPONSE。
      # 唯一缺少的是 response.body，它不会被 log 记录。
      try: # for Python 3
          from http.client import HTTPConnection
      except ImportError:
          from httplib import HTTPConnection
      HTTPConnection.debuglevel = 1

      logging.basicConfig() # 初始化 logging，否则不会看到任何 requests 的输出。
      logging.getLogger().setLevel(logging.DEBUG)
      requests_log = logging.getLogger("requests.packages.urllib3")
      requests_log.setLevel(logging.DEBUG)
      requests_log.propagate = True

      requests.get('http://httpbin.org/headers')


许可
~~~~~~~~~

有一个关键的与 API 无关的区别是开放许可从 ISC_ 许可 变更到 `Apache 2.0`_ 许可.  Apache 2.0
license 确保了对于 Requests 的贡献也被涵盖在 Apache 2.0 许可内.

.. _ISC: http://opensource.org/licenses/ISC
.. _Apache 2.0: http://opensource.org/licenses/Apache-2.0

迁移到 2.x
----------------


和 1.0 发布版相比，破坏向后兼容的更改比较少。不过在这个主发布版本中，依然还有一些应该注意的问题。

更多关于变更的细节，包括 API，以及相关的 GitHub Issue 和部分 bug 修复，请参阅 Cory blog_
中的相关主题。

.. _blog: http://lukasa.co.uk/2013/09/Requests_20/


API 变化
~~~~~~~~~~~

* Requests 处理异常的行为有部分更改。 ``RequestException`` 现在是 ``IOError`` 
  的子类，而非 ``RuntimeError`` 的子类，新的归类更为合理。此外，无效的 URL 
  转义序列现在会引发 ``RequestException`` 的一个子类，而非一个 ``ValueError``\。

  ::

      requests.get('http://%zz/')   # raises requests.exceptions.InvalidURL

  最后， 错误分块导致的 ``httplib.IncompleteRead`` 异常现在变成了 ``ChunkedEncodingError``\。

* 代理 API 略有改动，现在需要提供代理 URL 的 scheme。

  ::

      proxies = {
        "http": "10.10.1.10:3128",    # 需使用 http://10.10.1.10:3128
      }

      # requests 1.x 中有效， requests 2.x 中会引发 requests.exceptions.MissingSchema

      requests.get("http://example.org", proxies=proxies)


行为变化
~~~~~~~~~~~~~~~~~~~~~~~

* ``headers`` 字典中的 key 现在都是原生字符串，在所有版本的 Python
  中都是如此。也就是说，Python 2 中是 bytestring，Python 3 中是 unicode。
  如果 key 不是原声字符串（Python 2 中 unicode，或 Python 3 中 bytestring）
  它们会被以 UTF-8 编码转成原生字符串。

* ``headers`` 字典中的 value 应该都是字符串。在 1.0 版之前该项目就是要求这样做的，只不过\
  最近（v2.11.0之后）这条变成了强制条款。建议在可能的情况下，避免让 header 值使用 unicode 编码。
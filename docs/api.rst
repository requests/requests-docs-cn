.. _api:

开发者接口
===================

.. module:: requests

这部分文档包含了Requests所有的接口。对于Requests依赖的外部库部分，我们介绍
一些比较重要的并提供规范文档的链接。


主要接口
--------------

Requests所有的功能都可以通过以下7个方法访问。
它们全部都会返回 :class:`Response <Response>` 对象的一个实例。

.. autofunction:: request

.. autofunction:: head
.. autofunction:: get
.. autofunction:: post
.. autofunction:: put
.. autofunction:: patch
.. autofunction:: delete


较低级别的类
~~~~~~~~~~~~~~~~~~~

.. autoclass:: requests.Request
   :inherited-members:

.. autoclass:: Response
   :inherited-members:

请求会话
----------------

.. autoclass:: Session
   :inherited-members:


异常
~~~~~~~~~~

.. module:: requests

.. autoexception:: RequestException
.. autoexception:: ConnectionError
.. autoexception:: HTTPError
.. autoexception:: URLRequired
.. autoexception:: TooManyRedirects


状态码查询
~~~~~~~~~~~~~~~~~~

.. autofunction:: requests.codes

::

    >>> requests.codes['temporary_redirect']
    307

    >>> requests.codes.teapot
    418

    >>> requests.codes['\o/']
    200

Cookies
~~~~~~~

.. autofunction:: dict_from_cookiejar
.. autofunction:: cookiejar_from_dict
.. autofunction:: add_dict_to_cookiejar


编码
~~~~~~~~~

.. autofunction:: get_encodings_from_content
.. autofunction:: get_encoding_from_headers
.. autofunction:: get_unicode_from_response
.. autofunction:: decode_gzip


类
~~~~~~~

.. autoclass:: requests.Response
   :inherited-members:

.. autoclass:: requests.Request
   :inherited-members:

.. autoclass:: requests.PreparedRequest
   :inherited-members:

.. _sessionapi:

.. autoclass:: requests.Session
   :inherited-members:


迁移到 1.x
----------------

本节详细介绍0.X和1.x的主要区别，减少升级带来的一些不便。


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
      r.raw.read(10)

* requests 方法的``config`` 参数已全部删除。 现在配置这些选项都在 ``Session`` ，比如 keep-alive 和最大数目的重定向。 详细程度选项应当由配置日志来处理。

  ::

      # Verbosity should now be configured with logging
      my_config = {'verbose': sys.stderr}
      requests.get('http://httpbin.org/headers', config=my_config)  # bad!


许可
~~~~~~~~~

有一个关键的与 API 无关的区别是开放许可从 ISC_ 许可 变更到 `Apache 2.0`_ 许可.  Apache 2.0
license 确保了对于requests的贡献也被涵盖在 Apache
2.0 许可内.

.. _ISC: http://opensource.org/licenses/ISC
.. _Apache 2.0: http://opensource.org/licenses/Apache-2.0


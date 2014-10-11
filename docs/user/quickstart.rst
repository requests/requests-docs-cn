.. _quickstart:

快速上手
==========

.. module:: requests.models

迫不及待了吗？本页内容为如何入门Requests提供了很好的指引。其假设你已经安装了Requests。如果还没有，
去 :ref:`安装 <install>` 一节看看吧。

首先，确认一下：

* Requests :ref:`已安装 <install>`
* Requests是 :ref:`最新的 <updates>`

让我们从一些简单的示例开始吧。

发送请求
----------

使用Requests发送网络请求非常简单。

一开始要导入Requests模块::

    >>> import requests

然后，尝试获取某个网页。本例子中，我们来获取Github的公共时间线 ::

    >>> r = requests.get('https://github.com/timeline.json')

现在，我们有一个名为 ``r`` 的 :class:`Response <requests.Response>` 对象。可以从这个对象中获取所有我们想要的信息。

Requests简便的API意味着所有HTTP请求类型都是显而易见的。例如，你可以这样发送一个HTTP POST请求::

    >>> r = requests.post("http://httpbin.org/post")

漂亮，对吧？那么其他HTTP请求类型：PUT， DELETE， HEAD以及OPTIONS又是如何的呢？都是一样的简单::

    >>> r = requests.put("http://httpbin.org/put")
    >>> r = requests.delete("http://httpbin.org/delete")
    >>> r = requests.head("http://httpbin.org/get")
    >>> r = requests.options("http://httpbin.org/get")

都很不错吧，但这也仅是Requests的冰山一角呢。

为URL传递参数
-------------------

你也许经常想为URL的查询字符串(query string)传递某种数据。如果你是手工构建URL，那么数据会以键/值
对的形式置于URL中，跟在一个问号的后面。例如， ``httpbin.org/get?key=val`` 。
Requests允许你使用 ``params`` 关键字参数，以一个字典来提供这些参数。举例来说，如果你想传递
``key1=value1`` 和 ``key2=value2`` 到 ``httpbin.org/get`` ，那么你可以使用如下代码::

    >>> payload = {'key1': 'value1', 'key2': 'value2'}
    >>> r = requests.get("http://httpbin.org/get", params=payload)

通过打印输出该URL，你能看到URL已被正确编码::

    >>> print(r.url)
    http://httpbin.org/get?key2=value2&key1=value1

注意字典里值为 ``None`` 的键都不会被添加到 URL 的查询字符串里。

响应内容
--------------

我们能读取服务器响应的内容。再次以Github时间线为例::

    >>> import requests
    >>> r = requests.get('https://github.com/timeline.json')
    >>> r.text
    u'[{"repository":{"open_issues":0,"url":"https://github.com/...

Requests会自动解码来自服务器的内容。大多数unicode字符集都能被无缝地解码。

请求发出后，Requests会基于HTTP头部对响应的编码作出有根据的推测。当你访问 ``r.text``
之时，Requests会使用其推测的文本编码。你可以找出Requests使用了什么编码，并且能够使用
``r.encoding`` 属性来改变它::

    >>> r.encoding
    'utf-8'
    >>> r.encoding = 'ISO-8859-1'

如果你改变了编码，每当你访问 ``r.text`` ，Request都将会使用 ``r.encoding`` 的新值。你可能希望在使用特殊逻辑计算出文本的编码的情况下来修改编码。比如 HTTP 和 XML 自身可以指定编码。这样的话，你应该使用 ``r.content`` 来找到编码，然后设置 ``r.encoding`` 为相应的编码。这样就能使用正确的编码解析 ``r.text`` 了。

在你需要的情况下，Requests也可以使用定制的编码。如果你创建了自己的编码，并使用
``codecs`` 模块进行注册，你就可以轻松地使用这个解码器名称作为 ``r.encoding`` 的值，
然后由Requests来为你处理编码。


二进制响应内容
-------------------

你也能以字节的方式访问请求响应体，对于非文本请求::

    >>> r.content
    b'[{"repository":{"open_issues":0,"url":"https://github.com/...
   
Requests会自动为你解码 ``gzip`` 和 ``deflate`` 传输编码的响应数据。

例如，以请求返回的二进制数据创建一张图片，你可以使用如下代码::

    >>> from PIL import Image
    >>> from StringIO import StringIO
    >>> i = Image.open(StringIO(r.content))


JSON响应内容
---------------

Requests中也有一个内置的JSON解码器，助你处理JSON数据::

    >>> import requests
    >>> r = requests.get('https://github.com/timeline.json')
    >>> r.json()
    [{u'repository': {u'open_issues': 0, u'url': 'https://github.com/...

如果JSON解码失败， ``r.json`` 就会抛出一个异常。例如，相应内容是 401 (Unauthorized) ，尝试访问 ``r.json`` 将会抛出 ``ValueError:
No JSON object could be decoded`` 异常。


原始响应内容
----------------

在罕见的情况下你可能想获取来自服务器的原始套接字响应，那么你可以访问 ``r.raw`` 。
如果你确实想这么干，那请你确保在初始请求中设置了 ``stream=True`` 。具体的你可以这么做::

    >>> r = requests.get('https://github.com/timeline.json', stream=True)
    >>> r.raw
    <requests.packages.urllib3.response.HTTPResponse object at 0x101194810>
    >>> r.raw.read(10)
    '\x1f\x8b\x08\x00\x00\x00\x00\x00\x00\x03'

但一般情况下，你应该下面的模式将文本流保存到文件::

    with open(filename, 'wb') as fd:
        for chunk in r.iter_content(chunk_size):
            fd.write(chunk)

使用 ``Response.iter_content`` 将会处理大量你直接使用 ``Response.raw`` 不得不处理的。
当流下载时，上面是优先推荐的获取内容方式。

定制请求头
-------------

如果你想为请求添加HTTP头部，只要简单地传递一个 ``dict`` 给 ``headers`` 参数就可以了。

例如，在前一个示例中我们没有指定content-type::

    >>> import json
    >>> url = 'https://api.github.com/some/endpoint'
    >>> payload = {'some': 'data'}
    >>> headers = {'content-type': 'application/json'}

    >>> r = requests.post(url, data=json.dumps(payload), headers=headers)


更加复杂的POST请求
----------------------

通常，你想要发送一些编码为表单形式的数据---非常像一个HTML表单。
要实现这个，只需简单地传递一个字典给 `data` 参数。你的数据字典
在发出请求时会自动编码为表单形式::

    >>> payload = {'key1': 'value1', 'key2': 'value2'}
    >>> r = requests.post("http://httpbin.org/post", data=payload)
    >>> print r.text
    {
      ...
      "form": {
        "key2": "value2",
        "key1": "value1"
      },
      ...
    }

很多时候你想要发送的数据并非编码为表单形式的。如果你传递一个 ``string`` 而不是一个 ``dict`` ，那么数据会被直接发布出去。

例如，Github API v3接受编码为JSON的POST/PATCH数据::

    >>> import json
    >>> url = 'https://api.github.com/some/endpoint'
    >>> payload = {'some': 'data'}

    >>> r = requests.post(url, data=json.dumps(payload))


POST一个多部分编码(Multipart-Encoded)的文件
---------------------------------------------

Requests使得上传多部分编码文件变得很简单::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': open('report.xls', 'rb')}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "<censored...binary...data>"
      },
      ...
    }

你可以显式地设置文件名，文件类型和请求头::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': ('report.xls', open('report.xls', 'rb'), 'application/vnd.ms-excel', {'Expires': '0'})}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "<censored...binary...data>"
      },
      ...
    }

如果你想，你也可以发送作为文件来接收的字符串::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': ('report.csv', 'some,data,to,send\nanother,row,to,send\n')}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "some,data,to,send\\nanother,row,to,send\\n"
      },
      ...
    }

如果你发送一个非常大的文件作为 ``multipart/form-data`` 请求，你可能希望流请求(?)。默认下 ``requests`` 不支持, 但有个第三方包支持 -
``requests-toolbelt``. 你可以阅读 `toolbelt 文档
<https://toolbelt.rtfd.org>`_ 来了解使用方法。

在一个请求中发送多文件参考 :ref:`高级用法 <advanced>`
一节.

响应状态码
--------------

我们可以检测响应状态码::

    >>> r = requests.get('http://httpbin.org/get')
    >>> r.status_code
    200

为方便引用，Requests还附带了一个内置的状态码查询对象::

    >>> r.status_code == requests.codes.ok
    True

如果发送了一个失败请求(非200响应)，我们可以通过 :class:`Response.raise_for_status()`
来抛出异常::

    >>> bad_r = requests.get('http://httpbin.org/status/404')
    >>> bad_r.status_code
    404

    >>> bad_r.raise_for_status()
    Traceback (most recent call last):
      File "requests/models.py", line 832, in raise_for_status
        raise http_error
    requests.exceptions.HTTPError: 404 Client Error

但是，由于我们的例子中 ``r`` 的 ``status_code`` 是 ``200`` ，当我们调用
``raise_for_status()`` 时，得到的是::

    >>> r.raise_for_status()
    None

一切都挺和谐哈。


响应头
----------

我们可以查看以一个Python字典形式展示的服务器响应头::

    >>> r.headers
    {
        'content-encoding': 'gzip',
        'transfer-encoding': 'chunked',
        'connection': 'close',
        'server': 'nginx/1.0.4',
        'x-runtime': '148ms',
        'etag': '"e1ca502697e5c9317743dc078f67693f"',
        'content-type': 'application/json'
    }

但是这个字典比较特殊：它是仅为HTTP头部而生的。根据 `RFC 2616 <http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html>`_ ，
HTTP头部是大小写不敏感的。

因此，我们可以使用任意大写形式来访问这些响应头字段::

    >>> r.headers['Content-Type']
    'application/json'

    >>> r.headers.get('content-type')
    'application/json'


Cookies
---------

如果某个响应中包含一些Cookie，你可以快速访问它们::

    >>> url = 'http://example.com/some/cookie/setting/url'
    >>> r = requests.get(url)

    >>> r.cookies['example_cookie_name']
    'example_cookie_value'

要想发送你的cookies到服务器，可以使用 ``cookies`` 参数::

    >>> url = 'http://httpbin.org/cookies'
    >>> cookies = dict(cookies_are='working')

    >>> r = requests.get(url, cookies=cookies)
    >>> r.text
    '{"cookies": {"cookies_are": "working"}}'


重定向与请求历史
-------------------

默认情况下，除了 HEAD, Requests会自动处理所有重定向。

可以使用响应对象的 ``history`` 方法来追踪重定向。

:class:`Response.history <requests.Response.history>` 是一个:class:`Response <requests.Response>` 对象的列表，为了完成请求而创建了这些对象。这个对象列表按照从最老到最近的请求进行排序。

例如，Github将所有的HTTP请求重定向到HTTPS。::

    >>> r = requests.get('http://github.com')
    >>> r.url
    'https://github.com/'
    >>> r.status_code
    200
    >>> r.history
    [<Response [301]>]



如果你使用的是GET, OPTIONS, POST, PUT, PATCH 或者 DELETE,，那么你可以通过 ``allow_redirects`` 参数禁用重定向处理::

    >>> r = requests.get('http://github.com', allow_redirects=False)
    >>> r.status_code
    301
    >>> r.history
    []

如果你使用的是HEAD，你也可以启用重定向::

    >>> r = requests.head('http://github.com', allow_redirects=True)
    >>> r.url
    'https://github.com/'
    >>> r.history
    [<Response [301]>]


超时
--------

你可以告诉requests在经过以 ``timeout`` 参数设定的秒数时间之后停止等待响应::

    >>> requests.get('http://github.com', timeout=0.001)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    requests.exceptions.Timeout: HTTPConnectionPool(host='github.com', port=80): Request timed out. (timeout=0.001)


.. admonition:: 注:

    ``timeout`` 仅对连接过程有效，与响应体的下载无关。
    ``timeout`` is not a time limit on the entire response download;
    rather, an exception is raised if the server has not issued a
    response for ``timeout`` seconds (more precisely, if no bytes have been
    received on the underlying socket for ``timeout`` seconds).

错误与异常
--------------

遇到网络问题（如：DNS查询失败、拒绝连接等）时，Requests会抛出一个 :class:`~requests.exceptions.ConnectionError` 异常。

遇到罕见的无效HTTP响应时，Requests则会抛出一个 :class:`~requests.exceptions.HTTPError` 异常。

若请求超时，则抛出一个 :class:`~requests.exceptions.Timeout` 异常。

若请求超过了设定的最大重定向次数，则会抛出一个 :class:`~requests.exceptions.TooManyRedirects` 异常。

所有Requests显式抛出的异常都继承自 :class:`requests.exceptions.RequestException` 。

-----------------------

准备好学习更多内容了吗？去 :ref:`高级用法 <advanced>` 一节看看吧。

.. _faq:

常见问题
==========================

这部分的文档回答了有关 Requests 的常见问题。

已编码的数据?
-------------

Requests 自动解压缩的 gzip 编码的响应体，并在可能的情况下尽可能的将响应内容解码为 unicode.

如果需要的话，你可以直接访问原始响应内容（甚至是套接字）。


自定义 User-Agent?
-------------------

Requests 允许你使用其它的 HTTP Header 来轻松的覆盖自带的 User-Agent 字符串。


怎么不使用 Httplib2?
---------------------

Chris Adams 给出了一个很好的总结
`Hacker News <http://news.ycombinator.com/item?id=2884406>`_:

    httplib2 是你应该使用 Request 的一个原因，尽管 httplib2 名声在外，但它文档欠佳，\
    而且基本操作要写的代码依旧太多。对于 httplib2 我是很感激的，要写一个现代 HTTP 客户端\
    要跟一吨低级麻烦事打交道，实在是件苦差事。但无论如何，还是直接使用 Requests 吧。\
    Kenneth Reitz 是一个很负责的作者，他能把简单的东西做简单。httplib2 感觉像是一个\
    学术产物，而 Requests 才真的是一个人们可以在生产系统中使用的东西。[1]

    免责声明：尽管我名列在 Requests 的 AUTHORS 文件中，但对于 Requests 的优秀状态，\
    我的贡献大约只有 0.0001% 吧。

    1. http://code.google.com/p/httplib2/issues/detail?id=96 是一个好例子，\
    这个讨厌的 bug 影响了很多人，有人几个月前就写了一个修正，这个修正很有效，我把它放在\
    一个代码分支中，用它处理了几 TB 的数据都没问题，但它花了一年多才进到主分支中，进到
    PyPI 则花了更长时间，所以用到 httplib2 的项目花了很长时间才等到问题的解决。


支持 Python 3 吗?
-----------------

当然！下面是官方支持的python平台列表:

* Python 2.6
* Python 2.7
* Python 3.3
* Python 3.4
* Python 3.5
* PyPy

"hostname doesn't match" 错误是怎么回事？
--------------------------------------------

当 :ref:`SSL certificate verification <verification>` 发现服务器响应的认证\
和它认为自己连接的主机名不匹配时，就会发生这样的错误。如果你确定服务器的 SSL 设置\
是正确的（例如你可以用浏览器访问页面），而且你使用的是 Python 2.6 或者 2.7，那么一个\
可能的解释就是你需要 Server-Name-Indication。


`Server-Name-Indication`_ 简称 SNI，是一个 SSL 的官方扩展，其中客户端会告诉服务器\
它连接了哪个主机名。当服务器使用虚拟主机（ `Virtual Hosting`_）时这点很重要。这样的\
服务器会服务多个 SSL 网站，所以它们需要能够针对客户端连接的主机名返回正确的证书。

Python 3 和 Python 2.7.9+ 的 SSL 模块包含了原生的 SNI 支持。更多关于在 Request、\
SNI 以及 Python < 2.7.9 的信息请参见这个 `Stack Overflow 答案`_\。

.. _`Server-Name-Indication`: https://en.wikipedia.org/wiki/Server_Name_Indication
.. _`virtual hosting`: https://en.wikipedia.org/wiki/Virtual_hosting
.. _`Stack Overflow 答案`: https://stackoverflow.com/questions/18578439/using-requests-with-tls-doesnt-give-sni-support/18579484#18579484

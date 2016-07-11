.. _faq:

常见问题
==========================

这部分的文档回答了有关requests的常见问题。

已编码的数据?
-------------

Requests 自动解压缩的gzip编码的响应体，并在可能的情况下尽可能的将响应内容解码为unicode.

如果需要的话，你可以直接访问原始响应内容（甚至是套接字）。


自定义 User-Agents?
-------------------

Requests 允许你使用其它的HTTP Header来轻松的覆盖自带的User-Agent字符串。


怎么不使用 Httplib2?
---------------------

Chris Adams 给出来一个很好的总结
`Hacker News <http://news.ycombinator.com/item?id=2884406>`_:

    httplib2 is part of why you should use requests: it's far more respectable
    as a client but not as well documented and it still takes way too much code
    for basic operations. I appreciate what httplib2 is trying to do, that
    there's a ton of hard low-level annoyances in building a modern HTTP
    client, but really, just use requests instead. Kenneth Reitz is very
    motivated and he gets the degree to which simple things should be simple
    whereas httplib2 feels more like an academic exercise than something
    people should use to build production systems[1].

    Disclosure: I'm listed in the requests AUTHORS file but can claim credit
    for, oh, about 0.0001% of the awesomeness.

    1. http://code.google.com/p/httplib2/issues/detail?id=96 is a good example:
    an annoying bug which affect many people, there was a fix available for
    months, which worked great when I applied it in a fork and pounded a couple
    TB of data through it, but it took over a year to make it into trunk and
    even longer to make it onto PyPI where any other project which required "
    httplib2" would get the working version.


支持 Python 3 吗?
-----------------

当然！下面是官方支持的python平台列表:

* Python 2.6
* Python 2.7
* Python 3.1
* Python 3.2
* Python 3.3
* Python 3.4
* PyPy 1.9
* PyPy 2.2

What are "hostname doesn't match" errors?
-----------------------------------------

These errors occur when :ref:`SSL certificate verification <verification>`
fails to match the certificate the server responds with to the hostname
Requests thinks it's contacting. If you're certain the server's SSL setup is
correct (for example, because you can visit the site with your browser) and
you're using Python 2.6 or 2.7, a possible explanation is that you need
Server-Name-Indication.

`Server-Name-Indication`_, or SNI, is an official extension to SSL where the
client tells the server what hostname it is contacting. This is important
when servers are using `Virtual Hosting`_. When such servers are hosting
more than one SSL site they need to be able to return the appropriate
certificate based on the hostname the client is connecting to.

Python3 and Python 2.7.9+ include native support for SNI in their SSL modules.
For information on using SNI with Requests on Python < 2.7.9 refer to this
`Stack Overflow answer`_.

.. _`Server-Name-Indication`: https://en.wikipedia.org/wiki/Server_Name_Indication
.. _`virtual hosting`: https://en.wikipedia.org/wiki/Virtual_hosting
.. _`Stack Overflow answer`: https://stackoverflow.com/questions/18578439/using-requests-with-tls-doesnt-give-sni-support/18579484#18579484

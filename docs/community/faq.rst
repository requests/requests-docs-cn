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


怎么不是Httplib2?
-----------------

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

恭喜！回答是肯定的。下面是官方支持的python平台列表:

* Python 2.6
* Python 2.7
* Python 3.1
* Python 3.2
* Python 3.3
* PyPy 1.9

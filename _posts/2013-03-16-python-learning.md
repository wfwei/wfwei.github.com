---
layout: post
title: "python learning"
category: posts
---

1. easy_install & pip to install python packages

use `pip uninstall <package>` to remove package
use `pip freeze` to list installed packages

[stackoverflow](http://stackoverflow.com/questions/1231688/how-do-i-remove-packages-installed-with-pythons-easy-install)

2. file encoding etc.

Rather than mess with the encode, decode methods I find it easier to use the open method from the codecs module.

    import codecs
    f = codecs.open("test", "wr", "utf-8")
    var = u'test:\u7b2c1\u9875:\u9664\u4e86\u623f\u8fd8\u662f\u623f'
    f.write(var)
    f.flush()
    f.read()
[stackoverflow](http://stackoverflow.com/questions/491921/unicode-utf8-reading-and-writing-to-files-in-python)

3

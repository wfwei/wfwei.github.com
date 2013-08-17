---
layout: post
title: "python learning"
categories: [programming, python]
---

###Basic Usage
* python -c "import django; print(django.get_version())"

### install numpy on windows
just use [exe installer](http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy)

###easy_install & pip to install python packages

* use `pip uninstall <package>` to remove package

* use `pip freeze` to list installed packages

* [more](http://stackoverflow.com/questions/1231688/how-do-i-remove-packages-installed-with-pythons-easy-install)

###file encoding etc.

Rather than mess with the encode, decode methods I find it easier to use the open method from the codecs module.

    import codecs
    f = codecs.open("test", "wr", "utf-8")
    var = u'test:\u7b2c1\u9875:\u9664\u4e86\u623f\u8fd8\u662f\u623f'
    f.write(var)
    f.flush()
    f.read()

[more](http://stackoverflow.com/questions/491921/unicode-utf8-reading-and-writing-to-files-in-python)

###[语言规范](http://zh-google-styleguide.readthedocs.org/en/latest/google-python-styleguide/python_language_rules/)



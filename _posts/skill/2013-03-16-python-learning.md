---
layout: post
title: "python learning"
categories: [skill, python]
---

###Basic Usage
* python -c "import django; print(django.get_version())"


### Python re模块

>Python通过re模块提供对正则表达式的支持。使用re的一般步骤是先将正则表达式的字符串形式编译为Pattern实例，然后使用Pattern实例处理文本并获得匹配结果（一个Match实例），最后使用Match实例获得信息，进行其他的操作。

####1. 正则表达式语法

[一图胜千言?](/image/python-re.png)

####2. 模块内容

re模块中包含一些变量，方法和一个异常，常用的方法如下：

最基本的是`re.compile(pattern, flats=0)`，该方法将正则表达式`pattern`编译成`class re.RegexObject`，`RegexObject`对象包括很多方法，包括:

1. 查找方法`search(string [, start_pos [, end_pos]])`，在`string`中查找和正则表达式匹配的子串，找到了返回**一个**`MatchObject`，否则返回`None`
2. 匹配方法`match(string, [, start_pos [, end_pos]])`，匹配`string`的**开头**是否匹配正则表达式，如果匹配，返回这个`MatchObject`，否则返回`None`
3. 切分方法`split(string, max_split=0)`
4. 查找所有方法`findall(string [, start_pos [, end_pos]])`，以列表的形式，返回在`string`中匹配到的所有非重合子串
5. 迭代查找所有方法`finditer(string, start_pos [, end_pos]]`, Return an iterator yielding MatchObject instances over all non-overlapping matches for the RE pattern in string. 
6. 替换方法`sub(replacement, string, count=0)`，将正则表达式在`string`中匹配到的非重复子串替换为`replacement`并返回，`cont`选项设置最大替换个数，默认0表示不限制
7. 替换方法`subn(replacement, string, count=0)，和`sub`类似，只是返回一个tuple(new_string, replace_count)

上面提到的这些方法，都在re包中有对应的静态方法：

1. `re.search(pattern, string, flags=0)`
2. `re.search(pattern, string, flags=0)`
3. `re.match(pattern, string, flags=0)`
4. `re.split(pattern, string, max_split=0, flags=0)`
5. `re.findall(pattern, string, flags=0)`
6. `re.finditer(pattern, string, flags=0)`
7. `re.sub(pattern, repl, string, count=0, flags=0)`
8. `re.subn(pattern, repl, string, count=0, flags=0)`

re包还有一些其他方法：

1. `re.escape(string)`， 对`string`中的非字母数字进行转移(前面加上反斜杠)
2. `re.purge()`，清空re的缓存


发现re包中的方法都带有`flag`，这些参数有来配置各种选项，常用的`flag`选项有：

1. `re.DEBUG` 这样会输出一些debug的信息
2. `re.I` i.e. `re.IGNORECASE` 忽略大小写
3. `re.L` i.e. `re.LOCALE` 本地化，使`\w, \W, \b, \B, \s and \S`匹配当前语言环境的字符
4. `re.M` i.e. `re.MULTILINE` 使`^,$`分别匹配每行的开始和结束
5. `re.S` i.e. `re.DOTALL` 使`.`匹配任意字符，默认情况下，`.`匹配除换行符外的字符
6. `re.U` i.e. `re.UNICODE` 使`\w, \W, \b, \B, \d, \D, \s and \S ` dependent on the Unicode character properties database.
7. `re.X` i.e. `re.VERBOSE` 使正则表达式写起来更优雅，比如默认忽略空格，可以使用`#`添加注释

说到`MatchObject`对象，其保存了匹配结果

最重要的一个方法是`group([group1, group2,...])`，其中的参数有多种形式，直接是捕获组的ID，如`group(2)`，也可以是几个组ID，`group(1,3)`，如果有命名捕获组，可以是捕获组的名字，`group("name")`

还有一个常用方法是`groups([default])`，用来把所有的捕获组放到tuple中输出

参考

1. [官方文档很好的](http://docs.python.org/2/library/re.html)


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

TODO


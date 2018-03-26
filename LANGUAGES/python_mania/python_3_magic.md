### Get args in specific position and pack the rest
    >>> a, *b, c, d = range(10)
    >>> a
    0
    >>> b
    [1, 2, 3, 4, 5, 6, 7]
    >>> c
    8
    >>> d
    9

### Return the last value if a seires a value is all True, otherwise get the first value equavilent to False
    rc = 1 and 2 and 3
    >>> rc
    3
    rc2 = 'aa' and False and 0
    >>> rc2
False


### Else if in single expression

**List Comprehension**

    >>> [i if i < 0 else (i, 2) if i%2 == 0 else "good" if 1 < i < 10 else 'bad' for i in range(10)]
    [(0, 2), 'bad', (2, 2), 'good', (4, 2), 'good', (6, 2), 'good', (8, 2), 'good']

**lambda function**

    complicate_lambda_func = lambda *args, **kwargs: \
        "No message" if len(args) + len(kwargs) < 2  \
        else "Yes, sir!" if ('id' in kwargs and kwargs['id'] == 'General') \
        else "There is no door for you" if 'alibaba' in args \
        else "It's already long enough, let's top here"

    >>> complicate_lambda_func(1,2,3)
    "It's already long enough, let's top here"
    >>> complicate_lambda_func(command='fuck me', id='General')
    'Yes, sir!'
    >>> complicate_lambda_func("老乡", "开门啊", "我是", 'alibaba')
    'There is no door for you'

看完上面两个例子，我越发觉得elif是源自于 `else value if conditon` 的表达方式了。  
不过与elif不同的时候，上述例子的`else if`表达式最后一定得跟一个单纯的`else`语，不然报错在等着你！


### Unpacking Argument Lists
在python里你可以使用`*`来给函数定义可接受任意个参数。同时你也可以用`*`来将一个iterator unpack成一系列args传给函数!  
str, list, tuple, dict都可以使用这个技术，不过dict出来的参数顺序可能是乱的，因为hash的特性。  

    def print_args(*args):
        print " -- ".join(map(str, args))

    # Str
    >>> print_args(*"Monster")
    M -- o -- n -- s -- t -- e -- r

    # List
    >>> print_args(*range(5))
    0 -- 1 -- 2 -- 3 -- 4

    # Tuple
    >>> print_args(*("Harry Poter", "Princess White"))
    Harry Poter -- Princess White

    # Dict
    >>> print_args(*{"a":1, "b":2, "c":3, "d":4})
    a -- c -- b -- d

### Format String with dictionray variables

    my_dict = {
        'my_name': 'Z',
        'my_age': 3,
        'my_toy': (set([1,2,3]), {1:2, 3:4}, [5,6,7])
    }

    print('my name is {my_name}, my age is {my_age}, my toy is {my_toy}'.format(**my_dict))

    my name is Z, my age is 3, my toy is ({1, 2, 3}, {1: 2, 3: 4}, [5, 6, 7])


format支持`*args`和`**kwargs`, 也就是说你可以用`"{0}, {1}, x".format(a, b, x=val)`来format args 和 kwargs. 最前面部分的0和1分别代表第1个argument和第二个argument. 你甚至可以用`'{0.attr_name}'.format(arg1)`来调用第一个argument的属性。

如：
    In []: '{0.denominator}'.format(-1)
    Out[]: '1'

    In []: '{1.count}'.format(0, 'sth')
    Out[]: '<built-in method count of str object at 0x107821ab0>'

### 对原函数/类方法进行inspect/审查的decorator

    import random

    def function_inspector(func):
        def new_func(*args, **kwargs):
            Lucky_number = [6, 7, 8, 9]
            result = func(*args, **kwargs)
            if type(result) == int and result in Lucky_number:
                return result, "You are Lucky"
            else:
                return result
        return new_func

    @function_inspector
    def lottery():
         return random.choice(range(10))

    >>> lottery()
    3
    >>> lottery()
    (8, 'You are Lucky')


    def method_inspector(method):
        BANNED_WEBSITES = ['google', 'facebook', 'youtube', 'twitter']
        BANNED_WORDS = ['法轮功', '共产党']

        def new_method(self, *args, **kwargs):  # 此处的self参数是为了对类方法进行inspect所加
            if getattr(self, 'name', None) in BANNED_WEBSITES:
                return "404 网站无法访问"
            result = method(self, *args, **kwargs)
            for i in result:
                if type(i) != str:
                    continue
                for banned_word in BANNED_WORDS:
                    if banned_word in i:
                        return "该网站含有不恰当内容"
            return result

        return new_method


### 将url中特殊字符(% + alphanum表示的那些字符)进行转换
    import urllib
    converted_str = urllib.unquote(raw_url_str)

想要把特殊符号再转回成url中的%编码字符，你用
    reconverted_str = urllib.quote(converted_str)


### 使用文件名字符进行from m import *操作
    agent = importlib.import_module(module_name)
    globals().update(vars(agent))


### 将一个python数据转换成json格式
    import json
    converted_json_data = json.dumps(raw_python_data)


### 一行代码反转字典键值对(需要每个值都是独特的才可以）
    inv_map = {v:k for k,v in my_dict.items()}


### 查看当前python进程使用的内存大小
    import resource
    mem_usage = resource.getrusage(resource.RUSAGE_SELF)
    print mem_usage.ru_maxrss

    # OSX 下的单位是byte, 而Linux下的单位是kilobyte
    # 将RUSAGE_SELF 换成 RUSAGE_CHILDREN 来查看子进程的内存占用


### 获得当前文件路径
    import os
    os.path.dirname(os.ptah.abspath(__file__))      # 文件存储路径
    os.getcwd()                                     # 当前所在的执行


### 检测一个对象是否是字符串(包括普通字符和unicode字符)

    isinstance(obj, basestring)

### Log detailed error info without exit python process

    import traceback
    import sys
    try:
        eval('Your Arbitary Code here')
    except:
        print 'This is diy error log'
        print traceback.format_exc()
        print sys.exc_info()


## DATA STRUCTURE ##

#### SET OPERATION ####

    UNION               s | t           #  all elements in s and t, except duplicate
    INTERSECTION        s & t           #  elements that are in s and t
    DIFERENCE           s - t           #  element in s but not in t
    SYMMETRIC_DIFFER    s ^ t           #  equal to (s - t) | (t - s), means that element in t or s that are not appeared in both set


## DATETIME AND TIME


### 获取当前自然日的日期字符串

    from datetime import datetime
    natural_date = str(datetime.now().date())

### 快速将字符串解析成datetime对象
需要先安装py-dateutil

    from dateutil import parser
    datetime_obj = parser.parse(datetime_str, fuzzy=True)

> fuzzy=True会开启模糊模式，忽略一些parser不能识别的字符


### 快速打印格式化的当前年月日和时分秒
    import time
    time.strftime('%F %T')
    # %F 是年月日在time模块里的快捷表示，%T是时分秒的快捷表示, 样例：2015-01-01 00:00:00

---
categories:
  - Python   #注意一定要大写
published: true
title: python环境变量设置
---
# python环境变量设置

## 1. ERROR：未设置环境变量时的错误提示

```
PS C:\Users\asus> python
python : 无法将“python”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径
正确，然后再试一次。
所在位置 行:1 字符: 1
+ python
+ ~~~~~~
    + CategoryInfo          : ObjectNotFound: (python:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

![1565153563586](/assets/images/1565153563586.png)

## 2. 路径的正确设置(也是python安装时的默认设置)

```
正确路径配置：现在的环境变量地址：
##添加这个到系统变量或asus变量中无效
C:\Users\asus\AppData\Local\Programs\Python\Python36\Scripts\  

##添加这个到系统变量或asus变量中有效													    C:\Users\asus\AppData\Local\Programs\Python\Python36\          

​再装一下python3.5版本：
C:\Users\asus\AppData\Local\Programs\Python\Python35\Scripts\
																	C:\Users\asus\AppData\Local\Programs\Python\Python35\
```

![1565153321796](/assets/images/1565153321796.png)

## 3. CORRECT:

###### Python 3.6.8 (tags/v3.6.8:3c6b436a57, Dec 24 2018, 00:16:47) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.



![1565150656478](/assets/images/1565150656478.png)

#### 3.1 将上述正确地址(无\Scripts\)添加后的情况(测试添加在系统变量中，不是asus用户变量中)

Issue：是不是系统变量指的是对所有用户都适用，asus用户变量中则是指只对当前登陆用户有效

#### 这个添加在系统变量中只能添加一个名字命名为path 的路径，不可以两个都添加会覆盖

#### 而添加在asus用户变量中，则可以添加多个

```
PS C:\Users\asus> python
Python 3.6.8 (tags/v3.6.8:3c6b436a57, Dec 24 2018, 00:16:47) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
```

![1565153858993](/assets/images/1565153858993.png)

#### 3.2添加在asus用户变量中测试

```
PS C:\Users\asus> python
Python 3.6.8 (tags/v3.6.8:3c6b436a57, Dec 24 2018, 00:16:47) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

![1565154821330](/assets/images/1565154821330.png)

## 4. 最后恢复原始设置

changelog：

2019-08-06-add-by-C.W.  How to setting python environment

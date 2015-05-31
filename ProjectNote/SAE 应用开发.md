#SAE 应用开发笔记

##项目描述

##SAE本地安装

参考大妈的[42分钟乱入 SAE 手册](http://chaos2.qiniudn.com/sae/build/html/index.html)

安装SAE本地虚拟环境

```
$ git clone http://github.com/SAEPython/saepythondevguide.git
$ sudo python setup.py install
$ dev_server.py --help
Usage: dev_server.py [options]
```
到 dev_server.py --help 这一步提示错误 ``Error: Not an app directory(or any of the parent directories)``. 百度后搜到 [SAE 本地环境报错 python](http://www.cnblogs.com/frikyskicce/p/4099247.html),原因是'产生错误的原因是文件夹下不存在config.yaml',解决办法
是自己用记事本写一个文件.问题解决.



##创建应用
到新浪 sae 后台创建应用,选择 Python2.7 作为开发语言. 生成代码版本,本地的 saecloud 文件夹中也对应生成代码文件夹.


本地新建代码目录(或从 github 上 clone 下来). 然后在代码目录中,用 ``saeclound export [app 名称]``,将**应用目录**从 sae 服务器上复制下来. export 过程中,会要求输入 sae 的安全码,以及 sae 的安全邮箱(注意,不是新浪的帐号).

应用目录复制到本地后,包含两个文件: 一个是config.yaml，是应用的配置文件，另一个是index.wsgi，是应用的入口文件。这两个文件在创建应用的时候就已经生成了. 编辑完成后要上传到服务器,只需要在 **应用目录** 内执行 deploy 命令.



##Bottle 框架

[Bottle](http://bottlepy.org/docs/dev/index.html#)是一个简单高效的遵循 [wsgi](http://baike.baidu.com/view/1660037.htm) 的微型python Web框架。说微型，是因为它只有一个文件，除Python标准库外，它不依赖于任何第三方模块。

为啥需要 web 框架? 简单来说,它可以帮助我们处理 HTTP 请求. Ruby 的世界 Rails 一统江湖，而Python 则是一个百花齐放的世界，各种 micro-framework、framework 不可胜数，[不完全列表](http://wiki.python.org/moin/WebFrameworks)。更多内容可以看看[What is a Web Framework?](http://www.jeffknupp.com/blog/2014/03/03/what-is-a-web-framework),  [ 浅谈Python web框架](http://feilong.me/2011/01/talk-about-python-web-framework),这里不深究.

到这一步我们开始引入了第三方模块. 需要在**应用目录**(不是最外面的 SAE 目录!)里新建 site-packages 文件夹,将使用到的模块安装到里面.

[SAE 的 Python 入门指南](http://sae.sina.com.cn/doc/python/tutorial.html)中给出了 Bottle 框架的基本用法:

```
from bottle import Bottle, run

import sae

app = Bottle()

@app.route('/')
def hello():
    return "Hello, world! - Bottle"

application = sae.create_wsgi_app(app)

```


##配置微信后台

参考 [利用SAE搭建微信公众平台](http://www.nilday.com/%E5%88%A9%E7%94%A8sae%E6%90%AD%E5%BB%BA%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%B9%B3%E5%8F%B0%EF%BC%88%E4%B8%80%EF%BC%89web-py%E5%AE%9E%E7%8E%B0%E7%9A%84sae-hello-world/)

我们想将微信公众号内收到的消息,转发到 SAE 上由 程序处理,首先需要对公众号进行配置.

进入微信公众号后台 - 开发者中心 - 配置项 - 修改服务器配置,需要填写 URL, Token ,EncodingAESKey三项.填写可以参考[官方指南](http://mp.weixin.qq.com/wiki/17/2d4265491f12608cd170a95559800f2d.html).

比如 URL 我填入SAE 的地址: ``http://booksearcher.sinaapp.com/api`` , ``/api``意味着我将微信的请求都转到 /api 下处理,需要写 HTTP get 和 post 请求如何处理.这里超出小白能力,于是free 同学找到了现成的微信机器人框架: WeRoBot.

##WeRoBot 框架

[WeRoBot](https://github.com/whtsky/WeRoBot)是一个微信机器人框架.

将安装好的文件拷到 site-packages 文件夹里面.

一个非常简单的 Hello World 微信机器人，会对收到的所有文本消息回复 Hello World

```
import werobot

robot = werobot.WeRoBot(token='tokenhere')

@robot.text
def hello_world():
    return 'Hello World!'

robot.run()
```
最后一行 ``robot.run()``在这里应去掉,否则会跟 sae 的本地监听冲突. 表现是本地起 sae ``dev_server.py``时,会提示

```
Bottle v0.11.6 server starting up (using AutoServer())...
Listening on http://127.0.0.1:8888/
Hit Ctrl-C to quit.
```

到目前为止, index.wsgi 的代码:

```
\# -*- coding: utf-8 -*-

import os 
import sys
import json 

root = os.path.dirname(__file__)
sys.path.insert(0, os.path.join(root, 'site-packages'))

from bottle import Bottle, request, run, template

import sae

from baymax import robot 

app = Bottle()

@app.route('/')
def hello():
    return "Hello, world! - Bottle"

\# wechat request forward to robot
app.mount('/api', robot.wsgi)

application = sae.create_wsgi_app(app)
```
root 那两行从[这个 demo](https://github.com/whtsky/WeRoBot-SAE-demo/blob/master/robot.py) 抄来.

baymax.py 的代码:

```
from werobot.robot import werobot
\# from werobot.utils  import  to_binary

robot = werobot.WeRoBot(token='kidult00')

@robot.text
def hello_world():
```

本地允许 ok, 但是 deploy 到 sae 后报错:

```
Traceback (most recent call last):
  File "/data1/www/htdocs/783/booksearcher/1/index.wsgi", line 14, in <module>
    from baymax import robot
  File "/data1/www/htdocs/783/booksearcher/1/baymax.py", line 4, in <module>
    robot = werobot.WeRoBot(token='kidult00')
  File "/data1/www/htdocs/783/booksearcher/1/site-packages/werobot/robot.py", line 47, in __init__
    filename=os.path.abspath("werobot_session")
  File "/data1/www/htdocs/783/booksearcher/1/site-packages/werobot/session/filestorage.py", line 20, in __init__
    self.db = dbm.open(filename, "c")
  File "/usr/local/sae/python/lib/python2.7/anydbm.py", line 85, in open
    return mod.open(file, flag, mode)
  File "/usr/local/sae/python/lib/python2.7/dumbdbm.py", line 246, in open
    return _Database(file, mode)
  File "/usr/local/sae/python/lib/python2.7/dumbdbm.py", line 71, in __init__
    with _open(self._datfile, 'w') as f:
IOError: [Errno 13] Permission denied: '/data1/www/htdocs/783/booksearcher/1/werobot_session.dat'
```
Permission denied. 百度后找到[部署到 SAE 后报错的解决办法](https://github.com/whtsky/WeRoBot/issues/44)以及 [使用WeRoBot搭建微信公共平台（一）](http://blog.su-kaiyao.gitpress.org/~posts/Python/2014.04.24-%E4%BD%BF%E7%94%A8WeRoBot%EF%BC%88%E4%B8%80%EF%BC%89.md).在 baymax.py 文件里面加入以下代码:

```
from werobot.session.saekvstorage import SaeKVDBStorage

session_storage = SaeKVDBStorage()

robot = werobot.WeRoBot(token="kidult00", enable_session=True,
                        session_storage=session_storage)
```

完成这些步骤,再回到微信公众号后台,配置好 URL/Token 和EncodingAESKey,启用服务器设置!


##豆瓣帐户登录

[Python client library for Douban APIs (OAuth 2.0)](https://github.com/douban/douban-client)给出了OAuth2.0认证的使用方法.

到豆瓣新建应用后,申请对应的接口(这里申请了公用和读书的接口).

##豆瓣读书接口尝试
按 [开发者流程](http://developers.douban.com/wiki/?title=tutorial) 申请 key ,看到有一项'回调地址'不知道怎么填. [本地开发如何 填写 回调地址？](http://www.douban.com/group/topic/31960332/) 的讨论里提到需要本地配置虚拟域名.这里可以简单处理,直接填 sae 应用地址 + /douban 即可:``http://booksearcher.sinaapp.com/douban``


参考 [豆瓣电影查询助手](https://gist.github.com/Kingson/5149802/)

##WeRoBot回复消息

robot.filter —— 回应有指定文本的消息([说明](https://werobot.readthedocs.org/en/latest/handlers.html#id1))


##了解 API 开发
快速查看了Codecademy 课程 [WePay API (Python)](http://www.codecademy.com/en/tracks/wepay_python)

The Four Verbs
The number of HTTP methods you'll use is quite small—there are just four HTTP "verbs" you'll need to know! They are:

- GET: retrieves information from the specified source.
- POST: sends new information to the specified source.
- PUT: updates existing information of the specified source.
- DELETE: removes existing information from the specified source.

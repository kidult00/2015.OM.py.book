#SAE 应用开发笔记

##本地安装

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
到新浪 sae 后台创建应用. 生成代码版本,本地的 saecloud 文件夹中也对应生成代码文件夹.


本地新建代码目录(或从 github 上 clone 下来). 然后在代码目录中,用 saeclound export [app 名称],将**应用目录**从 sae 服务器上复制下来. export 过程中,会要求输入 sae 的安全码,以及 sae 的安全邮箱(注意,不是新浪的帐号).

应用目录复制到本地后,就可以开始编辑了.编辑完成后要上传到服务器,只需要在 **应用目录** 内执行 deploy 命令.

##豆瓣读书接口尝试
按 [开发者流程](http://developers.douban.com/wiki/?title=tutorial) 申请 key ,看到有一项'回调地址'不知道怎么填. [本地开发如何 填写 回调地址？](http://www.douban.com/group/topic/31960332/) 的讨论里提到需要本地配置虚拟域名.


参考 [豆瓣电影查询助手](https://gist.github.com/Kingson/5149802/)


##了解 API 开发
Codecademy 课程 [WePay API (Python)](http://www.codecademy.com/en/tracks/wepay_python)

The Four Verbs
The number of HTTP methods you'll use is quite small—there are just four HTTP "verbs" you'll need to know! They are:

- GET: retrieves information from the specified source.
- POST: sends new information to the specified source.
- PUT: updates existing information of the specified source.
- DELETE: removes existing information from the specified source.
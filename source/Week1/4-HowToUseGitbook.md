##Gitbook

Gitbook是一个开源的自出版平台（听起来有点酷），使用 Git 和 Markdown 写作和制作书籍。

####连接Github帐号

在Gitbook网站帐号设置中完成

####Mac下使用Gitbook

安装Gitbook

	$ npm install gitbook-cli -g

从书籍的SUMMARY.md文件创建目录：

	$ gitbook init

创建gitbook

	git remote add gitbook https://git.gitbook.com/{{username}}/{{bookname}}.git

推送更新

	git push -u gitbook master


####Gitbook写作

GitBook 至少包含 README 和 SUMMARY

- README: 书籍介绍
- SUMMARY: 章节提纲
- GLOSSARY: 术语表

提纲的格式：

	# Summary

	* [Chapter 1](chapter1.md)
	* [Chapter 2](chapter2.md)
	* [Chapter 3](chapter3.md)
	
	# Summary

	* [Part I](part1/README.md)
    	* [Writing is nice](part1/writing.md)
	    * [GitBook is nice](part1/gitbook.md)
	* [Part II](part2/README.md)
    	* [We love feedback](part2/feedback_please.md)
	    * [Better tools for authors](part2/better_tools.md)

[How to use Gitbook](https://github.com/GitbookIO/gitbook#how-to-use-it)

[Gitbook official help](http://help.gitbook.com/index.html)
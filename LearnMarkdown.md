
# Markdown是什么



#为什么用 Markdown

使用Markdown标记语法进行写作有几个好处：

* 不会为了专门调整格式而打断思路
* 跨平台，在支持markdown格式的编辑器、网页、工具里都显示良好 

#如何使用 Markdown

##标题

	# 一级标题
	## 二级标题
	### 三级标题
	#### 四级标题
	##### 五级标题
	###### 六级标题

##强调

* 斜体：``*``文字``*`` 或 ``_``文字``_``， 效果：*斜体* 。
* 粗体：``**``文字``**`` 或 ``__``文字``__``， 效果：__粗体__ 。
* 删除线：``~~``文字``~~`` ， 效果：~~删除线~~ 。


##列表

有序列表 ``1.`` 、``2.`` ……

1. 第一个列表项
2. 第二个列表项
3. 第三个列表项

无序列表 ``* `` / ``-`` / ``+``

* *号列表效果
- -号列表效果
+ +号列表效果

##链接

``[链接名字](链接地址)``

	[I'm an inline-style link](https://www.google.com)
	[I'm an inline-style link with title](https://www.google.com "Google's Homepage")
	[I'm a reference-style link][Arbitrary case-insensitive reference text]
	[You can use numbers for reference-style link definitions][1]

	下面这两行是上面链接定义，文章中不会显示
	[arbitrary case-insensitive reference text]: https://www.mozilla.org
	[1]: http://slashdot.org
	

[I'm an inline-style link](https://www.google.com)

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

[I'm a reference-style link][Arbitrary case-insensitive reference text]

[You can use numbers for reference-style link definitions][1]

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://www.uegeek.com
<br>

##上标和下标


上标 ``<sup></sup>``，效果<sup>[12]</sup> ；
下标 ``<sub></sub>``，效果<sub>[34]</sub> ；

##代码和语法高亮

行内代码 `` `code` `` 后面继续可以有文字。

代码块用 ``` 和 ``` 包围，或者tab缩进，代码以`；`识别换行。

	```javascript
	var s = "JavaScript syntax highlighting";
	alert(s);
	```

```python
s = "Python syntax highlighting"
print s
```

##表格


Colons can be used to align columns.

	| 表格       | Are        | Cool  |
	| --------- |:----------:| -----:|
	| 第一行     | 第一行第二列 | $1600 |
	| 第一列     | 第二行第二列 |   $12 |
	| 啦啦啦 	| 哈哈哈      |    $1 |
	
效果：
	
| 表格       | Are        | Cool  |
| --------- |:----------:| -----:|
| 第一行     | 第一行第二列 | $1600 |
| 第一列     | 第二行第二列 |   $12 |
| 啦啦啦 	| 哈哈哈      |    $1 |

简化：

	Markdown | Less | Pretty
	--- | --- | ---
	*Still* | `renders` | **nicely**
	1 | 2 | 3
	
效果：

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

##引用Blockquotes

> 从前
> 有座山.

另一段引用

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.
Inline HTML


##线Horizontal Rule



三个减号``---``

---

三个星号``***``

***

三个下划线``___``

___


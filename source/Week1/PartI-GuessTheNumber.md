#Interactive Phthon笔记 
#Introduction

> Beginner does not mean easy.

看视频只是帮助了解概念、知道要做什么事情，真正的学习是***各种姿势尝试使用***所学知识。如果觉得直接上手mini project 太难，可以从 practice exercise 开始。


#类型 type
``print type()`` 可以输出类型

py2和py3的除法规则不相同

###变量
为什么要用变量？

- 效率：命名比一串复杂数字更便于回忆和书写，用来代替复杂计算
- 可读性：使程序更易理解

single equal is assignment, double equal is equality testing.
单个等号用于赋值，双等号用于判定是否相等

# Function 函数

定义好之后以后执行的一段代码。

写一段函数：
1. 明确函数的作用，写在注释中
2. ``def name(varible1，varible2):``头部，用于定义；冒号表示一段代码开始，接下来有同样缩进的代码就是body部分。定义函数之后还需要调用函数。

函数就像一个代理，服务商，完成你想做的事情，只需要你提供需要加工的东西（变量值），然后在函数内部鼓捣一阵，交给你结果。

python的条件语句
	
	if() :
		codeblock
	elif :
		codeblock
	else :
		codeblock

#Event-driven programming
![](/Users/kidult/Downloads/Snip20150325_2.png)

事件

- Input
- Keyboard
- Mouse
- Timer

每个事件有handler。注册处理程序handler

###Event queue
事件队列机制

#Global variable & Local variable 

在函数内定义的都是局部变量.
何时用哪种？优先使用局部变量，这样不会影响全局。
py在函数内将变量自动识别未局部变量。那么怎样修改全局变量？声明全局变量：

	def func1:
		global num
		num = 5

#Program Structure

- Globals (state)
- Helper functions
- Classes
- Define event handlers
- create a frame
- Register event handlers
- Start frame & timers

#Guess the numnber 折腾过程
按照Mini-project development process：

1. 创建 frame 和 input field
	
	查找 doc 中 Graphic Modules 的 Frame 和 Control Objects 的说明，创建成功后(包括input的定义)运行可见对话框和输入框；
	
	问题：input的label不支持中文

2. event handler input_guess(guess)

	转换输入框中的字符串为数字（整型），然后 print 出来

3. 初始化全局变量 secret_number

	global secret_number
    secret_number = random.randrange(0, 100)

4. input_guess 函数比较大小输出结果

		def input_guess(guess):
	    	global secret_number
	    	number_input = int(guess)
	    	print "Your guess was: ",number_input

	   		if secret_number > number_input :
    	    	print "Higher!"
    		elif secret_number < number_input :
        		print "Lower!"
    		else:
        		print "\(^o^)/~ Bingo!!"
  
5. 修改引导语和ui

6. 增加输入不为整数时的判断和提示 
	 
	遇到一个坑：在函数内部，如果要判断传入的 str 是否可以转为 int，是先转后判断，还是先判断后转？陷入了一个死循环。后来找到了 ``isdigit()``函数，可以直接判断是否是数字，于是在转换成int之前，先判断是否为数字字符串：

		if guess.isdigit():
        	number_input = int(guess)
        	print "Your guess was: ",number_input
        
        	if secret_number > number_input :
            	print "Nope!Higher!"
            	print ""
	        elif secret_number < number_input :
    	        print "Nope!Lower!"
        	    print ""
        	else:
            	print "\(^o^)/~ Bingo!!"
            	print ""
            	
    	else:    
        	print "Please input an integer!"
    	

	问题
	google 搜索 怎么判断输入类型是数字，然后在doc中找到Test String for Digits




#其他：Codeskulptor打开不
网站打不开，vpn翻墙后还是打不开。搜索到相关讨论：
[Codeskulptor 的保存/载入方法](http://mooc.guokr.com/note/4760)。用[这个链接可以上传py文件](http://codeskulptor.appspot.com/save/)，但依然打不开网站。看了果壳上的讨论[http://www.codeskulptor.org/这个打开的好慢啊！！你们是不是这样的啊？](http://mooc.guokr.com/discussion/4389/)似乎chrome的插件会有问题，但试了ff还是不行。

[Troubleshooting saving and loading issues for CodeSkulptor](https://class.coursera.org/interactivepython1-002/forum/thread?thread_id=9)

1. Please read the FAQ. It includes an excellent summary of the common issues which interfere with CodeSkulptor retrieving files and/or saving files. It also has a link for a alternative save service you can try (if the problem is just with saving your work).
2. If you happen to be in China, please note that you may need to use a proxy or VPN. There's internet blocking in China that you'll have to work around. Jobin (a CTA who lives in China) will post a thread concerning this issue.
中国用户需要用代理或vpn

3. You can try using a different browser, different computer on the same network, and/or different network. This won't fix the issues, but MIGHT get you some additional information to work with. (This is still working with the issues listed in the FAQ)
尝试用不同浏览器（已试），不同电脑（台式win7笔记本mac已试）

4. There are some tips about specific settings for some browser plug-ins etc such as HTTPS:Everywhere that you should review.
浏览器插件问题

开启vpn后ok。
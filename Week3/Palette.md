#第三周Miniproject：点彩画板

##一、要求
唯一作业:可回放的点彩画板

- 期待:
    + 三种形状画笔可选: 三角/方形/圆形
    + 颜色可定义: 颜色名 或是 RGB 声明
    + 每次 鼠标 点击画板任意一处,都绘制一个当前画笔可用彩色形状
    + 可记录1024次点彩绘制行为
    + 可回放整个记录的绘制行为
- 要求:
    + 基础: 有画笔,可点绘
    + 可用: 有画笔,有颜色,可点绘
    + 合格: 有画笔,有颜色,可点绘,可回放
    + 天才: 有画笔,有颜色,可点绘,可回放,回放速度可调节,回放可输出为文件

##二、作业地址
- Codesculptor: [点彩画板V1](http://www.codeskulptor.org/#user39_mI9h518rF5_5.py) （[测试版本](http://www.codeskulptor.org/#user39_mI9h518rF5_1.py)）
- [Github 源代码](https://github.com/kidult00/omooc.py/blob/master/src/iippy-2/Palette.py)
- [Github 教程](https://github.com/kidult00/pythoncamp0/blob/master/Week3/Palette.md)
- [Gitbook 教程](http://kidult00.gitbooks.io/kidult-s-python-book/content/Week3/Palette.html)
   
##三、分析
拆解任务，计划按以下顺序逐一攻破：

- 画三种形状 【done】
	+ 圆形怎么画
	+ 三角怎么画
	+ 方形怎么画
- 保存已绘图形 【done】
- 设置形状 【done】
- 设置颜色 【done】
- 设置RGB颜色
- 点击任意一处可绘制
	+ 特殊位置
	+ 重叠位置
- 记录1024次 【done】
- 回放 【done】
	+ 调节速度
- 输出为文件

##四、动手

###1. 准备姿势

[本地运行codeskulptor](https://github.com/OpenMindClub/OMOOC.py/wiki/codeskulptor_in_local)，不成功。先放一边，以后再试。

把课程的代码模板复制过来，帮助理清思路：
	
	import simplegui
	
	# helper function 
	     
	# define event handlers
	def mouseclick(pos):
	    pass
	    
	# create frame and controls
	frame = simplegui.create_frame("Palette", 800, 100)
	
	# register event handlers
	frame.set_mouseclick_handler(mouseclick)
	frame.set_draw_handler(draw)
	
	# get things rolling
	frame.start()
	
	# Always remember to review the grading rubric

###2. 画三种形状 （约1.5小时）
没思路？不必完全从头开始，可以先看[课程栗子](http://www.codeskulptor.org/#examples-mouse_input.py)，复习如何用鼠标画圆。

####（1）画圆形
在[文档](http://www.codeskulptor.org/docs.html#tabs-Python)中找到画圆的语法：

	canvas.draw_circle(center_point, radius, line_width, line_color, fill_color = color)

####（2）画三角形
#####A-思路：

- 有类似画圆一样的函数吗？
- 如果没有，用什么画？
- 是画三条线连起来，还是给三个点的坐标？

空想当然是想不出来的，于是：

#####B-找资料：

- [文档](http://www.codeskulptor.org/docs.html#tabs-Python)里面没有提供专门画三角形的函数；
- 搜索了一些画三角形的方法，都很复杂
- 于是去偷看同学作业：

		pos = [(x, y-n),(x+n,y+n/2),(x-n,y+n/2)]
	说明应该是给三个点的坐标画出三角形。
	
	
这时发现自己对 pos 不理解，于是回看[课程 Mouse input ](https://class.coursera.org/interactivepython2-002/lecture/37)，找到文档中 SimpleGUI Module — Control Objects - Set the Mouse Input Handler 查看：

	def mouse_handler(position):
   		…
    
这里的参数是一对数字（怎么觉得有点坑）。如果只需要用到点坐标，直接用变量读到坐标就能调用了：

	point_pos = list(pos)

但这里需要取到x和y的值，用来计算三角形的三个点的坐标。

#####C-问题：

> **鼠标位置只有两个点的坐标，怎么画出三个点的三角形？**

还是迷迷糊糊，直到画了个图来帮助理解：

![](http://img.hb.aicdn.com/53ca57f3ff22dbeec300a4c2196ed341ba722a295214-HlXq0F)

#####D-解决：

尝试将鼠标的坐标转换为三角形的三个顶点并绘制出来：

	def mouseclick(pos):
	    global circle_pos,triangle_pos
	    circle_pos = list(pos)
	    x = pos[0]
	    y = pos[1]
	    triangle_pos = [[x, y-tri_height],[x+2*tri_height,y+tri_height],[x-2*tri_height,y+tri_height]]

	def draw(canvas):
    	canvas.draw_circle(circle_pos, BALL_RADIUS, 1, "Black")
    	canvas.draw_polygon(triangle_pos, 1, "Black")

但是程序提示出错 undefined。改成分别定义三角形的三个坐标，用draw_polygon可以画出来。	
	canvas.draw_polygon([triangle_1,triangle_2,triangle_3], 1, "Black")

***为什么？？***

####（3）画正方形

原理同三角形

###3.保存已绘图形（约0.5小时）

#####A-思路：

怎样把每一次绘制的位置、形状、颜色保存到一个数组里面？

#####B-找资料：

参考[课程](http://www.codeskulptor.org/#examples-list_of_balls.py)

	def click(pos):
    	ball_list.append(pos)  //记录每个位置

	def draw(canvas):
    	for ball_pos in ball_list:
        	canvas.draw_circle(ball_pos, ball_radius, 1, "Black", ball_color)
  
#####C-解决：

用 .append(pos) 存放每次的坐标位置，然后在draw函数中用for循环将list里面的位置全部绘制出来。

可以先进行圆形/三角形/正方形的判断，然后存入list中：

    shape_list.append(pos)
   
###4.设置形状（约0.75小时）

#####A-思路：

在画布中给出三个形状的按钮，然后定义各自的 event handler。在draw函数中用if判断绘制圆形还是多边形（三角形和正方形）。按这个思路写完会报错，在圆形和多边形切换的时候，问题出在记录的pos进行 2点/3点 的切换。(上面画三角形时的疑问又出来了)

	def mouseclick(pos):
	    global ShapeType,shape_list,Radius
	    
	    x = pos[0]
	    y = pos[1]
	
	    if ShapeType == "circle":
	        pos = [x,y]
	    elif ShapeType == "triangle":
	        pos = [(x, y - Radius),(x + 2*Radius, y + Radius),(x - 2*Radius, y + Radius)]
	    elif ShapeType == "square":
	        pos = [(x - Radius , y - Radius),(x + Radius , y - Radius),(x + Radius , y + Radius),(x - Radius , y + Radius)]
	
	    shape_list.append(pos)

	def draw(canvas):
	    global ShapeType,shape_list,Radius
	    for shape_pos in shape_list:
	        if ShapeType == "circle":
	            canvas.draw_circle(shape_pos,Radius, 1, "Black")
	        else:
	            canvas.draw_polygon(shape_pos, 1, "Black")

报错``TypeError: center must be a 2 element sequence``。

#####B-问题：

猜想是记录每个形状时，仅仅记录了位置信息 ``shape_list.append(pos)``，draw函数里面用来判断形状的是全局变量，这个变量并没有记录到历史中，所以切换形状时，三个点和两个点的情况无法兼容。

解决办法是把形状也记录到历史中。

#####C-找资料&解决：

参考了[同学的作业](http://www.codeskulptor.org/#user39_2cml5Yti4z_1.py)，修改如下：

	def mouseclick(pos):
	    global ShapeType,shape_list,Radius
	    
	    x = pos[0]
	    y = pos[1]
	
	    if ShapeType == "circle":
	        pos = [x,y]
	    elif ShapeType == "triangle":
	        pos = [(x, y - Radius),(x + 2*Radius, y + Radius),(x - 2*Radius, y + Radius)]
	    elif ShapeType == "square":
	        pos = [(x - Radius , y - Radius),(x + Radius , y - Radius),(x + Radius , y + Radius),(x - Radius , y + Radius)]
	
	    print pos
	    shape_list.append[pos,ShapeType]

	def draw(canvas):
	    global ShapeType,shape_list,Radius
	    for shapes in shape_list:
	        if shapes[0] == "circle":
	            canvas.draw_circle(shapes[0],Radius, 1, "Black")
	        else:
	            canvas.draw_polygon(shapes[0], 1, "Black")
 
运行时对这行``shape_list.append[pos,ShapeType]``报错：``TypeError: '<invalid type>' does not support indexing``。看来是用错了[]。改成 () 后报错``TypeError: append() takes exactly 2 arguments (3 given)``。嗯哪，对list还是不太明白，滚去看 [doc]([Types & Operations](http://www.codeskulptor.org/docs.html#tabs-Types): list用[]没错，但append应该用()，append内用()还是[] ?

code | output
--- | --- | ---
a_list = [1, 2, 3] | [1, 2, 3, 4, [5, 6, 7]]
a_list.append(4) |
a_list.append([5, 6, 7]) |
print a_list |

append里面每个记录应该用[]。

修改后继续报错 - - 。检查代码后发现 ``if shapes[0] == "circle":`` 调用错了list中shape的位置，修改0为1。终于把三种图形画出来了。

###5.设置颜色（约0.25小时）

#####A-思路：

思路自然是像坐标一样记录到list中，但是这样写完，运行结果是更换颜色后，所有点的颜色都跟着变了。

	def draw(canvas):
	    global ShapeType,ShapeColor,shape_list,Radius
	    for shapes in shape_list:
	        if shapes[1] == "circle":
	            canvas.draw_circle(shapes[0],Radius, 1, "Black",ShapeColor)
	        else:
	            canvas.draw_polygon(shapes[0], 1, "Black",ShapeColor)

#####B-问题：

问题出在draw函数中，用了全局的ShapeColor，而不是shape_list中的存放的颜色记录。

#####C-解决：

修改成以下代码后文件解决：

	if shapes[1] == "circle":
            canvas.draw_circle(shapes[0],Radius, 1, "Black",shapes[2])
   	else:
            canvas.draw_polygon(shapes[0], 1, "Black",shapes[2])
            
###6.设置RGB颜色


###7.点击任意一处可绘制

特殊位置

重叠位置

###8.记录1024次（约0.25小时）

#####A-思路：

每click（还是draw）一次，计数器+1。

#####B-问题和解决：


    if clickCount < 1025 :
        shape_list.append([pos,ShapeType,ShapeColor])
        clickCount += 1

画布上需要给出当前点击计数，不然不知道点了多少次。。

	canvas.draw_text(clickCount,(10,10),12,"green","Verdana")

错误提示：``TypeError: text must be a string``，又忘记转为字符串了。

	canvas.draw_text(str(clickCount)+"st click" ,(10,10),12,"black")
	
Done.

###9.回放（约3小时）

####A-分析

之前有同学剧透是用timer控制。先去doc里面查了查timer用法：

	Create Timersimplegui.create_timer()
	Start Timertimer.start()
	Stop Timer

不太有思路，找了[例子]([Screensaver](http://www.codeskulptor.org/#examples-timers.py)来看。timer的两个关键要素：间隔和动作。间隔容易处理，那么希望间隔后做什么事情？一次间隔画出记录中的一步。

####折腾1：每步读取历史

#####A-思路：

画出记录意味着从list中读取历史

#####B-问题：

问题来了，既要读取第几步，又要读取这一步的参数，咋整？list是二维的吗？

#####C-找资料：

简单查了一下，list支持二维，尝试写下：

	def replayStep():
	    global clickCount,Radius
	    if clickCount > 0 :
	        if shape_list[clickCount-1][1] == "circle":
	            canvas.draw_circle(shape_list[clickCount],Radius,1,"Black",shape_list[clickCount][2] )
	        else :
	            canvas.draw_polygon(shape_list[clickCount],1,"Black",shape_list[clickCount][2] )
	        clickCount -= 1

提示是 ``NameError: name 'canvas' is not defined``，看来 canvas.draw 不能随便调用。此路不通。

####折腾2：在回放函数中重绘历史
#####A-思路：

又偷看了同学的作业，思路是在回放函数中整个重绘。

之前思路的问题，过于相信程序的强大，想当然地认为回放过的记录会保留在画布中，每次间隔后只绘制下一个点。其实，每次绘制的步骤都是：将需要绘制的点全部绘制一遍。因为有timer的间隔，所以看上去是新增了最近的一个点。于是画布干的事情是**每次间隔后清空整个画布并全量重新绘制**。（又或许确实有优雅的方法只绘制增量？）

再理一下步骤：

- 复制原来的 list
- 画布上清空原来的 list
- 计算回放步数：原来list中有多少个形状？
- 在回放步数范围内，整个重绘：在新 list 中 append 每个形状

#####B-问题和解决：
按上面的思路写出：

		def replayStep():
		    global clickCount,shape_list,history_list
		
		    shape_list = [] #清空
		
		    if step < clickCount :
		        step += 1
		        for history_step in range(0,step):
		                shape_list.append(history_list[history_step])
		    else:
		        step = 0

期间经历的错误包括：

- if语句忘记以 ``:`` 结尾
- 定义了一个局部变量表示历史步骤数，而不是直接使用已经定义的全局变量 clickCount
- 不知道如何写历史步骤 history_step 的 for 语句，学习 range() 后理解错误，写成 ``range(0,history_step)``自循环
- 引入步骤变量 step 后，混淆 step 和 history_step，以致弄错循环对象：shape_list.append(history_list[step])
- step 加1放在了 for 循环后面，导致回放时会少绘制最后一个点

当然，最致命的错误，是没有将step定义为全局变量，导致提示 ``undefined: Error: local variable 'step' referenced before assignment``。开始还尝试定义局部变量解决这个问题，运行结果是一旦开始回放，就把之前绘制的图形清空到只剩下最初的那一个。说明记录并没有被保存。检查了很久没有发现问题，再次偷看同学作业，终于发现全局变量的问题。

将step定义为全局变量后，问题解决。


####调节速度
未实现

###10.输出为文件
未实现

###五、todo

- 在回放过程中增加形状，回放结束会报错

###六、Do it better next time

1. 多动手画逻辑
2. 看别人代码时，首先弄清楚变量和变量之间的关系
3. 将自己分析的逻辑写下来/画出来，否则会有很多模棱两可的东西，大脑经常需要回到前几个节点重新开始
4. 问题是什么？思路是什么？有哪些不确定待查的信息？解决不了，可能的原因是什么？
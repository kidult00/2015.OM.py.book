#要求
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

[作业Codeskulptor源码链接](http://www.codeskulptor.org/#user39_mI9h518rF5_1.py)

http://www.codeskulptor.org/#user39_mI9h518rF5_5.py
   
#分析
实现顺序

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
- 回放 
	+ 调节速度
- 输出为文件

#动手

###1. 准备姿势

[本地运行codeskulptor](https://github.com/OpenMindClub/OMOOC.py/wiki/codeskulptor_in_local)，不成功

代码模板
	
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

###2. 画三种形状
到[课程栗子](http://www.codeskulptor.org/#examples-mouse_input.py)复习如何用鼠标画圆。

####画圆形
[文档](http://www.codeskulptor.org/docs.html#tabs-Python)中找到画圆的语法

	canvas.draw_circle(center_point, radius, line_width, line_color, fill_color = color)

####画三角形
想不出来，网上找画三角形的方法都很复杂，于是偷看同学作业：

	pos = [(x, y-n),(x+n,y+n/2),(x-n,y+n/2)]

发现对pos不理解，于是回看[课程Mouse input](https://class.coursera.org/interactivepython2-002/lecture/37)，找到文档中 SimpleGUI Module — Control Objects - Set the Mouse Input Handler 查看。

	def mouse_handler(position):
    …
    
这里的参数是一对数字（怎么觉得有点坑）。如果只需要用到点坐标，直接用变量读到坐标就能调用了：

	point_pos = list(pos)

但这里需要取到x和y的值，用来计算三角形的三个点的坐标。画个图来帮助理解

![](/Users/ennolin/Pictures/Triangle.png)


为什么提示undefined？

	def mouseclick(pos):
	    global circle_pos,triangle_pos
	    circle_pos = list(pos)
	    x = pos[0]
	    y = pos[1]
	    triangle_pos = [[x, y-tri_height],[x+2*tri_height,y+tri_height],[x-2*tri_height,y+tri_height]]

	def draw(canvas):
    	canvas.draw_circle(circle_pos, BALL_RADIUS, 1, "Black")
    	canvas.draw_polygon(triangle_pos, 1, "Black")

改成分别定义三角形的三个坐标，用draw_polygon可以画出来。***为什么？？***
	
	canvas.draw_polygon([triangle_1,triangle_2,triangle_3], 1, "Black")

####画正方形

原理同三角形

###3.保存已绘图形

参考[课程](http://www.codeskulptor.org/#examples-list_of_balls.py)

	def click(pos):
    	ball_list.append(pos)  //记录每个位置

	def draw(canvas):
    	for ball_pos in ball_list:
        	canvas.draw_circle(ball_pos, ball_radius, 1, "Black", ball_color)
  
用 .append(pos) 存放每次的坐标位置，然后在draw函数中用for循环将list里面的位置全部绘制出来。

可以先进行圆形/三角形/正方形的判断，然后存入list中：

    shape_list.append(pos)
   
###4.设置形状

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

报错``TypeError: center must be a 2 element sequence``。猜想是记录每个形状时，仅仅记录了位置信息 ``shape_list.append(pos)``，draw函数里面用来判断形状的是全局变量，这个变量并没有记录到历史中，所以切换形状时，三个点和两个点的情况无法兼容。解决办法是把形状也记录到历史中。

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
 
运行时对这行``shape_list.append[pos,ShapeType]``报错：``TypeError: '<invalid type>' does not support indexing``。看来是用错了 []。改成 ()后报错``TypeError: append() takes exactly 2 arguments (3 given)``。嗯哪，对list还是不太明白，滚去看 [doc]([Types & Operations](http://www.codeskulptor.org/docs.html#tabs-Types): list用[]没错，但append应该用()，append内用()还是[] ?

code | output
--- | --- | ---
a_list = [1, 2, 3] | [1, 2, 3, 4, [5, 6, 7]]
a_list.append(4) |
a_list.append([5, 6, 7]) |
print a_list |

append里面每个记录应该用[]。修改后继续报错 - - 。检查代码后发现 ``if shapes[0] == "circle":`` 调用错了list中shape的位置，修改0为1。终于把三种图形画出来了。

###5.设置颜色

思路自然是像坐标一样记录到list中，但是这样写完，运行结果是更换颜色后，所有点的颜色都跟着变了。

	def draw(canvas):
	    global ShapeType,ShapeColor,shape_list,Radius
	    for shapes in shape_list:
	        if shapes[1] == "circle":
	            canvas.draw_circle(shapes[0],Radius, 1, "Black",ShapeColor)
	        else:
	            canvas.draw_polygon(shapes[0], 1, "Black",ShapeColor)

问题出在draw函数中，用了全局的ShapeColor，而不是shape_list中的存放的颜色记录。修改成以下代码后文件解决：

	if shapes[1] == "circle":
            canvas.draw_circle(shapes[0],Radius, 1, "Black",shapes[2])
   	else:
            canvas.draw_polygon(shapes[0], 1, "Black",shapes[2])
            
###6.设置RGB颜色


###7.点击任意一处可绘制

特殊位置

重叠位置

###8.记录1024次 

思路：每click（还是draw）一次，计数器+1。

    if clickCount < 1025 :
        shape_list.append([pos,ShapeType,ShapeColor])
        clickCount += 1

画布上需要给出当前点击计数，不然不知道点了多少次。。

	canvas.draw_text(clickCount,(10,10),12,"green","Verdana")

错误提示：``TypeError: text must be a string``，又忘记转为字符串了。

	canvas.draw_text(str(clickCount)+"st click" ,(10,10),12,"black")
	
Done.

###9.回放 

之前有同学剧透是用timer控制。先去doc里面查了查timer用法：

	Create Timersimplegui.create_timer()
	Start Timertimer.start()
	Stop Timer

不太有思路，找了[例子]([Screensaver](http://www.codeskulptor.org/#examples-timers.py)来看。timer的两个关键要素：间隔和动作。间隔容易处理，那么希望间隔后做什么事情？一次间隔画出记录中的一步。

画出记录意味着从list中读取历史，那么问题来了，既要读取第几步，又要读取这一步的参数，咋整？list是二维的吗？简单查了一下，list支持二维，尝试写下：

	def replayStep():
	    global clickCount,Radius
	    if clickCount > 0 :
	        if shape_list[clickCount-1][1] == "circle":
	            canvas.draw_circle(shape_list[clickCount],Radius,1,"Black",shape_list[clickCount][2] )
	        else :
	            canvas.draw_polygon(shape_list[clickCount],1,"Black",shape_list[clickCount][2] )
	        clickCount -= 1

提示是 ``NameError: name 'canvas' is not defined``，看来canvas.draw不能随便调用。

####调节速度

###10.输出为文件
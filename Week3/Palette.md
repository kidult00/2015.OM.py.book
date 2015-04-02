#要求
唯一作业:

- 可回放的点彩画板
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

http://www.codeskulptor.org/#user39_mI9h518rF5_1.py
   
#分析
实现顺序

- 画三种形状
	+ 三角怎么画
	+ 方形怎么画
	+ 圆形怎么画
- 保存已绘图形
- 设置预设的颜色
- 设置RGB颜色
- 点击任意一处可绘制
	+ 特殊位置
	+ 重叠位置
- 记录1024次
- 回放 
	+ 调节速度
	+ 输出为文件

#动手

###1. 准备姿势

[本地运行codeskulptor](https://github.com/OpenMindClub/OMOOC.py/wiki/codeskulptor_in_local)

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
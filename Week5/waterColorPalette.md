#水彩画板
Codeskulptor [地址](http://www.codeskulptor.org/#user39_pgdhZaiK8h_1.py)

##任务拆分

1. 画点
2. 画彩色点
3. 自动消失的点
4. 渐渐消失的点


##画点

```Python

	def draw(canvas):
	    global dots
	    for dot in dots:
	        canvas.draw_circle(dot, 10, 1, "Black", "Black")
	 
	def click(pos):  
	    dots.append(pos)
	    print len(dots),dots
```

##画彩色点
纠结不出
直接写 rgb()，报错 ``global name 'rgb' is not defined`` 。看了栗子，发现是通过定义一个 RGBcolor实现的可用颜色。于是跟着写 RGBcolor 类 和 Dot 类。

##自动消失的点
设定一定条件，让点从 list 中 remove 掉


##渐渐消失的点
给点设定一个生命值，让每一帧生命值减一
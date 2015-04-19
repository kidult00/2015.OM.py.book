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

```Python

# 定义颜色类
class RGBcolor:
    def __init__(self,red,green,blue):
        self.red = red
        self.green = green
        self.blue = blue
        
    def make_html(self):
        return "rgb(" + str(self.red) + "," + str(self.green) + "," + str(self.blue) + ")"
        # 返回:  rgb(123,123,123) 传递参数用

    def __str__(self):
        return "Red:" + str(self.red) + ", Green:" + str(self.green) + ", Blue:" + str(self.blue)
        # 返回:  Red:123, Green:123, Blue:123  查看信息用

    def fade(self):
        self.red += 1
        self.green += 1
        self.red += 1

# 定义点类
class Dot:
    def __init__(self,pos,color,radius,life):
        self.pos = pos
        self.color = color
        self.life = life
        self.radius = radius

    def draw(self,canvas):  # 调用方法的时候记得传canvas
        # Dot 的 color 域，是 RGBcolor 的实例，因而可以调用 RGBcolor 的 make_html 方法
        dotColor = self.color.make_html()
        # 画点的方法
        canvas.draw_circle(self.pos, self.radius, 1, dotColor, dotColor)

    def animate(self):
        self.life -= 1
        if self.radius < 20 :
            self.radius += 0.8

        self.color.fade()

```


##自动消失的点
设定一定条件，让点从 list 中 remove 掉


##渐渐消失的点
给点设定一个生命值，让每一帧生命值减一
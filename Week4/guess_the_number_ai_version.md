#Guess the number AI version



###作业:猜数游戏AI版
- 期待:
    + 抽象你的自然思维
    + 在尽可能短的代码行数中完成:无人介入的猜数游戏
    + 最好能动画式演示游戏过程
- 要求:
    + 基础: 用程序模拟出自己猜数的策略, 并进行检验
    + 可用: 用自制的猜数AI, 和自己的游戏对战
    + 合格: 猜数AI的游戏过程,可记录,可回放
    + 天才: 猜数AI的游戏过程,可记录,可回放,可分享,加载...进一步的:
        * 通过大量的游戏对战,统计自个儿AI 的能力?! 
        * 发布他人的AI 也可以接入的服务?
        * 并行多组游戏?
        * 怎么证明自个儿的 AI 策略是最优的?能用最少次数猜中?
        
##二、作业地址
- Codesculptor: [点彩画板V1](http://www.codeskulptor.org/#user39_mI9h518rF5_8.py) （[版本1：实现基本猜数功能](http://www.codeskulptor.org/#user39_ILZDVOyDt5_1.py)）


##三、分析
之前[猜数游戏](http://www.codeskulptor.org/#user39_o4PKSQCHO3_3.py)已实现手动猜数功能。AI版需要增加：

- 输出到画布上 [done]
- 生成一定范围内随机数 [done]
- 升级猜数函数 [done]
- 保存每次猜数历史 [done]
- 回放猜数历史
- 统计猜数次数

##四、动手

###1. 输出到画布上
复习前面的作业，增加``frame.set_draw_handler(draw)``和 draw(canvas)函数。

###2.生成一定范围内随机数
先默认生成10内随机数作为神秘数，方便测试

###3.升级猜数函数
####A-思路
- 拆分函数

	拆分成两个函数：
	
	+ 生成最接近中数的函数 ai_num()
	+ 比较猜数和神秘数的函数 compare()



####B-问题和解决
compare()问题

- 尝试：写compare函数，传入两个参数，一个代表猜数，一个代表神秘数。
- 新问题： button 的 handler 只允许传一个参数。
- 尝试：修改为用全局变量 guess_number 传递猜数。
- 新问题：第一次猜数后，猜数不会变，一直停留在5。说明最大值和最小值没有更新
- 尝试：在每次比较后更新最大值或最小值。这个过程需要可视化实例帮助思考，不然很容易出错
- 新问题：依然没有更新
- 尝试：找啊找啊找啊找，发现函数没有return
- 新问题：依然没有更新
- 尝试：找啊找啊找啊找，发现没有定义最大值最小值为全局变量

		def compare():
		    global secret_number,guess_number,num_max,num_min,result
		    guess_number = ai_num()
		    print "Your guess was: ",guess_number
		    if secret_number > guess_number :
		         result = 'low'
		         num_min = guess_number     # set the new min
		    elif secret_number < guess_number :
		         result = 'high'
		         num_max = guess_number     # set the new max
		    else:
		         result = 'bingo'
		    
		    return result

ai_num()问题

- 尝试：为方便调试，将最小值固定为零，ai_num生成 （guess_num-0）/2 的数字。然后跟神秘数进行比较。
- 问题：试了半天发现只能适用于<5的情况。
- 尝试：增加两个全局变量，num_max 和 num_min ，分别代表猜数过程中最大和最小数构成的范围。
- 问题：还是只适用一半的情况，检查很久无果
- 尝试：用 viz mode 检查，设 10 以内的随机数，首次 ai 生成 5，没有问题；第二次如果生成大于 5 的数，ai 数却小于 5
- 问题：问题出在
	
		def ai_num():
		    global num_max,num_min		    
		    newAI = (num_max - num_min)/2
		    return newAI
	用错了减法，应该用加法，多么痛的领悟！算法很重要啊。
	
- 尝试：修改 newAI 的公式，问题解决。


###4.保存每次猜数历史

``guess_list = []``

``guess_list.append(newAI)``

- 问题：新开一局时，之前 list 中的记录没有清除
- 尝试：在生成随机数函数中加入 ``guess_list = []``
- 问题：并没有起作用
- 尝试：查找文档中 list 的说明
- 尝试：用 guess_list.pop()
- 问题：首次使用时提示``pop from empty list``
- 尝试：挪到 bingo 后执行
- 问题：无效，应该需要指定 pop 的位置
- 尝试：用 for 循环 pop list

		for i in range(len(guess_list))
             guess_list.pop()
	提示``invalid syntax``  
- 问题：list 长度如何取？ range()只能用数字吗？
- 尝试：用局部变量传递值到 range 中，还是不行
- 问题：看了半天，发现 for 循环忘记写 ：
- 尝试：补充：后运行，猜到最后一步提示 ``pop index out of range``
- 问题：又重头到尾检查一遍，发现问题又出在重新开始的函数里面，没有讲 guess_list 定义为全局变量
- 尝试：定义全局变量，问题解决

###5.回放历史
增加timer，写 timer handler，需要增加计数器记录猜数的次数

- 尝试：

###Do it better next time
- 画程序思路时，需要层层细化，先是功能拆分，然后是各事件和函数的关系，再是各函数内部的逻辑，以及算法。不要指望一个图概括全部。
- 常见错误：
	+ 函数忘记写冒号 ：
	+ 忘记定义全局变量
	+ 传参数传错

###笔记：类的用法
	
	class Character:
	   def __init__(self, name, initial_health):   # __int__ 初始化对象
	       self.name = name					# self 是新的对象的引用
	       self.health = initial_health	# name 和 health 是self对象中的域(field)
	       self.inventory = []
	       
	   def __str__(self):
	       s  = "Name: " + self.name
	       s += " Health: " + str(self.health)
	       s += " Inventory: " + str(self.inventory)
	       return s
	   
	   def grab(self, item):			# methond defines the behaviors of objects
	       self.inventory.append(item)
	       
	   def get_health(self):			# method 所有方法的第一个参数都是self
	       return self.health
	       
类和函数的区别在于，类只能操作某个类型的对象，而不能通过其他方法直接被调用。每一个类都包含属性（变量和值）和行为（函数）。

面向对象编程的强大之处，对于一个类，理解其接口和已实现的方法，就可以使用了。



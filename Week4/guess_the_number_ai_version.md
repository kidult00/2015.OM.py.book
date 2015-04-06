#Guess the number AI version

###类
	
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
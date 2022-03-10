# Python元类

> 元类是创建类的类，在python中一切皆对象，类本身也是对象，类的实例也是对象，函数function也是对象，模块也是对象

- type()或type类
- metaclasss 指定某一个类的元类



**\__new\_\_()和\_\_init\_\_()关系**

**\__call\_\_的作用**

```
__new__() 是在类被创建时调用，在元类中，当创建某一类对象时，会调用元类的__new__()函数，在这个函数内返回类的对象，一般类对象包含，类的属性或方法@classmethod

__init__() 创建类对象的参数类的实例方法

__call__() 当类对象作为函数被调用时，会执行类对象的__call__()函数
```

```python
class A:
    def __init__(self, a, b):
        self.a = a
        self.b = b
        print('--init--')
    def __new__(cls, *args, **kwargs):
        print('---A new()---')
        # 只是创建类的对象，而不需要传入初始化参数
        return super().__new__(cls)
    def __call__(self):
        return self.a, self.b
    
a = A(10, 20)
a()  # 对象作为函数被调用，调用时调用类对象中call方法
```

```
---A new()---
--init--
```

### type()函数创建类

- class_name/name 类名
- base_name/bases 父类名
- attrs 类成员属性，使用字典结构

```python
# 创建User类，类的属性有name，id， 没有父类
# type(name, bases, attrs) 其中bases，attrs必须按位置传参
User = type('User', None, {'name': 'disen', 'id': 100})
User
```

```python
def say(self):
    print(self.id, self.name)
```

```
User.say = say
```

```
User.id
u = User()
print(u)
```

```
AppUser = type('AppUsr', (User,), {})
AppUser
   
```

#### 基于type类实现ORM

- 定义元类，继承type类
- 声明无类解析的模型类。声明时解析出类的字段
- 声明基本模型类，类似Django.models.Model

#### 定义字段类

```python
class Field:
	def __init__(self, db_column=None):
        self.db_column = db_column
class InitField(Fiekd):
    def __init__(self, db_column=None, max_values=None):
        super().__init__(db_column)
        if max_value < 0:
            raise Excption('最大值不能小于0')
        self.max_value = max_value
class CharField(Field):
    def __init__(self, db_column=None, max_length=None):
        super().__init__(db_column)
        self.max_length = max_length
     
```

#### 定义一个模型元类ModelMeta

- 声明一个\__new\_\_(lcs, name, bases, attrs)方法

- 判断name是否为Model(所有模型的父亲)，如果不是则提取模型类的属性和Meta

- ```python
	class ModelMetaclass(type):
		def __new__(cls, name, bases, attrs):
			if name == 'name':
				retunr super().__new__(cls, name, bases,atrs)
			# name 肯定model的子类，也就是实际与表对应的模型类
			fields = {}
			_meta = {}
			# 提取模型类定义的字段
			for key, field in attrs.items():
				if isinstance(field, Fiedl):
					fields[key] = field
	        # 提取模型定义类中的表名
			meta = attrs.get('meta', None)
	        if meta is None:
	            _meta['db_table'] = name.lower()
	        else:
	            _meta['db_table'] = getattr(meta, 'db_table')
	            
	        attrs.pop('Meta')
	        attrs['fields'] = fields
	        attrs['_meta'] = _meta
	     	
	        return super().__new__(csl,name, bases, attrs)
	```

	#### 定义model类

	

	- 指定元类是ModelMetaclass
	- 实现ORM相关的函数
		- save()
		- update()
		- delete()
		- get()
		- filter()

	```python
	class Model(metaclass=ModelMetaclass):
	    def __init__(self, **values):
	        for key, value in values.items():
	            # 设置当前模型对象的属性和属性值
	            setattr(self, key, value)
	            self[key] = values
	    def save(self):
	       	# 子类的对象已被元类解析
	    	# self.fields
	       	# self._meta
	        sql = 'insert into %s(%s) values(%s)'
	        field_str = ','.join([key for key in self.fields.keys()])
        values_str = ','.join("%%(%s)s") % key for key in 
	        							
	```
	
	```python
	class UserModel(Model):
		id = IntField(max_value=20)
		name = CharField(max_value=50)
		age = IntField(max_value=200)
		
    class meta:
	        db_table = ''
	```
	
```
	u1 = UserModel(id=1, name='disen', age=20)
	```
	
	
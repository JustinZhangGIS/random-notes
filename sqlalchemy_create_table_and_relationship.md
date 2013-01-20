# Sqlalchemy #

创建表及其关系，首先引用下面的

	from sqlalchemy import Table, Column, Integer, ForeignKey
	from sqlalchemy.orm import relationship, backref
	from sqlalchemy.ext.declarative import declarative_base
	
	Base = declarative_base()

## 创建表 ##

### User ###

采用declarative的方式创建一个User表，代码如下：

	class User(Base):
	    __tablename__ = 'user'
	    id = Column(Integer, primary_key=True)
	    name = Column(String(50), nullable=False)
	    password = Column(String(80), nullable=False)
	    email = Column(String(50))
	    
	    def __init__(self, name, password, email=''):
	        self.name = name
	        self.password = password
	        self.email = email


其中，数据表的名称为user，主键为id，有name, password和email三个字段。

### Item ###
	
	class Item(Base):
	    __tablename__ = 'item'
	    id = Column(Integer, primary_key=True)
	    title = Column(String(50), nullable=False)
	    price = Column(String(20), nullable=False)
	    desc = Column(String(150), nullable=False)
	    
	    def __init__(self, title, price, desc):
	        self.title = title
	        self.price = price
	        self.desc = desc

### Weibo ###

	class Weibo(Base):
	    __tablename__ = 'weibo'
	    id = Column(Integer, primary_key=True)
	    uid = Column(Integer, nullable=False)
	    name = Column(String(50), nullable=False)
	    
	    def __init__(self, uid, name):
	        self.uid = uid
	        self.name = name


## 表的关系 ##

### one-to-many ###

假设User拥有多个Item，可以做如下定义：

	class User(Base):
	    __tablename__ = 'user'
	    id = Column(Integer, primary_key=True)
	    name = Column(String(50), nullable=False)
	    password = Column(String(80), nullable=False)
	    email = Column(String(50))
		itemlist = relationship("Item", order_by="Item.id", backref="user")

	class Item(Base):
	    __tablename__ = 'item'
	    id = Column(Integer, primary_key=True)
	    title = Column(String(50), nullable=False)
	    price = Column(String(20), nullable=False)
	    desc = Column(String(150), nullable=False)
	    user_id = Column(Integer, ForeignKey('user.id'))

使用方式如下：

	user = User(name='jimmy', password='123', email='xx@xx.com')
    item1 = Item(title='apple', price='2.00', desc='fresh apple')
	item2 = Item(title='orange', price='1.00', desc='fresh orange')
    user.itemlist.add(item1)
	user.itemlist.add(item2)
	dbsession.add(user)
	dbsession.commit()
	print item1.user.name


### one-to-one ###

假设User有唯一的一个weibo账号，定义如下：

	class User(Base):
	    __tablename__ = 'user'
	    id = Column(Integer, primary_key=True)
	    name = Column(String(50), nullable=False)
	    password = Column(String(80), nullable=False)
	    email = Column(String(50))
		weibo = relationship("Weibo", uselist=False, backref="user")

	class Weibo(Base):
	    __tablename__ = 'weibo'
	    id = Column(Integer, primary_key=True)
	    uid = Column(Integer, nullable=False)
	    name = Column(String(50), nullable=False)
		user_id = Column(Integer, ForeignKey('user.id'))
	    

也可以使用下面的定义方式：

	class User(Base):
	    __tablename__ = 'user'
	    id = Column(Integer, primary_key=True)
	    name = Column(String(50), nullable=False)
	    password = Column(String(80), nullable=False)
	    email = Column(String(50))
		weibo_id = Column(Integer, ForeignKey('weibo.id'))
    	weibo = relationship("Weibo", backref=backref("user", uselist=False))

	class Weibo(Base):
	    __tablename__ = 'weibo'
	    id = Column(Integer, primary_key=True)
	    uid = Column(Integer, nullable=False)
	    name = Column(String(50), nullable=False)


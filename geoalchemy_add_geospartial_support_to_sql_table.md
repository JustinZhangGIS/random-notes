# GeoAlchemy #

## 简介 ##

GeoAlchemy在Sqlalchemy的基础上对数据表提供地理位置索引的支持，可以提供点、线、区域等地理位置特征的支持。这里简单讨论对点（Spot）的支持。

## 方法 ##

假设有Item数据表，具有地理位置location属性，数据表的定义如下：

	from geoalchemy import (Geometry, Point, LineString, Polygon, GeometryColumn, GeometryDDL, WKTSpatialElement)

	class Item(Base):
	    __tablename__ = 'item'
	    id = Column(Integer, primary_key=True)
	    title = Column(String(50), nullable=False)
	    location = GeometryColumn(Point(2), nullable=False)
	    
	    def __init__(self, title, location=''):
	        self.title = title
	        self.location = location

Item数据表定义了如下的二维点的location属性：

	location = GeometryColumn(Point(2), nullable=False)

随后执行下面语句，完成Item数据表的位置索引创建

	GeometryDDL(Item.__table__)

## 使用 ##

#### 创建对象 ####

	item = Item(title='iphone', location='POINT(%s %s)' %('31.199', '121.587'))

#### 获取位置 ####

获取item对象的位置信息：

	lat = dbsession.scalar(item.location.x)
	lng = dbsession.scalar(item.location.y)

#### 区域搜索 ####

假设有一个矩形的地理区域，由两个点坐标定义，north-east和south-west。javescript中获取地图当前矩形框的代码如下：

	var bounds = this.getBounds();
	north = bounds.getNorthEast().lat();
	east = bounds.getNorthEast().lng();
	south = bounds.getSouthWest().lat();
	west = bounds.getSouthWest().lng();

geoalchemy对区域的搜索方法：

	box = "POLYGON((%s %s, %s %s, %s %s, %s %s, %s %s))" % (north, east, south, east, south, west, north, west, north, east)
    items = dbsession.query(Item).filter(Item.location.within(box)).all()
    return items

其中首先顶一个多边形（POLYGON）区域，再采用Item.location.within(box)作为条件。

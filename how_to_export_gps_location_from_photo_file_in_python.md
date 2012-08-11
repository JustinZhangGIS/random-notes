# 获取图片文件的地理位置信息

## EXIF

可交换图像文件常被简称为EXIF（Exchangeable image file format），是专门为数码相机的照片设定的，可以记录数码照片的属性信息和拍摄数据。参考[WIKI](http://zh.wikipedia.org/wiki/EXIF)。

## GPS

照片中可以记录GPS信息，查看http://en.wikipedia.org/wiki/Geotagging。从图片中读取出的数据格式为度分秒：

	GPS Latitude                    : 57 deg 38' 56.83" N
	GPS Longitude                   : 10 deg 24' 26.79" E
	GPS Position                    : 57 deg 38' 56.83" N, 10 deg 24' 26.79" E

转换为数字格式为：

	GPS Latitude                    : 57.64911
	GPS Longitude                   : 10.40744
	GPS Position                    : 57.64911 10.40744

## 读取

使用python库[pyexiv2](http://tilloy.net/dev/pyexiv2/overview.html)可以方便地读取到照片的EXIF数据。

	#!/usr/bin/python
	import pyexiv2
	
	try:
	  # Open the photo file.
	  photo = './pic.jpg'
	  md = pyexiv2.ImageMetadata(photo)
	  md.read()
	
	  # Read the GPS info.
	  latref = md['Exif.GPSInfo.GPSLatitudeRef'].value
	  lat = md['Exif.GPSInfo.GPSLatitude'].value
	  lonref = md['Exif.GPSInfo.GPSLongitudeRef'].value
	  lon = md['Exif.GPSInfo.GPSLongitude'].value
	
	except:
	  print "No GPS info in file %s" % photo
	  sys.exit()
	
	# Convert the latitude and longitude to signed floating point values.
	latitude = float(lat[0]) + float(lat[1])/60 + float(lat[2])/3600
	longitude = float(lon[0]) + float(lon[1])/60 + float(lon[2])/3600
	if latref == 'S': latitude = -latitude
	if lonref == 'W': longitude = -longitude
	
	print latitude
	print longitude
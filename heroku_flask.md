# heroku flask
===

## heroku toolbelt

install the toolbelt, then:
	
	$ heroku login
	$ heroku apps #查看所有应用
	$ heroku run COMMAND #例如heroku run python app.py
	$ heroku logs
	$ heroku apps:create APP_NAME #创建应用
	$ heroku apps:destroy APP_NAME #删除应用
	$ heroku apps:info --app APP_NAME
	$ heroku apps:open --app APP_NAME #浏览器中打开该应用

## git 

	$ ssh-keygen -t rsa
	$ heroku keys:add ~/.ssh/id_rsa.pub
	$ git init
	$ git add .
	$ git commit -m "init"
	$ git push heroku master
	
## virtualenv

use venv

	$ mkdir helloflask && cd helloflask
	$ virtualenv venv --distribute
	$ source venv/bin/activate
	$ pip install flask
	$ pip install Flask-Sqlachelmy

generate the dependency

	$ pip freeze > requirements.txt
	
	
## gunicorn

	$ brew install libevent
	$ pip install gunicorn
	
add `gevent` and `gunicorn` to 	requirements.txt. Add cmd to Procfile

	web: gunicorn -w 4 -b 0.0.0.0:$PORT -k gevent app:app
	
其中，app:app，前者指脚本的文件名，后者指App对象。
	
	
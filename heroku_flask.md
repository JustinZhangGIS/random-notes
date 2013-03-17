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
	$ heroku rename NEW_APP_NAME #修改应用名称
	
## PostgreSQL
	$ pip install psycopg2
	$ heroku heroku addons:add heroku-postgresql:dev
	Adding heroku-postgresql:dev on tbbttrans... done, v7 (free)
	Attached as HEROKU_POSTGRESQL_AQUA_URL
	Database has been created and is available
 	! This database is empty. If upgrading, you can transfer
 	! data from another database with pgbackups:restore.
	Use `heroku addons:docs heroku-postgresql:dev` to view documentation.
	
then,

	$ heroku pg:promote HEROKU_POSTGRESQL_AQUA_URL
	Promoting HEROKU_POSTGRESQL_AQUA_URL to DATABASE_URL... done
	
and, 

	app.config['SQLALCHEMY_DATABASE_URI'] = os.environ['DATABASE_URL']
	
ref to this blog post: [flask-and-postgresql-on-heroku](http://blog.y3xz.com/blog/2012/08/16/flask-and-postgresql-on-heroku/).

## Scheduled Jobs

ref to this doc:[clock processes python](https://devcenter.heroku.com/articles/clock-processes-python).

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
	$ deactivate

generate the dependency

	$ pip freeze > requirements.txt
	
install all lib in the requirements.txt

	$ pip install -r requirements.txt
	
	
## gunicorn

	$ brew install libevent
	$ pip install gunicorn
	
add `gevent` and `gunicorn` to 	requirements.txt. Add cmd to Procfile

	web: gunicorn -w 4 -b 0.0.0.0:$PORT -k gevent app:app
	
其中，app:app，前者指脚本的文件名，后者指App对象。
	
	

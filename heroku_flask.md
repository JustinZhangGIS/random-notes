# heroku flask
===

## heroku toolbelt

install the toolbelt, then:
	
	$ heroku login

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
	
	
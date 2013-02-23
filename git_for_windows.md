#Git

##Install

[Git for Windows地址](http://msysgit.github.com/)

##Config

#### Generate a new SSH key

	ssh-keygen -t rsa -C "your_email@youremail.com"
	# Creates a new ssh key using the provided email
	# Generating public/private rsa key pair.
	# Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
	
#### Add your SSH key to GitHub

1.Go to your Account Settings
1.Click "SSH Keys" in the left sidebar
1.Click "Add SSH key"
1.Paste your key into the "Key" field
1.Click "Add key"
1.Confirm the action by entering your GitHub password

#### Test everything out

	ssh -T git@github.com
	# Attempts to ssh to github
	
you may see this warning:

	The authenticity of host 'github.com (207.97.227.239)' can't be established.
	# RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
	# Are you sure you want to continue connecting (yes/no)?
	
Verify that the fingerprint matches the one here and type "yes".

## Use

参考[http://rogerdudler.github.com/git-guide/](http://rogerdudler.github.com/git-guide/)

	

# Windows下使用Sublim Text 2编写CoffeeScript


### 安装node.js

点击这里下载并安装[node.js](http://nodejs.org/)。假设安装到了`C:\nodejs`目录下。

### 安装CoffeeScript

打开cmd，切换到`C:\nodejs`目录下，执行

	npm install -g coffee-script

### 安装Sublim Text 2

不再赘述。

### 安装Sublime Package Control

打开`Sublime Console`（`View -> Show Console` 或 快捷键ctrl+`），并传输入：

	import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

回车执行上面脚本之后，关闭重启Sublime Text 2完成安装过程。启动Sublime Text 2之后，检查`Preferences`菜单下是否有`Package Control`子菜单，如果存在表示安装成功。

### Sublime Text 2安装Coffee支持

1. 使用`Shift + Ctrl + P`调出命令面板
1. 输入`install`调出`Package Control: Install Package`选项，按下回车
1. 在列表中找到`CoffeeScript`，按下回车进行安装
1. 重启Sublime Text 2使之生效

### 注意事项

自动安装的Build支持在Windows下有点问题，打开

	C:\Users\jimmy\AppData\Roaming\Sublime Text 2\Packages\CoffeeScript\CoffeeScript.sublime-build

此为Win7下当前用户路径，注意替换为你机器上的当前用户路径。然后将

	"cmd": ["coffee","-c","$file"] 

修改为 

	"cmd": ["coffee.cmd","-c","$file", "&&", "coffee.cmd","$file"]

### Hello Coffee!

下面，新建`hello.coffee`文件

	console.log "Hello Coffee!"

快捷键`CTL+B`执行脚本，得到以下输出：

	Hello, Coffee!
	[Finished in 0.3s]

恭喜，安装完成！
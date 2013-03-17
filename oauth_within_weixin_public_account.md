#在微信公众帐号中进行OAuth授权登陆
======


## 在微信内收藏文章

假设有微信用户A君，订阅了公众帐号“小道消息”（微信号DbaNotes），每天收到一篇文章推送。A君有时希望能把文章收藏起来，可以稍后阅读或者反复欣赏。当然，可以有千百种方法来完成收藏这个事，A君是个微信脑残粉，希望整个收藏过程仅在微信内即可完成。

印象笔记的微信公众号“我的印象笔记”是基于微信的私有API开发的，可以把消息、文章的转存为笔记。使用该帐号的基本的步骤是：关注“我的印象笔记”，其会提示用户绑定印象笔记的帐户，并发送OAuth授权登陆链接，点击链接到印象笔记网站登陆，完成后，给“我的印象笔记”发送的内容会保存为笔记。使用微信内置Web页面打开文章后，点击分享按钮，“我的印象笔记”已经内置其中，可以一键保存文章。总体上，“我的印象笔记”是很方便的。如果A君是印象笔记的用户（注意，Evernote的国际帐号是无法使用的），那么A君有福了。可惜A君是Evernote用户，而且也不太习惯在Evernote内阅读文章。

A君是Pocket的忠实粉丝，平时在网上闲逛，遇到有意思的文章，会保存到Pocket阅读列表，在PC上这个很简单。A君希望能有一个类似“我的印象笔记”的公众帐号，绑定自己的Pocket帐户，通过发送文章链接的消息给该帐号，将文章添加至Pocket列表。

我们假设这个公众帐号叫MyPocket，下面重点叙述，如何将用户的Pocket帐户OAuth授权给MyPocket。


## 节点

在整个的授权登陆过程中，涉及到的节点主要有下面四个：

1，`微信用户`。例如A君。

2，`公众号消息服务器`。用户可见的是公众帐号，例如MyPocket。

3，`微信服务器`。微信官方服务器，完成1和2之间的消息传递。

4，`Pocket授权登陆服务器`，Pocket应用自身的登陆服务器或页面。

微信用户发送一条普通的文本消息，并得到公众帐号的回应，消息传递的过程如下：

（1），微信用户节点1发送一条消息给节点2

（2），消息到达节点3，对消息进行封装通过HTTP POST推送给节点2

（3），消息到达节点2，处理后对HTTP进行响应，返回给节点3，转发给节点1

（4），消息到达节点1，微信用户接收到消息回应

注意，目前节点2是无法主动发送消息给用户的，仅能在收到节点3的消息推送后，进行消息回复。


## OAuth授权登陆步骤

和普通的在浏览器中完成的OAuth授权登陆有所不同，在微信中OAuth登陆的基本步骤是：

1，微信用户节点1发送特定表识消息（例如“auth”）开始OAuth授权登陆

2，节点2消息后：

	（1）记住节点1的标识串FromUserName，需要保存
	（2）到节点4获取request token，需要保存
	（3）消息回复：用户授权登陆的地址链接，地址链接中需要包含：用户表识串FromUserName和授权完成后的回调（callback）地址
	
3，节点1打开登陆链接，微信内置浏览器中填写帐户，完成授权

4，节点4重定向至节点2的callback地址

5，节点2处理授权登陆callback，根据request token获取access token，需要保存

下面是对该过程的几点说明：

（1）消息推送中所携带的用户相关信息只有FromUserName，是一个加密后的字符串，形如oGfS0jrOSOwJXqNTIy9B8WI8sEac。因此直接获取用户的微信号或者昵称存在困难。据从网上查询到的信息看，一个微信用户发送给某特定的公众帐号的所有消息，该FromUserName是固定的。可用来唯一标识节点1。

（2）步骤2中的FromUserName、request token和步骤5的access token都需要保存。因为节点2会和节点3，以及节点1的浏览器发生HTTP交互，使用session不合适，此处暂使用数据库进行保存操作。

（3）浏览器支持。因为OAuth登陆，其中一个步骤要到第三方的登陆授权界面进行登陆，需要用到浏览器，微信内置的浏览器，可以完美完成此任务。

## 代码

本例采用python的flask框架实现了一个小例子，主要的代码整理如下。授权登陆的步骤2：

	@app.route('/weixin', methods=['POST'])
	def weixin_msg():
		data = request.data
		msg = parse_msg(data)
		content = msg['Content']
		username = msg['FromUserName']
		if content == 'auth':
			pocket = Pocket(POCKET_CONSUMER_KEY, BASE_URL + url_for('auth_callback') + '?username=' + username)
			code = pocket.get_request_token() #code即为request token
			url = pocket.get_authorize_url(code)
			#保存code和username
			user = User(username)
          user.pocket_code = code
          db.session.add(user)
          db.session.commit()
			rmsg = u'点击下面链接登陆Pocket并授权给给我\n\n' + rmsg.encode('utf-8')
    	return response_text_msg(msg, rmsg)
    return help_msg()

步骤5

	@app.route('/auth_callback')
	def auth_callback():
		username = request.args.get('username')
		user = User.query.filter_by(username=username).first()
		pocket = Pocket(POCKET_CONSUMER_KEY)
		resp = pocket.get_access_token(user.pocket_code)
		user.pocket_token = resp['access_token']
		user.pocket_username = resp['username']
		db.session.commit()
		return '<html><head></head><body><h1>认证成功，发送文章链接给MyPocket，即可保存到Pocket阅读列表。</h1></body></html>'
		
授权完成后，用户直接发送文章链接，节点2匹配到Url地址，即添加到Pocket列表：

	def add_item_to_pocket(username, content):
		r = r"(http://[^ ]+)"
		urls = re.findall(r, content)
		rmsg = u''
		user = User.query.filter_by(username=username).first()
      	pocket = Pocket(POCKET_CONSUMER_KEY)
       pocket.set_access_token(user.pocket_token)
		for url in urls:
       	itemjson = pocket.add(url=url)
       	item = json.dumps(itemjson)
       	rmsg += u'已添加"%s"至Pocket\n' % item['title']
       return rmsg
       
## 用法

A君想要收藏小道君发送的文章“「大概8点20分发」”，在微信中点击推送消息，内置浏览器打开文章，点击右上角的分享按钮，点选“复制链接”，打开MyPocket，粘贴，发送的链接如下：

	http://mp.weixin.qq.com/mp/appmsg/show?__biz=MjM5ODIyMTE0MA==&appmsgid=10000250&itemidx=1#wechat_redirect
	
这样，A君就可以在3月17日的早晨大概8点20分在Pocket中欣赏这片好文了。你不妨也试试看：

![image](http://url2pocket.sinaapp.com/static/img/qrcode_for_gh_7fce1bf79138_258.jpg)
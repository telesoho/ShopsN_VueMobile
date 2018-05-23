#shopsn_vue
shopsn前端

ShopsN移动端编译代码方法
不要以为前后端分离的编译技术对于不懂编程的你如同天书，
按我们的步骤非常方便。有基本网络操作技术即可。只需要简单几步即可完成！
 
一,下载 2.2版本的 ShopsN手机H5端(vue源码) 到你电脑, pc端,服务端(api站)传到你的云主机，配置好数据库连接，域名绑定好 

现在你的云主机上有2个站点：mydomain.com就是你自己的域名，别跟我说你没有域名。。
在域名商的管理界面，增加www和api 两个A记录，绑定到两个不同站点上。代码也是不同的哦！www放置商城代码，api放置接口代码。这个在下载的百度云盘里是写的很清楚的。
www.mydomain.com   （web+wap+微商城 默认安装的时候已经配置好数据库了，会跟下面的api站共用一个mysql数据库。可以在\Application\Common\Conf\db.php找到）
api.mydomain.com  (api接口用。不显示内容，为前后端分离的Vue及app所用接口服务.别忘了配置对接好跟web站共用的数据库，Application\Common\Conf\db_config.php,内容参考web站的mysql配置。否则这个接口api站是不发生作用的，可能会导致手机站打开一片空白)

三, 在自己电脑D盘建一个目录如d:\ wap ，这是做编译用的。不是在站点里。把h5压缩包放进去解开压缩（注意不要再有如解压后产生的任何额外目录，除非你会根据情况增加实际编译路径）。打开 \手机端文件夹\config\globle.js,寻找window.API_URL='http://api.shopsn.net/'; 改为你的api站点名字，如api.mydomain.com  

四,本地安装Node.js (http://nodejs.cn/download/),目前nodejs已经集成安装了npm包，一般windows机器直接下载msi后缀后直接下一步即可完成。装好后点击运行，输入cmd打开终端窗口。输入node –version 检测你的node是否出现版本号，npm –v 检测。两次检测均有反应，证明已经成功安装了环境。但让人头疼的是，依赖的模块不全，需要再安装一次npm 包,这就是标准的技术坑。当然技术有挖坑就有填坑，后面继续看。
目前node版本中默认安装的npm仅有核心模块，导致编译时缺少某些模块，编译时会产生如Cannot find module 'chalk'的提示。在编译前需要联网下载扩展模块安装一次npm。安装Node.js环境跟安装npm 包,可能会出现一些问题,百度上一般都有解决办法。

命令： npm install 。因为网络的问题，很多人都会半路遇到报错：npm ERR! code EINTEGRITY。
 
这时有两种方法：

1. 启用VPN。

原因是socket挂起，再运行一次就是了。我某次试了3次才好。一次1-2分钟左右。 
 
2. 如果你没有vpn时， 

    1. 如果有package-lock.json文件，就删掉
    2. 管理员权限进入cmd 
    3. 执行npm cache clean --force 
    4. 之后再npm install 

如此方法不行，网上还有种方法是更换sha1，百度npm ERR! code EINTEGRITY  查询其他方法。
然后就没问题了。下载有进度条，需要等2-3分钟。边下载边自动解压文件。文件大概4M左右吧。提示完成后，就可以编译了。这次真的不会有问题了。
运行npm run build


五,使用npm 包来编译 手机客户端,
	1. 打开CMD,  切换到客户端文件夹如cd D:\wap,使用命令 npm run build 重新编译文件,  
	2. 等待几分钟编译完成,将编译过后的文件 dist内的文件(static跟index.html)代码上传至服务器根目录下的mobile目录。没有就直接建立一个。	只需要将dist 文件夹内的文件上传或覆盖即可,其他文件不需要提交
    3. 请将服务器的urlrewrite 重写功能打开。下载内容提供一个windows服务器的web.config放在api站根目录下。Linux暂缺，可自行参考设置。

测试包
为了给用户一个快速体验移动端演示效果。我们做了一个使用本地测试环境的编译包。

1.首先，手机和电脑都处于同一个无线网路由器下面。将电脑ip设置为192.168.0.8 。商城测试地址设置端口为80 。 api站设置端口为88 （注意web和api都要配置好能联通数据库才行，web可以正常打开商城，api能打开一个报错页面，参照api.shopsn.net不显示商城）
 
测试包放在同一下载目录内。请按照教程自行复制到路径进行测试。

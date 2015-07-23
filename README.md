# 总结
##<a name="index"/>目录
* [2015-07-21](#2015-07-21)
* [2015-07-22](#2015-07-22)

<a name="2015-07-21"/>
##2015-07-21
* 今天看了[提高iOS开发效率的方法和工具](http://www.cocoachina.com/ios/20150717/12626.html)里边的代码片段部分，觉得很方便，可以很好的提高工作效率。随机的查了相关的内容，发现把他通过github管理起来更是方便，换了工作环境也不担心代码片段会丢失。
* 如果想将本地文件夹创建为管理，github创建仓库时，不需要勾选那个创建readme的，然后在需要创建仓库的目录下命令行输入下面代码，最后把文件扔进客户端就行了

```
    echo "# xxx" >> README.md
	git init
	git add README.md
	git commit -m "first commit"
	git remote add origin https://github.com/windream1127/xxx.git
	git push -u origin master
```
* reveal加载方法([Reveal：分析iOS UI的利器](http://security.ios-wiki.com/issue-3-4/)):命令行输入如下代码，然后在- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions 中添加断点，编辑断点 reveal_load_sim
```
command alias reveal_load_sim expr (void*)dlopen("/Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/libReveal.dylib", 0x2);

command alias reveal_load_dev expr (void*)dlopen([(NSString*)[(NSBundle*)[NSBundle mainBundle] pathForResource:@"libReveal" ofType:@"dylib"] cStringUsingEncoding:0x4], 0x2);

command alias reveal_start expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter] postNotificationName:@"IBARevealRequestStart" object:nil];

command alias reveal_stop expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter] postNotificationName:@"IBARevealRequestStop" object:nil];
```
* CoreData编译错误：`CoreData: warning: Unable to load class named ~~`解决方法：将文件添加到编译文件

<a name="2015-07-22"/>
##2015-7-22
###sublime 安装插件
* 第一步，先要启用Sublime Text 2的install package功能。方法如下：在Sublime Text2中View菜单中开启Show Console，在Console命令窗口输入下列代码：
```
import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
```
回车执行脚本，重启Sublime Text2后，使用cmd+shift+p调出Sublime Text2管理器UI，然后在管理器命令行上输入install，找到install Package选项，回车。
* 第二步，输入需要安装的插件关键字，选择后回车即可

###sublime 添加代码片段
tool->New Snippet 会弹出新添加代码片段代码，如下： 保存时后缀必须是`.sublime-snippet`
```
<snippet>
	<content><![CDATA[
Hello, ${1:this} is a ${2:snippet}.
Hello. ${1}
]]></content>
	<!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
	<tabTrigger>hello</tabTrigger>
	<!-- Optional: Set a scope to limit where the snippet will trigger -->
	<!-- <scope>source.python</scope> -->
</snippet>
```
其中`[CDATA[
Hello, ${1:this} is a ${2:snippet}.
Hello. ${1}
]]`是代码片段内容。`${1:this}`部分为tab光标输入部分，所有`${1}`会同时修改，`this`为说明标签。 `<tabTrigger>hello</tabTrigger>`为关键字，输入`hello`然后tab 就会调出内容。

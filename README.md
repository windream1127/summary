# 总结
##<a name="index"/>目录
* [2015-07-21](#2015-07-21)

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





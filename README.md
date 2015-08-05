# 总结
##<a name="index"/>目录
* [2015-07-21](#2015-07-21)
* [2015-07-22](#2015-07-22)
* [2015-07-23](#2015-07-23)
* [2015-07-28](#2015-07-28)
* [2015-07-29](#2015-07-29)
* [2015-07-29](#2015-07-30)
* [2015-07-29](#2015-07-31)

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

<a name="2015-07-23"/>
##2015-7-23
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

###block防止循环引用
创建block匿名函数之前一般需要对self进行weak化，否则造成循环引用无法释放controller:
```
 __weak MyController *weakSelf = self 或者 __weak __typeof(self) weakSelf = self;
 ```
执行block方法体的时候也可以转换为强引用之后再使用：
```
MyController* strongSelf = weakSelf; if (!strongSelf) { return; } 或者 __strong __typeof(weakSelf)strongSelf = weakSelf;
```

<a name="2015-07-28"/>
##2015-7-28
###NSString 一些东西
```
NSString* constString1 = nil;
NSString* constString2 = @"";
NSString* constString3 = @"Hello, World!";
NSString* constString4 = @"哈喽,世界!"; // 汉字+半角标点混合
NSLog(@"constString1[size,length] = [%zd, %zd]", sizeof(constString1),constString1.length); // [8,0]
NSLog(@"constString2[size,length] = [%zd, %zd]", sizeof(constString2),constString2.length); // [8,0]
NSLog(@"constString3[size,length] = [%zd, %zd]", sizeof(constString3),constString3.length); // [8,13]
NSLog(@"constString4[size,length] = [%zd, %zd]", sizeof(constString4),constString4.length); // [8,6]
```
以上可知string.length可用来判空：如果length为零，则表示字符串对象为nil或为不包含任何字符。</br>

NSStringCompareOptions 枚举：
```
typedef NS_OPTIONS(NSUInteger, NSStringCompareOptions) {
     NSOrderedAscending = -1,    // < 升序
     NSOrderedSame,              // = 等于
     NSOrderedDescending   // > 降序
} NSComparisonResult;
```
NSStringCompareOptions 枚举的值
```
enum{
    NSCaseInsensitiveSearch = 1,//不区分大小写比较
    NSLiteralSearch = 2,//区分大小写比较
    NSBackwardsSearch = 4,//从字符串末尾开始搜索
    NSAnchoredSearch = 8,//搜索限制范围的字符串
    NSNumbericSearch = 64//按照字符串里的数字为依据，算出顺序。例如 Foo2.txt < Foo7.txt < Foo25.txt
//以下定义高于 mac os 10.5 或者高于 iphone 2.0 可用
    ,
    NSDiacriticInsensitiveSearch = 128,//忽略 "-" 符号的比较
    NSWidthInsensitiveSearch = 256,//忽略字符串的长度，比较出结果
    NSForcedOrderingSearch = 512//忽略不区分大小写比较的选项，并强制返回 NSOrderedAscending 或者 NSOrderedDescending
//以下定义高于 iphone 3.2 可用
    ,
    NSRegularExpressionSearch = 1024//只能应用于 rangeOfString:..., stringByReplacingOccurrencesOfString:...和 replaceOccurrencesOfString:... 方法。使用通用兼容的比较方法，如果设置此项，可以去掉 NSCaseInsensitiveSearch 和 NSAnchoredSearch
}
```

<a name="2015-07-29"/>
##2015-7-29
###UIButton label 大小设置
```
button.titleLabel.font = [UIFont systemFontOfSize:15.0f];
```

<a name="2015-07-30"/>
##2015-7-30
###性能优化
* 通过僵尸对象分析EXC_BAD_ACCESS 问题
* Analyze静态分析
** 分析变量是否未被使用
** 分析代码逻辑问题
** 分析潜在内存泄露
* leak 分析内存泄露

###IOS socket基础  还没看，以后看
[IOS的socket开发基础](http://my.oschina.net/u/1378445/blog/340206)

<a name="2015-07-31"/>
##2015-7-31
###tableView相关
* 变更tableView数据的时候，可以动态的修改
```
[self.tableView beginUpdates];
[self.tableView setTableHeaderView:headerView];//修改内容代码
[self.tableView endUpdates];
```
* tableView中的几个View（不全面）
	* TableHeaderView、tableFooterView 直接设置即可
	* section的headView、footerView 通过代理设置
```
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section;   // custom view for header. will be adjusted to default or specified header height<br>
- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section;
```

<a name="2015-08-05"/>
##2015-8-05
* [scrollToRowAtIndexPath出错原因分析](http://blog.csdn.net/yuanya/article/details/11644243)
 在项目中用到scrollToRowAtIndexPath，但程序一旦调用scrollToRowAtIndexPath就出错，经分析是数据源和tableview中cell不同步的原因， 比如，dataArray中有3个元素，而tableview中只显示了2个元素，当你scrollToRowAtIndexPath:[NSIndexPath indexPathForRow:2 时当然就出错了。<br>

所以调用前要确保数据源data source 和 tableview中的数据是一致的，调用scrollToRowAtIndexPath之前最后调用一下［self.dataTableView reloadData］；
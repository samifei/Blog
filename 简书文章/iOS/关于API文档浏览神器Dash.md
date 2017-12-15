![](http://upload-images.jianshu.io/upload_images/304454-c2468a951b809f76.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>先抛出几个问题
Dash是干嘛的？
Dash为啥被苹果下架了？
Dash怎么用？

### [Dash](https://kapeli.com/dash)
- Dash作用
Dash是一个API文档浏览器（ API Documentation Browser）
是 代码片段管理工具（Code Snippet Manager）。
`你没看错，他就这俩功能，没了😄但对程序员来说这不就是最关心的特性么，我之前用过些其他的，感觉还是Dash最好用。说的确实少点，那就再加些具体的。`
Dash 拥有强悍的API文档浏览·搜索功能，大家最常用的功能。每天都会反复查看、搜索那么多的API细节，`没个NB的好APP怎么旋转跳跃的装B`查文档要窗口不停的切来切去，很烦啊！Dash采用集成单一窗口的方式，很好的解决了这个问题

![看我NB不](http://upload-images.jianshu.io/upload_images/304454-c138af78d1e1ffc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图便是Dash的API浏览器主界面：
左侧边栏是各种编程语言以及框架的导航大纲`看你下载多少`，
点击某个节点，右边的内容区域就是文档的详细信息啦，非常直观。
也可以在左上方的搜索框内通过输入关键字，查找相关的API文档，非常类似全文检索的实现方式，Dash的响应速度非常快！
**关键**是可以同时查询不同的语言、框架内容，实在是太方便了。
来看下设置页面：

![1.png](http://upload-images.jianshu.io/upload_images/304454-82063cb8ac1242c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![2.png](http://upload-images.jianshu.io/upload_images/304454-af7368d5f8f3bc2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![3.png](http://upload-images.jianshu.io/upload_images/304454-fc3dd2ec1e106aad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



- Dash为啥被苹果下架了？
> 我就不多说什么，看知乎 跳大神

[如何看待 Dash 被 App Store 下架？](https://www.zhihu.com/question/51357789?sort=created)
[API文档浏览器Dash 被下架：包含大量虚假评价](http://www.cnbeta.com/articles/tech/545793.htm)
Apple:以后老实点行吗？Dash
Dash：。。。
Apple：叫爸爸
Dash：。。。
Apple：草，滚犊子吧。
Dash：。。。

- Dash怎么用？
我X，说这么多你还没去下载。什么，你下载了不会用？那我在说点。
Dash自带了丰富的API文档，涉及各种主流的编程语言和框架，全列出来很吓人的：
```
ActionScript, Android, C++, Cappuccino, Cocos2D, Cocos3D, 
Corona, CSS, Django, Groovy, HTML, Java, JavaFX, 
JavaScript, jQuery, Kobold2D, Lua, MySQL, Node.js,
 Man Pages, Perl, PHP, Python, Ruby, Ruby on Rails,
 Scala, Sparrow, SQLite, Unity 3D, WordPress, XSLT, XUL
... ...
```
而且它的文档库采用了docset格式，高级用户基于网站提供的教程，很容易就能自行添加其他的扩充文档，其实Dash在最初发布的时候，只支持很少的几个文档浏览，是后来通过用户不断贡献，以及作者及时的反馈，逐步壮大，才具备了如此广泛的语言、框架支持。要添加API文档，打开软件配置界面，切换到Docset选项卡即可看到所有内置的文档列表，按需要自行下载即可（如果是自己制作的docset，也可以导入Dash）：

![导入自己制作的docset](http://upload-images.jianshu.io/upload_images/304454-5536b0dbb57d1cfb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####代码片段管理功能
利用Dash的代码片段管理功能，我们可以把日常使用频繁（也就是你经常需要复制粘贴）的代码保存起来，然后为其设置一个独一无二的缩写，这样一来原本需要一遍又一遍的敲击键盘重复录入的繁琐工作，就可以交给Dash来帮你搞定啦。比如截图中的例子，就是ExtJS中发起Ajax请求的代码片段，哪怕是copy & paste，时间长了也会很烦的，我给它设置了一个缩写（ajax），以后在需要编写这段代码的时候，就只需要敲击这几个字母，它就会魔法般的出现在光标所在位置啦！很神奇吧？嘿嘿，其实这种扩展缩写的功能，还有很多软件都能做到，比如TextExpander，不过就用户体验和各种细节，诸如界面UI，特别是扩展占位符的处理上，目前还没有哪一个能比得过Dash的（Dash is the best!）。来看看使用代码片段的截图吧：

![1](http://upload-images.jianshu.io/upload_images/304454-7504821f375bdbe6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![2](http://upload-images.jianshu.io/upload_images/304454-1c19fdf6695cdc33.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Dash的缩写扩展功能很强大，比方说上面那个例子，在保存代码片段的时候，你可以使用双下划线标明占位符，在执行扩展的时候就可以通过tab键来在各个占位符之间切换，根据需要输入实际的值，最后回车即可把片段粘贴到光标所在之处。除了占位符，它还支持下面这些变量符号：
@clipboard 自动插入当前剪贴板中的内容
@cursor 代码片段粘贴完毕之后，自动将光标定位到此处
@date 自动插入当前日期
@time 自动插入当前时间









END

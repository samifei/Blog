这么晚还写这些，主要是有些东西以前没用到，最近使用到，所以写下算做个记录吧。
##正文
###Interface Builder
Xcode6中在原有的Auto layout的基础上，添加了Size Classes新特性，通过这个新特性可以使用一个XIB或者SB文件，适配不同的屏幕以及iPhone和iPad两种设备。（本人喜欢代码控制）
- 创建一个XIB文件进去，点击下面红框的位置，会出现从3.5寸-5.5寸一系列屏幕尺寸的选项。直接点击不同屏幕尺寸，以及横竖屏选项，切换不同的屏幕显示。在iPad上还可以选择是否分屏，功能非常强大。在右边有一个Vary for Traits 🔘按钮，点击这个选项就可以同时显示所有可选的屏幕样式，只是显示上看起来比较多，没啥卵用（默认的XIB是6s的长方形，比之前正方形看起来舒服了）.
![Interface Builder](http://upload-images.jianshu.io/upload_images/304454-b3d85225ff650d67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 在Xcode8之前，创建一个XIB或SB文件，都是一个600*600的方块。在Xcode8之后，创建的XIB文件默认是6s尺寸的大小。
Xcode8打开之前旧项目的XIB或SB文件时，会弹出下面的“弹框”， 这时候一般直接选择Choose Device即可。

![弹框](http://upload-images.jianshu.io/upload_images/304454-5f44aeecc9f4fa5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>  但是这样有个问题，如果Xcode8打开过这个XIB文件，并选择Choose Device之后。其他的Xcode8以下版本的编译器，会报以下错误：
The document “ViewController.xib” requires Xcode 8.0 or later. This version does not support documents saved in the Xcode 8 format. Open this document with Xcode 8.0 or later.
有两种方法解决这个问题：
也升级Xcode8，比较推荐这种方式，应该迎接改变。
右击XIB或SB文件 -> Open as -> Source Code，删除xml文件中下面一行字段（如下）。

![](http://upload-images.jianshu.io/upload_images/304454-b9713ef0f1cc8d55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###Target中General的变化
在Xcode8中可以通过Automatically manage signing选项，让苹果为我们管理证书和配置文件，设置也都是由苹果来完成的。在Xcode8中新建项目，这个选项默认是被勾选的（我也不习惯用，我都是debug release分开设置，这样做demo时候直接用debug 通配符的profile）。

![Target中General---Automatically manage signing](http://upload-images.jianshu.io/upload_images/304454-b3d6182c55de03d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![debug release分开设置](http://upload-images.jianshu.io/upload_images/304454-73e1b572e34cf60b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###Xcode插件
在Xcode8中所有第三方插件都失效了，在之前很多iOS开发者，都是通过[Alcatraz](http://alcatraz.io/)来管理插件的，现在Alcatraz也是不可用的。但是Xcode8自身也对编译器进行了升级，将一些比较好的插件功能加入到Xcode中（我常用的VVDocumenter-规范注释生成器，快捷键是“option + command + / ”）。
>在Xcode8中支持了开发插件工程，并且为我们提供了一个插件模板，开发的插件可以上传到App Store下载。苹果这么做有一个原因在于，之前Xcode和插件是运行在同一个进程的，所以插件的崩溃也会导致Xcode崩溃。苹果现在将插件作为一个单独的应用程序，分开进程运行，不会对Xcode带来其他影响。

![没用过，不清楚](http://upload-images.jianshu.io/upload_images/304454-d6343fe065f740a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###Runtime Issues
在开发过程中，因为语法或明显的代码错误(例如Retain Cycle)，编译器可以发现并报黄色或红色警告。但是一些因为代码逻辑导致的错误，编译器并没有办法找到。例如因为代码逻辑的问题导致两个数组相互引用，都不能释放。
![数组循环引用](http://upload-images.jianshu.io/upload_images/304454-f9e782375826c348.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过Xcode8提供的Runtime Issues新特性，查找到运行过程中出现的问题（紫色的感叹号），并通过Graph的方式将问题可视化的展现给开发者(不会用)
![Debug Memory Graph](http://upload-images.jianshu.io/upload_images/304454-744ea6b765bb7561.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- Debug Memory Graph
在Xcode6中出现了Debug View Hierarchy新特性，可以通过其调试当前App的视图层级。在Xcode8中苹果为开发者提供了Debug Memory Graph特性（👆图片就是），可以直接选择一个对象，查看与其相关的内存关系。Debug Memory Graph和Runtime Issues可以配合使用，通过Debug Memory Graph分析内存关系完成后，点击Runtime Issues可以看到已经发现的内存问题。

![Debug View Hierarchy](http://upload-images.jianshu.io/upload_images/304454-4b48c81051a9ba00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


结束

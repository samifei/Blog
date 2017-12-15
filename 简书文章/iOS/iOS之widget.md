今天偶尔捣鼓手机，翻出这几个东西，就想做下，不废话上图。
![1](http://upload-images.jianshu.io/upload_images/304454-44ecf2f7df333c69.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![2](http://upload-images.jianshu.io/upload_images/304454-7affd03dc3bb5e43.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![3](http://upload-images.jianshu.io/upload_images/304454-1a7bb267f5f4e348.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最后一个最丑的就是我做的，没错最丑那个，没毛病😄。
#准备
iOS extension的出现，方便了用户查看，比如用户可以在Today的widgets中查看应用的某些信息，然后点击进入相关的应用界面。
- 添加Today Extension
（什么？你没找到，创建target会吧，就是那。）
![w1.png](http://upload-images.jianshu.io/upload_images/304454-67771a6600f1faa9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 代码书写
```
//很清楚简单，直接贴出来，其他的可以在storyboard自己设置
//通过extensionContext借助host app调起app
    [self.extensionContext openURL:[NSURL URLWithString:@"widgetsam://sam..."] completionHandler:^(BOOL success) {
        NSLog(@"open url result:%d 🐒 %d",success ,testNumber);
    }];
-(void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    //添加折叠效果
    self.extensionContext.widgetLargestAvailableDisplayMode = NCWidgetDisplayModeExpanded;
}
-(void)widgetActiveDisplayModeDidChange:(NCWidgetDisplayMode)activeDisplayMode withMaximumSize:(CGSize)maxSize {
    /**
     iOS10以后，重新规定了Today Extension的size。宽度是固定(例如在iPhone6上是359)，所以无法改变；但是高度方面，提供了两种模式：
     
     NCWidgetDisplayModeCompact：固定高度，则为110
     
     NCWidgetDisplayModeExpanded：可以变化的高度，区间为110~616
     */
    if (activeDisplayMode == NCWidgetDisplayModeCompact) {
        self.preferredContentSize = CGSizeMake([UIScreen mainScreen].bounds.size.width, 110);
    } else {
        self.preferredContentSize = CGSizeMake([UIScreen mainScreen].bounds.size.width, 250);
    }
}
```
- 关于数据共享
扩展与宿主App是隔离的，那么数据共享就需要使用App Groups。
>在App主Target的Capabilities栏，找到App Groups项，开启功能，并点击“+”符号添加一个共享的数据容器名称，例如group.xxx。然后会发现主Target和扩展Target目录中都生成了一个entitlements类型文件，记录了一个App Groups项。
这个共享的容器，就是存放扩展和宿主App共用的数据的空间。
为了正常编译，还需要前往开发者中心，编辑主应用和扩展的AppID，开启支持App Groups功能，类似于开启推送功能。
配置完成后，就是使用了。不管是采用UserDefaults、Archive、CoreData、FMDB、LevelDB等哪种数据存储或操作方式，只需要将路径指向共享的容器路径就可以。

![App Groups](http://upload-images.jianshu.io/upload_images/304454-3591e62998ccae10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 完整代码
[Demo](https://github.com/samifei/widgetDemo)






结束

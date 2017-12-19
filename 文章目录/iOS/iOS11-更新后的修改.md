⚠️有知道简书上传图片怎么设置大小的吗？我使用的MarkDown编辑的。

![惯例先看女神](http://upload-images.jianshu.io/upload_images/304454-1f4c9f1ba44d033c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 目前为止在 iOS11 beta版本遇到的Bug，每遇到一个问题都做一下记录，持续更新中ing...

1. HTTPS 必须使用TLS1.2

 `
公司项目遇到第一个也是最大一个问题，iOS11后HTTPS 必须使用TLS1.2 。
因为我们一直用的证书双向认证，而加密使用TLS1.0, 证书认证不通过，网络服务一直连不通，后来搭建tomcat测试服务，发现TLS1.2正常，但以为我们产品加密是写到内核中，目前无大牛支持修改。想看看是否还可以使用TLS1.0，最终通过🍎苹果官方，答案看下面。
`

![苹果官方回答](http://upload-images.jianshu.io/upload_images/304454-91b44fa880542065.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. UIBarButtonItem 图片显示与尺寸 @2x @3x 有关
先上代码看下，我用到同一图片，不同的命名
`samli.png  sam@3x.png`
```
    self.title = @"国家电力投资集团公司";
    UIButton * leftBtn = [UIButton buttonWithType:UIButtonTypeCustom];
    leftBtn.frame = CGRectMake(0, 0, 30, 30);
    [leftBtn setBackgroundImage:[UIImage imageNamed:@"sam"] forState:UIControlStateNormal];
    leftBtn.titleLabel.textColor = [UIColor redColor];
    [leftBtn addTarget:self action:@selector(leftBarButtonItemTargetAction) forControlEvents:UIControlEventTouchUpInside];
    UIBarButtonItem * leftBarbtn = [[UIBarButtonItem alloc]initWithCustomView:leftBtn];
    self.navigationItem.leftBarButtonItem = leftBarbtn;
    //
    UIButton * rightBTN = [UIButton buttonWithType:UIButtonTypeCustom];
    rightBTN.frame = CGRectMake(0, 0, 30, 30);
    [rightBTN setBackgroundImage:[UIImage imageNamed:@"samli"] forState:UIControlStateNormal];
    [rightBTN addTarget:self action:@selector(leftBarButtonItemTargetAction) forControlEvents:UIControlEventTouchUpInside];
    UIBarButtonItem * rightBarbtn = [[UIBarButtonItem alloc]initWithCustomView:rightBTN];
    self.navigationItem.rightBarButtonItem = rightBarbtn;
    
    //samli ios11 控制title 位置
    if (@available(iOS 11.0, *)) {
        self.navigationController.navigationBar.prefersLargeTitles = YES;
    } else {
        // Fallback on earlier versions
    }
```
然后看效果


![示例1.png](http://upload-images.jianshu.io/upload_images/304454-abd050288c786340.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![示例2.png](http://upload-images.jianshu.io/upload_images/304454-8eb64b287988fe6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######重要通知！！！
`本人现在用的Xcode Version 9.0 beta 5 (9M202q) ，此版本有关bug，工程中拖入文件 即使 你选择target，工程右边的👉 Target Membership 也是没选中！！！`

3.  [简书App适配iOS 11](http://www.jianshu.com/p/26fc39135c34)
[iOS 11 安全区域适配](http://www.jianshu.com/p/efbc8619d56b)


4. [wkwebView加载mainBundle资源相关本地html 涉及html，css ，js等资源时奔溃](https://github.com/ChenYilong/iOS11AdaptationTips/issues/7)




END

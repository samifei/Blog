![Song Hye Kyo](http://upload-images.jianshu.io/upload_images/304454-71d5d3045c9f67be.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>delegate是经典设计模式也就是大部分的语言都可以实现的模式，delegate只是保存了一个对象指针，直接回调，没有额外消耗.[assign和weak区别](https://my.oschina.net/jlongtian/blog/831481)
MRC @property (nonatomic, assign) id <MyDelegate> delegate; 
ARC @property (nonatomic, weak) id <MyDelegate> delegate;
[block](http://www.jianshu.com/p/17872da184fb)出栈需要将使用的数据从栈拷贝到堆，当然对象的话就是加计数，使用完或者block置nil后才消除。所以我们用block时要进行弱引用：
ARC下：weak typeof(self) weakSelf = self;
非ARC下：block typeof(self) weakSelf = self;
notification 通知的用法相对就是比较简单的,注意添加和删除通知；
[iOS属性中常用修饰词的总结](http://blog.csdn.net/prliu/article/details/51279576)

###Delegate
```
Class A
A.h
@protocol TestViewDelegate <NSObject>
-(void)selectedString:(NSString *)string;
@end
@property(weak, nonatomic) id <TestViewDelegate> testViewDelegate;
A.m
- (IBAction)back:(id)sender {
    if (self.testViewDelegate && [self.testViewDelegate respondsToSelector:@selector(selectedString:)]) {
        [self.testViewDelegate selectedString:@"T - T - Delegate"];
    }
}

```
```
Class B
#pragma mark - Delegate
- (IBAction)buttonClick:(id)sender {
    DelegateViewController *vc = [[DelegateViewController alloc]init];
    vc.testViewDelegate = self;
    [self presentViewController:vc animated:YES completion:nil];
}
-(void)selectedString:(NSString *)string{
    [self dismissViewControllerAnimated:YES completion:nil];//返回上个页面
    NSLog(@"string-- >%@",string);
}
```

###Block
```
Class A
A.h
typedef void (^TestViewblock)(NSString *string);
@interface BlockViewController : UIViewController
@property(nonatomic,strong)TestViewblock testViewBlock;
@end
A.m
- (IBAction)back:(id)sender {
    if (_testViewBlock) {
        _testViewBlock(@"T - T - Block");
    }
}

```
```
Class B
#pragma mark - block
- (IBAction)BlockClick:(id)sender {
    BlockViewController *vc = [[BlockViewController alloc]init];
    [self presentViewController:vc animated:YES completion:nil];
    
    __weak typeof(self) weakSelf=self;//避免block 循环缓存
    vc.testViewBlock=^(NSString *string){
        [weakSelf dismissViewControllerAnimated:YES completion:nil];
        NSLog(@"string-->%@",string);
    };
}
```

###Notification
```
- (IBAction)back:(id)sender {
    [[NSNotificationCenter defaultCenter] postNotificationName:@"test_notification" object:@"T - T - Notification"];
    [self dismissViewControllerAnimated:YES completion:nil];
}
---------------------------------------
#pragma mark - Notification
- (IBAction)NotificationClick:(id)sender
{
    NotificationViewController *vc = [[NotificationViewController alloc]init];
    [self presentViewController:vc animated:YES completion:nil];
    [[NSNotificationCenter defaultCenter] removeObserver:self];//移除通知
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(notification:) name:@"test_notification" object:nil];//添加通知
}
-(void)notification:(NSNotification *)notification{
    NSString *sting = [notification object];
    NSLog(@"sting --->%@",sting);
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}

```

[Github代码链接](https://github.com/samifei/DelegateBlockNotification)

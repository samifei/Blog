- 1 #pragma 预处理指令
> 在C/C++标准中，#pragma是一条预处理的指令（preprocessor directive）。简单地说，#pragma是用来向编译器传达语言标准以外的一些信息。
>在你的 @implementation 中使用 #pragma mark 来将代码分割成逻辑区块。这些逻辑区块不仅仅使得阅读代码本身容易许多，也为Xcode源导航增加了视觉线索（#pragma mark 声明前有一个水平分割并由破折号（－）开始）。如下：

```
#pragma mark - UITableViewDelegate  
```
- 2   #pragma clang diagnostic   clang诊断设置
>在iOS开发中，clang diagnostic（clang 诊断设置） 是#pragma的常用命令：

```
#pragma clang diagnostic push  
#pragma clang diagnostic ignored "-相关命令"  
    // 你自己的代码  
#pragma clang diagnostic pop 
```
[-相关命令](http://fuckingclangwarnings.com/)
- 3  自定义警告Warning 或error
>两种强制警告的方法在视觉效果上结果是一样的，但是警告类型略有不同，一个是-W#pragma-messages，另一个是-W#warnings。对于第二种写法，把warning换成error，可以强制使编译失败。比如在发布一些需要API Key之类的类库时，可以使用这个方法来提示别的开发者别忘了输入必要的信息。

```
#pragma message "Warning" 
#warning "Warning 2" 
#error "Something wrong"
```

了解更多[谈谈Objective-C的警告](https://onevcat.com/2013/05/talk-about-warning/)
###下面来点黑科技（然并卵）
- 屏蔽方法废弃警告
```
#pragma clang diagnostic push    
#pragma clang diagnostic ignored "-Wdeprecated-declarations"        
[TestFlight setDeviceIdentifier:[[UIDevice currentDevice] uniqueIdentifier]];    
#pragma clang diagnostic pop  
```
- 屏蔽不兼容指针类型警告
```
#pragma clang diagnostic push     
#pragma clang diagnostic ignored "-Wincompatible-pointer-types"    
      //code
#pragma clang diagnostic pop  
```
- 屏蔽循环引用警告
```
// completionBlock是手动杀了AFURLConnectionOperation打破保留周期。
// completionBlock is manually nilled out in AFURLConnectionOperation to break the retain cycle.    
#pragma clang diagnostic push    
#pragma clang diagnostic ignored "-Warc-retain-cycles"   
    self.completionBlock = ^ {    
        ...    
    };    
#pragma clang diagnostic pop  
```
- 屏蔽未使用变量警告
```
#pragma clang diagnostic push     
#pragma clang diagnostic ignored "-Wunused-variable"    
    int a;     
#pragma clang diagnostic pop  
```



[转自](http://blog.csdn.net/jancywen/article/details/64444913?utm_source=itdadao&utm_medium=referral)


[关于#pragma](http://nshipster.cn/pragma/)
[#pragma 处理警告](http://www.jianshu.com/p/3c7a4feaee16)
[XCode启用/关闭Clang Warnings](http://www.bubuko.com/infodetail-904751.html)
结束

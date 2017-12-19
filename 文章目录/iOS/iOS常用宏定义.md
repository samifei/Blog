

![秋月](http://upload-images.jianshu.io/upload_images/304454-829fc189e3c70456.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
//--------------------------

//MARK:SamLi 判断当前的iPhone设备／模拟器/系统版本
//判断是否为iPhone
#define IS_IPHONE (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone)

//判断是否为iPad
#define IS_IPAD (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad)

//判断是否为ipod
#define IS_IPOD ([[[UIDevice currentDevice] model] isEqualToString:@"iPod touch"])

// 判断是否为 iPhone 5SE
#define iPhone5SE [[UIScreen mainScreen] bounds].size.width == 320.0f && [[UIScreen mainScreen] bounds].size.height == 568.0f

// 判断是否为iPhone 6/6s
#define iPhone6_6s [[UIScreen mainScreen] bounds].size.width == 375.0f && [[UIScreen mainScreen] bounds].size.height == 667.0f

// 判断是否为iPhone 6Plus/6sPlus
#define iPhone6Plus_6sPlus [[UIScreen mainScreen] bounds].size.width == 414.0f && [[UIScreen mainScreen] bounds].size.height == 736.0f

//获取系统版本
#define IOS_SYSTEM_VERSION [[[UIDevice currentDevice] systemVersion] floatValue]

//判断 iOS 8 或更高的系统版本
#define IOS_VERSION_8_OR_LATER (([[[UIDevice currentDevice] systemVersion] floatValue] >=8.0)? (YES):(NO))

//判断是真机还是模拟器
#if TARGET_OS_IPHONE
//iPhone Device
#endif
#if TARGET_IPHONE_SIMULATOR
//iPhone Simulator
#endif

//MARK: 获取屏幕(或者view) frame
//获取view的frame（不建议使用）
#define kGetViewWidth(view)  view.frame.size.width
#define kGetViewHeight(view) view.frame.size.height
#define kGetViewX(view)      view.frame.origin.x
#define kGetViewY(view)      view.frame.origin.y
//需要横屏或者竖屏，获取屏幕宽度与高度
#if __IPHONE_OS_VERSION_MAX_ALLOWED >= 80000 // 当前Xcode支持iOS8及以上

#define SCREEN_WIDTH    ([[UIScreen mainScreen] respondsToSelector:@selector(nativeBounds)]?[UIScreen mainScreen].nativeBounds.size.width/[UIScreen mainScreen].nativeScale:[UIScreen mainScreen].bounds.size.width)
#define SCREENH_HEIGHT  ([[UIScreen mainScreen] respondsToSelector:@selector(nativeBounds)]?[UIScreen mainScreen].nativeBounds.size.height/[UIScreen mainScreen].nativeScale:[UIScreen mainScreen].bounds.size.height)
#define SCREEN_SIZE     ([[UIScreen mainScreen] respondsToSelector:@selector(nativeBounds)]?CGSizeMake([UIScreen mainScreen].nativeBounds.size.width/[UIScreen mainScreen].nativeScale,[UIScreen mainScreen].nativeBounds.size.height/[UIScreen mainScreen].nativeScale):[UIScreen mainScreen].bounds.size)

#else

#define SCREEN_WIDTH [UIScreen mainScreen].bounds.size.width
#define SCREENH_HEIGHT [UIScreen mainScreen].bounds.size.height
#define SCREEN_SIZE [UIScreen mainScreen].bounds.size

#endif
//MARK: ---View---
//MARK: 获取图片资源
#define kGetImage(imageName) [UIImage imageNamed:[NSString stringWithFormat:@"%@",imageName]

//MARK: SamLi 颜色设置
//设置随机颜色
#define SLRandomColor [UIColor colorWithRed:arc4random_uniform(256)/255.0 green:arc4random_uniform(256)/255.0 blue:arc4random_uniform(256)/255.0 alpha:1.0]
//设置RGB颜色/设置RGBA颜色
#define SLRGBColor(r, g, b) [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:1.0]
#define SLRGBAColor(r, g, b, a) [UIColor colorWithRed:(r)/255.0 green:(r)/255.0 blue:(r)/255.0 alpha:a]
// clear背景颜色
#define SLClearColor [UIColor clearColor]

//MARK: 设置 view 圆角和边框
#define SLViewBorderRadius(View, Radius, Width, Color)\
\
[View.layer setCornerRadius:(Radius)];\
[View.layer setMasksToBounds:YES];\
[View.layer setBorderWidth:(Width)];\
[View.layer setBorderColor:[Color CGColor]]
//MARK: ------
//MARK: 获取通知中心
#define SLNotificationCenter [NSNotificationCenter defaultCenter]

//MARK: 弱引用/强引用
#define SLWeakSelf(type)  __weak typeof(type) weak##type = type;
#define SLStrongSelf(type)  __strong typeof(type) type = weak##type;

//MARK: 由角度转换弧度 由弧度转换角度
#define SLDegreesToRadian(x) (M_PI * (x) / 180.0)
#define SLRadianToDegrees(radian) (radian*180.0)/(M_PI)

//MARK: 获取当前语言
#define SLCurrentLanguage ([[NSLocale preferredLanguages] objectAtIndex:0])
//MARK: ------
//MARK: SamLi 使用 ARC 和 MRC
#if __has_feature(objc_arc)
// ARC code
#else
// MRC code
#endif
//MARK: Debug/Realse 中的NSLog
#ifdef DEBUG
#define SLLog(...) NSLog(@"%s 第%d行 \n %@\n\n",__func__,__LINE__,[NSString stringWithFormat:__VA_ARGS__])
#else
#define SLLog(...)
#endif
//MARK: 判断Target区分，书写代码
//Build Settings ->Preprocessor Macros ->Target_Name
#ifdef Target_Name
//    此target版本添加的代码
#endif
#ifndef Target_Name
//    除去此target版本 ，其他target添加使用的代码
#endif


//MARK: ---沙盒目录 ---
//获取temp
#define kPathTemp NSTemporaryDirectory()

//获取沙盒 Document
#define kPathDocument [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject]

//获取沙盒 Cache
#define kPathCache [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject]

//MARK: ---GCD ---
//GCD - 一次性执行
#define kDISPATCH_ONCE_BLOCK(onceBlock) static dispatch_once_t onceToken; dispatch_once(&onceToken, onceBlock);

//GCD - 在Main线程上运行
#define kDISPATCH_MAIN_THREAD(mainQueueBlock) dispatch_async(dispatch_get_main_queue(), mainQueueBlock);

//GCD - 开启异步线程
#define kDISPATCH_GLOBAL_QUEUE_DEFAULT(globalQueueBlock) dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), globalQueueBlocl);

//-----------------------------------
```

#KVC与KVO
由于ObjC主要基于[Smalltalk](http://baike.baidu.com/item/smalltalk)进行设计，因此它有很多类似于Ruby、Python的动态特性，例如动态类型、动态加载、动态绑定等，他们底层实现机制都是[isa](http://www.jianshu.com/p/3613338728c1)-[swizzing](http://nshipster.com/method-swizzling/)。今天我们介绍ObjC中的 键值编码Key Value Coding（KVC）、键值监听Key Value Observing（KVO）。
##KVC
KVC的操作方法由NSKeyValueCoding协议提供，而NSObject就实现了这个协议，也就是说ObjC中几乎所有的对象都支持KVC操作。
通常我们使用 语法和set方式更改对象的状态，即为对象赋值。
它是一种可以通过字符串的名字（key）来访问类属性的机制，而不是通过调用Setter、Getter方法访问。(貌似说的有矛盾，待思考解决)
- 动态设置： setValue:属性值 forKey:属性名（用于简单路径）、setValue:属性值 forKeyPath:属性路径（用于复合路径，例如Person有一个Account类型的属性，那么person.account就是一个复合属性）
- 动态读取： valueForKey:属性名 、valueForKeyPath:属性名（用于复合路径）




##KVO
```
[p addObserver:<#(NSObject *)#> forKeyPath:<#(NSString *)#> options:<#(NSKeyValueObservingOptions)#> context:<#(void *)#>]
参数说明：
第一个参数：监听器对象
第二个参数：监听的属性
第三个参数：当属性改变时，需要传递什么值给监听器（枚举类型）
监听器需要实现监听方法
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context
keypath：监听的属性
object：谁的属性改变了
change：改变的值或者原值  或者都是  在添加监听的options设置
移除监听器
但监听器是用完之后要进行移除
//删除观察者
[p removeObserver:self forKeyPath:@"name"];
```
（未完。。。）

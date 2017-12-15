> URL转译 stringByAddingPercentEscapesUsingEncoding(只对 `#%^{}[]|\"<> 加空格共14个字符编码，不包括”&?”等符号), ios9将淘汰。
建议用stringByAddingPercentEncodingWithAllowedCharacters方法


```
//中文链接
NSString *globalURL = [[NSString alloc]initWithFormat:@"http://127.0.0.1:%d/oa/workflow/submit/workflow?requestId=181193&userNumber=01281209&type=submit&remark=这是个什么鬼啊，这是个神",ListenPort];
//iOS9.0废弃方法
//NSString * encodingString = [globalURL stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
//转译后的链接
NSString * encodingString = [globalURL stringByAddingPercentEncodingWithAllowedCharacters:[NSCharacterSet URLQueryAllowedCharacterSet]];
```


**stringByAddingPercentEncodingWithAllowedCharacters需要传一个**

**NSCharacterSet对象（关于  **NSCharacterSet** [这篇](http://nshipster.cn/nscharacterset/) 文章说的很好）**

**如[NSCharacterSet  URLQueryAllowedCharacterSet]**
```
@interface NSCharacterSet (NSURLUtilities)
// 预定义字符集用于六个URL组件和子组件，它们允许百分比编码。 
- (nullable NSString *)stringByAddingPercentEncodingWithAllowedCharacters:(NSCharacterSet *)allowedCharacters

// 包含URL用户子组件中允许的字符的字符集
@property (class, readonly, copy) NSCharacterSet *URLUserAllowedCharacterSet API_AVAILABLE(macos(10.9), ios(7.0), watchos(2.0), tvos(9.0));

// 包含URL密码子组件中允许的字符的字符集
@property (class, readonly, copy) NSCharacterSet *URLPasswordAllowedCharacterSet API_AVAILABLE(macos(10.9), ios(7.0), watchos(2.0), tvos(9.0));

// 包含URL的主机子组件中允许的字符的字符集
@property (class, readonly, copy) NSCharacterSet *URLHostAllowedCharacterSet API_AVAILABLE(macos(10.9), ios(7.0), watchos(2.0), tvos(9.0));

// 返回一个包含字符的字符集允许URL的路径组件。字符“;”是一种合法的路径,但是建议最好是percent-encoded兼容NSURL(-stringByAddingPercentEncodingWithAllowedCharacters:percent-encode任何‘;’字符如果你通过URLPathAllowedCharacterSet)
@property (class, readonly, copy) NSCharacterSet *URLPathAllowedCharacterSet API_AVAILABLE(macos(10.9), ios(7.0), watchos(2.0), tvos(9.0));

// 包含URL查询组件中允许的字符的字符集
@property (class, readonly, copy) NSCharacterSet *URLQueryAllowedCharacterSet API_AVAILABLE(macos(10.9), ios(7.0), watchos(2.0), tvos(9.0));

// 包含URL片段组件中允许的字符的字符集
@property (class, readonly, copy) NSCharacterSet *URLFragmentAllowedCharacterSet API_AVAILABLE(macos(10.9), ios(7.0), watchos(2.0), tvos(9.0));

@end

编码字符范围
URLFragmentAllowedCharacterSet  "#%<>[\]^`{|}
URLHostAllowedCharacterSet      "#%/<>?@\^`{|}
URLPasswordAllowedCharacterSet  "#%/:<>?@[\]^`{|}
URLPathAllowedCharacterSet      "#%;<>?[\]^`{|}
URLQueryAllowedCharacterSet     "#%<>[\]^`{|}
URLUserAllowedCharacterSet      "#%/:<>?@[\]^`
```



END

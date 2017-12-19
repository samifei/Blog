此方法只适用于iOS10.3
> // Pass `nil` to use the primary application icon. The completion handler will be invoked asynchronously on an arbitrary background queue; be sure to dispatch back to the main queue before doing any further UI work.
- (void)setAlternateIconName:(nullable NSString *)alternateIconName completionHandler:(nullable void (^)(NSError *_Nullable error))completionHandler NS_EXTENSION_UNAVAILABLE("Extensions may not have alternate icons") API_AVAILABLE(ios(10.3), tvos(10.2));

在info.plist中添加下面信息
```
<key>CFBundleIcons</key>
	<dict>
		<key>CFBundleAlternateIcons</key>
		<dict>
			<key>star</key>
			<dict>
				<key>CFBundleIconFiles</key>
				<array>
					<string>star-29</string>
					<string>star-20</string>
					<string>star-40</string>
					<string>star-60</string>
				</array>
				<key>UIPrerenderedIcon</key>
				<true/>
			</dict>
		</dict>
	</dict>

```

![info.png](http://upload-images.jianshu.io/upload_images/304454-0ac5a6f727aa5e6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
###主要实现代码
```
-(void)setIcon:(NSString *)iconName
{
//默认的icon，iconName =nil;
    NSString *appiconName = [[UIApplication sharedApplication] alternateIconName];
    NSLog(@"目前iconName %@",appiconName);
    
    [[UIApplication sharedApplication]setAlternateIconName:iconName completionHandler:^(NSError * error){
        NSLog(@"samli error %@",error);
    }];
}
```
[代码链接](https://github.com/samifei/IconChange)
END

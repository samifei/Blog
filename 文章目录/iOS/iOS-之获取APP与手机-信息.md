



```
    NSDictionary *infoDictionary = [[NSBundle mainBundle] infoDictionary];
    CFShow(CFBridgingRetain(infoDictionary));//
    // app名称
    NSString *app_Name = [infoDictionary objectForKey:@"CFBundleDisplayName"];
    NSLog(@"应用名字：%@",app_Name);
    // Version版本
    NSString *app_Version = [infoDictionary objectForKey:@"CFBundleShortVersionString"];
    NSLog(@"Version版本：%@",app_Version);
    // build版本
    NSString *app_build = [infoDictionary objectForKey:@"CFBundleVersion"];
    NSLog(@"Build版本：%@",app_build);
    //手机序列号
    NSUUID* identifierNumber = [[UIDevice currentDevice] identifierForVendor];
    NSLog(@"手机序列号: %@",identifierNumber);
    //手机别名：用户定义的名称
    NSString* userPhoneName = [[UIDevice currentDevice] name];
    NSLog(@"手机别名: %@", userPhoneName);
    //系统名称
    NSString* deviceName = [[UIDevice currentDevice] systemName];
    NSLog(@"系统名称: %@",deviceName );
    //手机系统版本
    NSString* phoneVersion = [[UIDevice currentDevice] systemVersion];
    NSLog(@"手机系统版本: %@", phoneVersion);
    //手机型号
    NSString* phoneModel = [[UIDevice currentDevice] model];
    NSLog(@"手机型号: %@",phoneModel );
    //地方型号（国际化区域名称）
    NSString* localPhoneModel = [[UIDevice currentDevice] localizedModel];
    NSLog(@"国际化区域名称: %@",localPhoneModel );
```


![CoreNFC没有标志图标，换上女神的](http://upload-images.jianshu.io/upload_images/304454-0d6684d257a40a10.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>先了解几个概念
什么是NFC？
NDEF指的是什么？
什么是CoreNFC？

###什么是 NFC
NFC（Near Field Communication）即近距离无线通讯技术。该技术由[飞利浦](http://product.cnmo.com/pro_sub_manu/sub_57_manu_159_1.shtml)公司和[索尼](http://product.cnmo.com/pro_sub_manu/sub_57_manu_32831_1.shtml)公司共同开发，可以在移动设备、消费类电子产品、PC 和智能控件工具间进行近距离无线通信。NFC提供了一种简单、触控式的解决方案，可以让消费者简单直观地交换信息、访问内容与服务。
NFC通信技术，允许电子设备之间进行非接触式点对点数据传输（在十厘米内）交换数据。这个技术由免接触式射频识别（RFID）演变而来，并向下兼容RFID，主要用于手机等手持设备中提供[M2M](http://product.cnmo.com/cell_phone/index1622204.shtml)(Machine to Machine)的通信。由于近场通讯具有天然的安全性。
NFC是一种短距高频的无线电技术，在13.56MHz频率运行于10厘米距离内。其传输速度有106 Kbit/秒、212 Kbit/秒或者424 Kbit/秒三种。目前近场通信已通过成为ISO/IEC IS 18092国际标准、ECMA-340标准与ETSI TS 102 190标准。NFC采用主动和被动两种读取模式。
[NFC的百度百科](https://baike.baidu.com/item/nfc/5684?fr=aladdin)
[NFC通信](http://blog.csdn.net/eager7/article/details/8525659)

![模式图](http://upload-images.jianshu.io/upload_images/304454-549567464a4f0f70.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### NDEF指的是什么？
NFC Data Exchange Format : NFC数据交换格式，NFC组织约定的NFC tag中的数据格式。
NDEF是轻量级的紧凑的二进制格式，可带有URL、vCard和NFC定义的各种数据类型。
NDEF的由各种数据记录组成，而各个记录由报头(Header)和有效载荷(Payload)组成，其中NDEF记录的数据类型和大小由记录载荷的报头注明，这里的报头包含3部分，分别为Length、Type和Identifier.。
NFC Data Exchange Format : NFC数据交换格式，NFC组织约定的NFC tag中的数据格式。[NDEF修详解](http://blog.csdn.net/vampire_armand/article/details/39372953)
###什么是CoreNFC？
CoreNFC是苹果推出的支持NFC通讯的框架，仅支持装有iOS 11的iPhone 7和iPhone 7Plus,Xcode 9 beta版。CoreNFC读取的是NDEF标签的数据。`（吐槽下，为毛必须7以上的设备，这是又要换手机的节奏啊）`

##iOS开发部分
#####首先
在你的开发者账号里面添加上对NFC的支持：
（很简单，只需要配置App ID支持NFC，更新Provisioning Profiles）

![开发者账号中设置](http://upload-images.jianshu.io/upload_images/304454-aa18512e5844a6b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#####其次
在你的XCode中添加TARGETS->Capabilities中打开Near Field Communication Tag Reading选项，XCode会自动帮你添加其他步骤
![Xcode.png](http://upload-images.jianshu.io/upload_images/304454-39beed499efece34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#####然后
在你Project的info.plist中添加：
Privacy - NFC Scan Usage Description
NFC usage description
com.apple.developer.nfc.readersession.formats
NDEF
```
<key>NFCReaderUsageDescription</key>
<string>NFC Test</string>
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
      <string>NDEF</string>
</array>
```
![info.png](http://upload-images.jianshu.io/upload_images/304454-3ea5c91f8f7569ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#####最后
代码上也很简单
```
#import "SLViewController.h"
#import <CoreNFC/CoreNFC.h>

@interface SLViewController ()<NFCNDEFReaderSessionDelegate>
@property (strong, nonatomic) NFCNDEFReaderSession *session;
@end
//---------------------------
- (IBAction)NFCAction:(id)sender {
    [self coreNFCAlloc];
}
-(void)coreNFCAlloc{
    [self.session invalidateSession];//关闭以前的Session
    self.session = [[NFCNDEFReaderSession alloc] initWithDelegate:self
                                                            queue:nil
                                         invalidateAfterFirstRead:NO];
    if (NFCNDEFReaderSession.readingAvailable) {
        self.session.alertMessage = @"把卡放到手机背面";
        [self.session beginSession];//启动 Session
    } else {
        NSLog(@"此设备不支持NFC");
    }
}

#pragma mark - NFCNDEFReaderSessionDelegate
- (void)readerSession:(NFCNDEFReaderSession *)session didInvalidateWithError:(NSError *)error{
    // 读取失败
    NSLog(@"%@",error);
    if (error.code == 201) {
        NSLog(@"扫描超时");
    }
    if (error.code == 200) {
        NSLog(@"取消扫描");
    }
}

- (void)readerSession:(NFCNDEFReaderSession *)session didDetectNDEFs:(NSArray*)messages
{
    // 读取成功
    for (NFCNDEFMessage *msg in messages) {
        NSArray *ary = msg.records;
        for (NFCNDEFPayload *rec in ary) {
            
            NFCTypeNameFormat typeName = rec.typeNameFormat;
            NSData *payload = rec.payload;
            NSData *type = rec.type;
            NSData *identifier = rec.identifier;
            
            NSLog(@"TypeName : %d",typeName);
            NSLog(@"Payload : %@",payload);
            NSLog(@"Type : %@",type);
            NSLog(@"Identifier : %@",identifier);
        }
    }
}
```
注意：
1.开启一个session，并且同时只能开启一个
2.App完全在前台模式，切入后台失效
3.session最多扫存活60s，超时必须重启新session






END

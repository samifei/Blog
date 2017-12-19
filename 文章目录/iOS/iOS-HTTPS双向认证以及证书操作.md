![敲的是代码，写的是情怀](http://upload-images.jianshu.io/upload_images/304454-ce133574a9caa448.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>待完善


###双向认证
- 双向认证代理处理
*简单的请求*
```
NSURL *url = [NSURL URLWithString:[NSString stringWithFormat:@"https://192.168.31.63:443"]];
    // 2.创建一个网络请求
    NSURLRequest *request =[NSURLRequest requestWithURL:url];
    // 3.获得会话对象
    NSURLSession *session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:self delegateQueue:[NSOperationQueue mainQueue]];
    // 4.根据会话对象，创建一个Task任务：
    NSURLSessionDataTask *sessionDataTask = [session dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        NSLog(@"从服务器获取到数据");
        /*
         对从服务器获取到的数据data进行相应的处理：
         */
        NSLog(@"data = %@",data);
    }];
    // 5.最后一步，执行任务（resume也是继续执行）:
    [sessionDataTask resume];
    return;
```
*NSURLSessionTaskDelegate或 NSURLSessionDelegate代理处理双向认证*
```
- (void)URLSession:(NSURLSession *)session didReceiveChallenge:(NSURLAuthenticationChallenge *)challenge
 completionHandler:(void (^)(NSURLSessionAuthChallengeDisposition disposition, NSURLCredential * _Nullable credential))completionHandler {
    NSLog(@"证书认证");
    //NSURLAuthenticationMethodClientCertificate
    //NSURLAuthenticationMethodServerTrust
    if ([[[challenge protectionSpace] authenticationMethod]isEqualToString:@"NSURLAuthenticationMethodServerTrust"])
    {
        OSStatus err;
        SecTrustRef trust ;
        SecCertificateRef serverCert;
        SecTrustResultType      trustResult;
        BOOL trusted;
        trust = [[challenge protectionSpace] serverTrust];
        if (SecTrustGetCertificateCount(trust) > 0) {
            serverCert = SecTrustGetCertificateAtIndex(trust, 0);
        }
        
        NSArray *anchors = [NSArray array];
        SecTrustSetAnchorCertificates(trust, (CFArrayRef)anchors);
        err = SecTrustEvaluate(trust,&trustResult);
        trusted = (err == noErr) && ((trustResult == kSecTrustResultProceed) || (trustResult == kSecTrustResultUnspecified));
        
        NSURLCredential* newCredential = [NSURLCredential credentialForTrust:trust];
        completionHandler(NSURLSessionAuthChallengeUseCredential, newCredential);

    } else {
        if ([[[challenge protectionSpace] authenticationMethod]isEqualToString:@"NSURLAuthenticationMethodClientCertificate"])
        {
            SecIdentityRef identity = NULL;
            SecTrustRef trust = NULL;
            NSURLCredential* credential;
            
            NSData * cerData ;
            NSString *cerPath = [[NSBundle mainBundle] pathForResource:@"samp" ofType:@"p12"];//自签名证书
            cerData = [NSData dataWithContentsOfFile:cerPath];
            
            SecCertificateRef certificate = NULL;
            if ([self extractIdentity:&identity andTrust:&trust fromPKCS12Data:cerData])
            {
                SecIdentityCopyCertificate(identity, &certificate);
                const void*certs[] = {certificate};
                CFArrayRef certArray =CFArrayCreate(kCFAllocatorDefault, certs,1,NULL);
                credential =[NSURLCredential credentialWithIdentity:identity certificates:(__bridge  NSArray*)certArray persistence:NSURLCredentialPersistencePermanent];
            }            
            //关键回传NSURLCredential 证书凭证
            //NSURLCredential * getCredential = [NSURLCredential credentialWithIdentity:(SecIdentityRef)identity certificates:nil persistence:NSURLCredentialPersistenceForSession];
            completionHandler(NSURLSessionAuthChallengeUseCredential, credential);
        }
    }
//读取p12文件中的密码
- (BOOL)extractIdentity:(SecIdentityRef*)outIdentity andTrust:(SecTrustRef *)outTrust fromPKCS12Data:(NSData *)inPKCS12Data {
    OSStatus securityError = errSecSuccess;
    //client certificate password
    NSDictionary *optionsDictionary = [NSDictionary dictionaryWithObject:@"123456"
                                                                  forKey:(__bridge id)kSecImportExportPassphrase];
    
    CFArrayRef items = CFArrayCreate(NULL, 0, 0, NULL);
    securityError = SecPKCS12Import((__bridge CFDataRef)inPKCS12Data,(__bridge CFDictionaryRef)optionsDictionary,&items);
    
    if(securityError == 0) {
        CFDictionaryRef myIdentityAndTrust =CFArrayGetValueAtIndex(items,0);
        const void*tempIdentity =NULL;
        tempIdentity= CFDictionaryGetValue (myIdentityAndTrust,kSecImportItemIdentity);
        *outIdentity = (SecIdentityRef)tempIdentity;
        const void*tempTrust =NULL;
        tempTrust = CFDictionaryGetValue(myIdentityAndTrust,kSecImportItemTrust);
        *outTrust = (SecTrustRef)tempTrust;
    } else {
        NSLog(@"Failedwith error code %d",(int)securityError);
        return NO;
    }
    return YES;
}
```

###获取证书
- 获取keychain中的证书


###获取证书内部信息

> 证书获得的公钥(CertificatePublicKey)，序列号(CertificateSerialNumber) 的 NSData 为十六进制字符串.
[iOS-NSData和十六进制字符串之间的相互转换](http://blog.csdn.net/caryaliu/article/details/49283295)
[iOS-获取SecKeyRef的公钥 转NSData](https://stackoverflow.com/questions/16748993/ios-seckeyref-to-nsdata)
[Examining a Certificate苹果文档：获取公钥等的API](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/certificates/examining_a_certificate?language=objc)


- 获取Certificate的Base64，证书中的公钥，序列号
```
CFArrayRef          certificateRef;
int                 identityID;
//SecCertificateRef certificate//证书对象
- (NSString *)getCertificatePublicKey {
    // samli 获取公钥
    NSString *publicKey;
    SecKeyRef derKey  =SecCertificateCopyPublicKey((SecCertificateRef)CFArrayGetValueAtIndex(certificateRef, identityID));
    NSData *data = [self getPublicKeyBitsFromKey:derKey];
    publicKey = [self convertDataToHexStr:data];
    
    return publicKey ;
}
- (NSString *)getCertificateSerialNumber {
    // samli 获取序列号
    CFErrorRef  * error;
    CFDataRef  derData;
    if (@available(iOS 11.0, *)) {
        // CFDataRef SecCertificateCopySerialNumberData(SecCertificateRef certificate, CFErrorRef *error)
        derData = SecCertificateCopySerialNumberData((SecCertificateRef)CFArrayGetValueAtIndex(certificateRef, identityID),error);
    } else {
        // Fallback on earlier versions
        // CFDataRef SecCertificateCopySerialNumber(SecCertificateRef certificate)
        derData = SecCertificateCopySerialNumber((SecCertificateRef)CFArrayGetValueAtIndex(certificateRef, identityID));
    }
    NSData *data = (__bridge NSData *)derData;
    NSString * SerialNumber  = [self convertDataToHexStr:data];
    
    return SerialNumber ;
}
- (NSString*)getCertificateBase64
{
    // samli 获取证书的base64
    CFDataRef    derData  = SecCertificateCopyData((SecCertificateRef)CFArrayGetValueAtIndex(certificateRef, identityID));
    
    NSData *data = (__bridge NSData *)derData;
    unsigned char *pb64 = base64([data bytes],[data length]);
    NSString *certBase64 = [[NSString alloc]initWithBytes:pb64 length:strlen(pb64) encoding:NSUTF8StringEncoding];
    free(pb64);
    
    return certBase64;
}
```
- 上面获取Certificate信息用到方法
```
- (NSString *)convertDataToHexStr:(NSData *)data {
    if (!data || [data length] == 0) {
        return @"";
    }
    NSMutableString *string = [[NSMutableString alloc] initWithCapacity:[data length]];
    
    [data enumerateByteRangesUsingBlock:^(const void *bytes, NSRange byteRange, BOOL *stop) {
        unsigned char *dataBytes = (unsigned char*)bytes;
        for (NSInteger i = 0; i < byteRange.length; i++) {
            NSString *hexStr = [NSString stringWithFormat:@"%x", (dataBytes[i]) & 0xff];
            if ([hexStr length] == 2) {
                [string appendString:hexStr];
            } else {
                [string appendFormat:@"0%@", hexStr];
            }
        }
    }];
    return string;
}
- (NSData *)getPublicKeyBitsFromKey:(SecKeyRef)givenKey {
    
    static const uint8_t publicKeyIdentifier[] = "com.your.company.publickey";
    NSData *publicTag = [[NSData alloc] initWithBytes:publicKeyIdentifier length:sizeof(publicKeyIdentifier)];
    
    OSStatus sanityCheck = noErr;
    NSData * publicKeyBits = nil;
    
    NSMutableDictionary * queryPublicKey = [[NSMutableDictionary alloc] init];
    [queryPublicKey setObject:(__bridge id)kSecClassKey forKey:(__bridge id)kSecClass];
    [queryPublicKey setObject:publicTag forKey:(__bridge id)kSecAttrApplicationTag];
    [queryPublicKey setObject:(__bridge id)kSecAttrKeyTypeRSA forKey:(__bridge id)kSecAttrKeyType];
    
    // Temporarily add key to the Keychain, return as data:
    NSMutableDictionary * attributes = [queryPublicKey mutableCopy];
    [attributes setObject:(__bridge id)givenKey forKey:(__bridge id)kSecValueRef];
    [attributes setObject:@YES forKey:(__bridge id)kSecReturnData];
    CFTypeRef result;
    sanityCheck = SecItemAdd((__bridge CFDictionaryRef) attributes, &result);
    if (sanityCheck == errSecSuccess) {
        publicKeyBits = CFBridgingRelease(result);
        
        // Remove from Keychain again:
        (void)SecItemDelete((__bridge CFDictionaryRef) queryPublicKey);
    }
    
    return publicKeyBits;
}
/*  samli 此方法使用的 openssl.fremework  */
unsigned char *base64(const void *input, int length)
{
    BIO *bmem, *b64;
    BUF_MEM *bptr;
    
    b64 = BIO_new(BIO_f_base64());
    bmem = BIO_new(BIO_s_mem());
    b64 = BIO_push(b64, bmem);
    BIO_write(b64, input, length);
    BIO_flush(b64);
    BIO_get_mem_ptr(b64, &bptr);
    
    unsigned char *buff = (unsigned char *)malloc(bptr->length);
    memcpy(buff, bptr->data, bptr->length-1);
    buff[bptr->length-1] = 0;
    
    BIO_free_all(b64);
    return buff;
}
```



End

![这就是爱一个人的眼神吧！](http://upload-images.jianshu.io/upload_images/304454-3d5443d38a661b34.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 本文主要列出简单的图片处理代码，如：压缩图形大小，裁剪图片，添加文字水印，添加图片水印，压缩图片大小并保存。
```
//两张图片
slimage = [UIImage imageNamed:@"标准图.png"];
lfimage = [UIImage imageNamed:@"水印图.png"];

//压缩图形大小
[self scaleToSize:slimage size:CGSizeMake(100, 100)];
//裁剪图片
[self cutImage:slimage withRect:CGRectMake(300, 300, 100, 100)];
//添加文字水印
[self addImage:slimage text:@"SamLi" withFont:[UIFont systemFontOfSize:40.0] withRect:CGRectMake(20,20, 200, 200)];
//添加图片水印
[self addToImage:slimage image:lfimage withRect:CGRectMake(20,20, 200, 200)];

//压缩图片并保存
[self zipImageData:slimage];
```
```
//具体功能实现
#pragma mark -
//压缩图形
- (UIImage *)scaleToSize:(UIImage *)img size:(CGSize)size
{
    // 设置成为当前context
    UIGraphicsBeginImageContext(size);
    // 绘制改变大小的图片
    [img drawInRect:CGRectMake(0, 0, size.width, size.height)];
    // 从当前context中创建一个改变大小后的图片
    UIImage* scaledImage = UIGraphicsGetImageFromCurrentImageContext();
    // 使当前的context出堆栈
    UIGraphicsEndImageContext();
    return scaledImage;
}

//截图图片
- (UIImage *)cutImage:(UIImage *)image withRect:(CGRect )rect
{
    CGImageRef imageRef = CGImageCreateWithImageInRect([image CGImage], rect);
    UIImage * img = [UIImage imageWithCGImage:imageRef];
    CGImageRelease(imageRef);
    return img;
}

//压缩图片保存
- (void)zipImageData:(UIImage *)image
{
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"yyyyMMddHHSSS"];
    NSString *currentDateStr = [dateFormatter stringFromDate:[NSDate date]];
    NSString *dateStr = [NSString stringWithFormat:@"%@.jpg",currentDateStr];
    
    NSString *path = [NSTemporaryDirectory() stringByAppendingPathComponent:dateStr];
    if ([[NSFileManager defaultManager] fileExistsAtPath:path]) {
        NSError *error;
        [[NSFileManager defaultManager] removeItemAtPath:path error:&error];
    }
    NSData *imgData = UIImageJPEGRepresentation(image, 1);
    
    if([imgData writeToFile:path atomically:YES])
    {
        NSLog(@"saveSuccess");
    }
}
//加文字水印
- (UIImage *) addImage:(UIImage *)img text:(NSString *)mark withFont:(UIFont *)font withRect:(CGRect)rect
{
    int w = img.size.width;
    int h = img.size.height;
    
    UIGraphicsBeginImageContext(img.size);
    [[UIColor redColor] set];
    [img drawInRect:CGRectMake(0, 0, w, h)];
    
    if([[[UIDevice currentDevice]systemName]floatValue] >= 7.0)
    {
        //ios 7.0以上
        NSDictionary* dic = [NSDictionary dictionaryWithObjectsAndKeys:font, NSFontAttributeName,[UIColor blueColor] ,NSForegroundColorAttributeName,nil];
        [mark drawInRect:rect withAttributes:dic];
    }
    else
    {
        //7.0及其以后都废弃了,完全可以不写这个判断
        [mark drawInRect:rect withFont:font];
    }
    
    UIImage *aimg = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    return aimg;
}

//加图片水印
- (UIImage *) addToImage:(UIImage *)img image:(UIImage *)newImage withRect:(CGRect)rect
{
    int w = img.size.width;
    int h = img.size.height;
    UIGraphicsBeginImageContext(img.size);
    [img drawInRect:CGRectMake(0, 0, w, h)];
    [newImage drawInRect:rect];
    
    UIImage *aimg = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    
    return aimg;
}

```








*本来想放GitHub，不过看看代码都在这了，还传个毛线😄*


END

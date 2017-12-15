![愿与索大一样坚持✊](http://upload-images.jianshu.io/upload_images/304454-d095405eb381afba.JPG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> 想了一段时间该如何着手去写ARKit的东西（并不是没学一直瞎想呢😄），还是觉着吧，先把ARKit 捋一遍。
```
//先揭露一下ARKit 中的类
#import <ARKit/ARError.h>

#import <ARKit/ARSession.h>
#import <ARKit/ARConfiguration.h>

#import <ARKit/ARFrame.h>
#import <ARKit/ARCamera.h>
#import <ARKit/ARHitTestResult.h>
#import <ARKit/ARLightEstimate.h>
#import <ARKit/ARPointCloud.h>

#import <ARKit/ARAnchor.h>
#import <ARKit/ARPlaneAnchor.h>

#import <ARKit/ARFaceAnchor.h>
#import <ARKit/ARFaceGeometry.h>

#import <ARKit/ARSCNView.h>
#import <ARKit/ARSKView.h>
```

### ARKit 简单 类 说明

##### ARError
```
顾名思义，一些error的说明
typedef NS_ERROR_ENUM(ARErrorDomain, ARErrorCode) {
    /** Unsupported configuration. 不支持的配置*/
    ARErrorCodeUnsupportedConfiguration   = 100,
    
    /** A sensor required to run the session is not available. 运行所需的传感器是不可用的*/
    ARErrorCodeSensorUnavailable          = 101,
    
    /** A sensor failed to provide the required input.传感器无法提供输入 */
    ARErrorCodeSensorFailed               = 102,
    
    /** App does not have permission to use the camera. The user may change this in settings. 没有使用摄像头的权限。在设置中更改。*/
    ARErrorCodeCameraUnauthorized         = 103,
    
    /** World tracking has encountered a fatal error.发现一个致命的错误 */
    ARErrorCodeWorldTrackingFailed        = 200,
};
```

####Camera and Scene Details
##### ARFrame
[ARFrame SDK](https://developer.apple.com/documentation/arkit/arframe?language=objc)
视频图像和位置跟踪信息作为AR会话的一部分被捕获

```
此类中 *属性* 都是readonly
//---Accessing Captured Video Frames---
@property(nonatomic, readonly)  CVPixelBufferRef  capturedImage;
@property (nonatomic, readonly) NSTimeInterval timestamp;
@property (nonatomic, strong, readonly, nullable) AVDepthData *capturedDepthData;
@property (nonatomic, readonly) NSTimeInterval capturedDepthDataTimestamp;

//---Examining Scene Parameter---


```
#####ARCamera
[ARCamera](https://developer.apple.com/documentation/arkit/arcamera?language=objc)
```
-
```
#####ARLightEstimate
[ARLightEstimate](https://developer.apple.com/documentation/arkit/arlightestimate?language=objc)
```
- 
```

#####ARDirectionalLightEstimate
[ARDirectionalLightEstimate](https://developer.apple.com/documentation/arkit/ardirectionallightestimate?language=objc)
```
- 
```


END

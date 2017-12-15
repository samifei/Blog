之前Xcode准备建立模块就不说是，直接上个图
![fm1.png](http://upload-images.jianshu.io/upload_images/304454-ae1d07f9303244ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![fm4.png](http://upload-images.jianshu.io/upload_images/304454-0b6cdcbc063c7065.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![fm5.png](http://upload-images.jianshu.io/upload_images/304454-32e97216545a9b61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![fm2.png](http://upload-images.jianshu.io/upload_images/304454-1eb1b74bafdeaba8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![fm3.png](http://upload-images.jianshu.io/upload_images/304454-5aed544588d8996f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

贴出要加入的shell脚本，方便实用。
```
# Sets the target folders and the final framework product.
# 如果工程名称和Framework的Target名称不一样的话，要自定义FMKNAME
# 例如: FMK_NAME = "MyFramework"
FMK_NAME="SLFMWK"
# Install dir will be the final output to the framework.
# The following line create it in the root folder of the current project.
INSTALL_DIR=${SRCROOT}/Products/${FMK_NAME}.framework
# Working dir will be deleted after the framework creation.
WRK_DIR=build
DEVICE_DIR=${WRK_DIR}/Release-iphoneos/${FMK_NAME}.framework
SIMULATOR_DIR=${WRK_DIR}/Release-iphonesimulator/${FMK_NAME}.framework
# -configuration ${CONFIGURATION}
# Clean and Building both architectures.
xcodebuild -configuration "Release" -target "${FMK_NAME}" -sdk iphoneos clean build
xcodebuild -configuration "Release" -target "${FMK_NAME}" -sdk iphonesimulator clean build
# Cleaning the oldest.
if [ -d "${INSTALL_DIR}" ]
then
rm -rf "${INSTALL_DIR}"
fi
mkdir -p "${INSTALL_DIR}"
cp -R "${DEVICE_DIR}/" "${INSTALL_DIR}/"
# Uses the Lipo Tool to merge both binary files (i386 + armv6/armv7) into one Universal final product.
lipo -create "${DEVICE_DIR}/${FMK_NAME}" "${SIMULATOR_DIR}/${FMK_NAME}" -output "${INSTALL_DIR}/${FMK_NAME}"
rm -r "${WRK_DIR}"
open "${INSTALL_DIR}"
```
打包 .a的合并静态库
```
if [ "${ACTION}" = "build" ]
then



#要build的target名
target_Name=${PROJECT_NAME}
echo "target_Name=${target_Name}"


#build之后的文件夹路径
build_DIR=${SRCROOT}/build
echo "build_DIR=${build_DIR}"

#真机build生成的头文件的文件夹路径
DEVICE_DIR_INCLUDE=${build_DIR}/Release-iphoneos/include/${PROJECT_NAME}
echo "DEVICE_DIR_INCLUDE=${DEVICE_DIR_INCLUDE}"

#真机build生成的.a文件路径
DEVICE_DIR_A=${build_DIR}/Release-iphoneos/lib${PROJECT_NAME}.a
echo "DEVICE_DIR_A=${DEVICE_DIR_A}"

#模拟器build生成的.a文件路径
SIMULATOR_DIR_A=${build_DIR}/Release-iphonesimulator/lib${PROJECT_NAME}.a
echo "SIMULATOR_DIR_A=${SIMULATOR_DIR_A}"


#目标文件夹路径
INSTALL_DIR=${SRCROOT}/Products/${PROJECT_NAME}
echo "INSTALL_DIR=${INSTALL_DIR}"

#目标头文件文件夹路径
INSTALL_DIR_Headers=${SRCROOT}/Products/${PROJECT_NAME}/Headers
echo "INSTALL_DIR_Headers=${INSTALL_DIR_Headers}"

#目标.a路径
INSTALL_DIR_A=${SRCROOT}/Products/${PROJECT_NAME}/lib${PROJECT_NAME}.a
echo "INSTALL_DIR_A=${INSTALL_DIR_A}"


#判断build文件夹是否存在，存在则删除
if [ -d "${build_DIR}" ]
then
rm -rf "${build_DIR}"
fi

#判断目标文件夹是否存在，存在则删除该文件夹
if [ -d "${INSTALL_DIR}" ]
then
rm -rf "${INSTALL_DIR}"
fi
#创建目标文件夹
mkdir -p "${INSTALL_DIR}"


#build之前clean一下
xcodebuild -target ${target_Name} clean

#模拟器build
xcodebuild -target ${target_Name} -configuration Release -sdk iphonesimulator

#真机build
xcodebuild -target ${target_Name} -configuration Release -sdk iphoneos

#复制头文件到目标文件夹
cp -R "${DEVICE_DIR_INCLUDE}" "${INSTALL_DIR_Headers}"

#合成模拟器和真机.a包
lipo -create "${DEVICE_DIR_A}" "${SIMULATOR_DIR_A}" -output "${INSTALL_DIR_A}"

#打开目标文件夹
open "${INSTALL_DIR}"


fi
```
实用framework时候如果有 类别什么的，需要在Xcode的Build Settings下Other Linker Flags里面加入-ObjC标志。Objective-C的一个重要特性：类别（category)有关。根据这里的解释，Unix的标准静态库实现和Objective-C的动态特性之间有一些冲突：Objective-C没有为每个函数（或者方法）定义链接符号，它只为每个类创建链接符号。这样当在一个静态库中使用类别来扩展已有类的时候，链接器不知道如何把类原有的方法和类别中的方法整合起来，就会导致你调用类别中的方法时，出现"selector not recognized"，也就是找不到方法定义的错误。为了解决这个问题，引入了-ObjC标志，它的作用就是将静态库中所有的和对象相关的文件都加载进来。
本来这样就可以解决问题了，不过在64位的Mac系统或者iOS系统下，链接器有一个bug，会导致只包含有类别的静态库无法使用-ObjC标志来加载文件。变通方法是使用-all_load 或者-force_load标志，它们的作用都是加载静态库中所有文件，不过all_load作用于所有的库，而-force_load后面必须要指定具体的文件。
（还有问题的话可能加入第三方库路径问题）

[github地址](https://github.com/samifei/SLframework.git)





结束

---
layout: post
title: "IOS实用代码整理——UIImage&amp;UIColor部分"
date: 2014-07-31 17:18:00 +0800
comments: true
categories: 
---

整理了一些在开发中会常用到的代码，主要涉及以下内容：   

- UIImage常用宏  
- UIImage的本地存取
- UIColor取色宏
- UIColor16位色彩的快速设置，可以通过类似`@"#66ccff"`的十六进制代码获取相应颜色值

<!--more-->

###UIImage

####UIImage常用宏

```
//读取本地图片
#define LOADIMAGE(file,ext) [UIImage imageWithContentsOfFile:[[NSBundle mainBundle]pathForResource:file ofType:ext]]
//定义UIImage对象
#define IMAGE(A) [UIImage imageWithContentsOfFile:[[NSBundle mainBundle] pathForResource:A ofType:nil]]
//定义UIImage对象
#define ImageNamed(_pointer) [UIImage imageNamed:[UIUtil imageName:_pointer]]
```

####UIImage的本地存取

- 将图片保存至本地

```
+ (void) saveImage:(UIImage *)image withFileName:(NSString *)imageName ofType:(NSString *)extension inDirectory:(NSString *)directoryPath {
    if ([[extension lowercaseString] isEqualToString:@"png"]) {
        [UIImagePNGRepresentation(image) writeToFile:[directoryPath stringByAppendingPathComponent:[NSString stringWithFormat:@"%@.%@", imageName, @"png"]] options:NSAtomicWrite error:nil];
    } else if ([[extension lowercaseString] isEqualToString:@"jpg"] || [[extension lowercaseString] isEqualToString:@"jpeg"]) {
        [UIImageJPEGRepresentation(image, 1.0) writeToFile:[directoryPath stringByAppendingPathComponent:[NSString stringWithFormat:@"%@.%@", imageName, @"jpg"]] options:NSAtomicWrite error:nil];
    } else {
        //ALog(@"Image Save Failed\nExtension: (%@) is not recognized, use (PNG/JPG)", extension);
        NSLog(@"文件后缀不认识");
    }
}
```

- 从指定路径读取图片

```
+ (UIImage *) loadImage:(NSString *)fileName ofType:(NSString *)extension inDirectory:(NSString *)directoryPath {
    UIImage * result = [UIImage imageWithContentsOfFile:[NSString stringWithFormat:@"%@/%@.%@", directoryPath, fileName, extension]];
    return result;
}
```



- 将图片保存到默认文档路径

```
+ (void)saveImage:(UIImage *)image withFileName:(NSString *)imageName ofType:(NSString *)extension
{
    NSString * documentsDirectoryPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
    NSLog(@"保存路径:%@",documentsDirectoryPath);
    [self saveImage:image withFileName:imageName ofType:extension inDirectory:documentsDirectoryPath];
}
```
- 从默认文档路径读取图片

```
+ (UIImage *) loadImage:(NSString *)fileName ofType:(NSString *)extension
{
    NSString * documentsDirectoryPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
    
    //Load Image From Directory
    UIImage * image = [self loadImage:fileName ofType:extension inDirectory:documentsDirectoryPath];
    
    return image;
}
```

- 判断指定图片是否存在

```
+ (BOOL)imageIsExistInDirectoryWithImageName:(NSString *)imageName type:(NSString *)extension
{
    NSString * documentsDirectoryPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
    
    //Load Image From Directory
    UIImage * image = [self loadImage:imageName ofType:extension inDirectory:documentsDirectoryPath];
    
    if (imageFromWeb == nil) {
        return NO;
    }else{
        return YES;
    }
}
```

- 删除图片

```
+ (BOOL)deleteImageWithImageName:(NSString *)imageName type:(NSString *)extension
{
    NSString * documentsDirectoryPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
    NSString * imagePath = [NSString stringWithFormat:@"%@/%@.%@", documentsDirectoryPath, imageName, extension];
    
    NSFileManager *fileManager = [NSFileManager defaultManager];
    NSError *error;
    BOOL success = [fileManager removeItemAtPath:imagePath error:&error];
    if (success) {
        return YES;
    }
    else
    {
        NSLog(@"Could not delete file -:%@ ",[error localizedDescription]);
        return NO;
    }
}
```


------

###UIColor

####取色宏

```
//获取RGBA颜色
#define RGBA(r, g, b, a)    [UIColor colorWithRed:r/255.0f green:g/255.0f blue:b/255.0f alpha:a]
//获取RGB颜色
#define RGB(r,g,b)          RGBA(r,g,b,1.0f)
```

####随机取色

```
- (UIColor *)randomColor {
    static BOOL seeded = NO;
    if (!seeded) {
        seeded = YES;
        (time(NULL));
    }
    CGFloat red = (CGFloat)random() / (CGFloat)RAND_MAX;
    CGFloat green = (CGFloat)random() / (CGFloat)RAND_MAX;
    CGFloat blue = (CGFloat)random() / (CGFloat)RAND_MAX;
    return [UIColor colorWithRed:red green:green blue:blue alpha:1.0f];
}
```

####16位颜色值

- 16进制字符串转换为UIColor对象，16进制字符串要以#开始，例子：`@"#66CCFF"`

```
+ (UIColor *)getColorFromHex:(NSString *)hexColor
{
    // 转换成标准16进制数
    NSMutableString * colorStr = [[NSMutableString alloc]initWithString:hexColor];
    [colorStr replaceCharactersInRange:[colorStr rangeOfString:@"#" ] withString:@"0x"];
    // 十六进制字符串转成整形。
    long colorLong = strtoul([colorStr cStringUsingEncoding:NSUTF8StringEncoding], 0, 16);
    // 通过位与方法获取三色值
    int R = (colorLong & 0xFF0000 )>>16;
    int G = (colorLong & 0x00FF00 )>>8;
    int B =  colorLong & 0x0000FF;
    //string转color
    return [UIColor colorWithRed:R/255.0 green:G/255.0 blue:B/255.0 alpha:1.0];
}
```
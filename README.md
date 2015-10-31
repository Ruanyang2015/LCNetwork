# LCNetwork

![License MIT](https://img.shields.io/dub/l/vibe-d.svg)
![Pod version](http://img.shields.io/cocoapods/v/LCNetwork.svg?style=flat)
![Platform info](http://img.shields.io/cocoapods/p/LCNetwork.svg?style=flat)


基于 `AFNetworking` 的封装，参考了[YTKNetwork](https://github.com/yuantiku/YTKNetwork)的实现方式，
采用离散型的API调用方式。

##功能
1. 支持`block`和`delegate`的回调方式
2. 支持设置主、副两个服务器地址
3. 支持`response`缓存，基于`TMCache`
4. 支持统一的`argument`加工
5. 支持统一的`response`加工
6. 支持检查返回 JSON 内容的合法性
7. 支持多个请求同时发送，并统一设置它们的回调
8. 支持以类似于插件的形式显示HUD

`ViewController`层的调用例子如下

```
Api2 *api2 = [[Api2 alloc] init];
api2.requestArgument = @{
                         @"lat" : @"34.345",
                         @"lng" : @"113.678"
                         };
[api2 startWithCompletionBlockWithSuccess:^(Api2 *api2) {
    self.weather2.text = api2.responseJSONObject[@"Weather"];
} failure:NULL];

```


##集成

Cocoapods:
```
pod 'LCNetwork'
```

##使用
###统一配置

__`LCNetworkConfig` 提供的两个功能：__

1. 设置服务器地址
2. 设置是否打印请求的log信息

```
LCNetworkConfig *config = [LCNetworkConfig sharedInstance];
config.mainBaseUrl = @"http://api.zdoz.net/";// 设置主服务器地址
config.viceBaseUrl = @"https://api.zdoz.net/";// 设置副服务器地址
config.logEnabled = YES;// 是否打印请求的log信息
```

###创建接口调用类
每个请求都需要一个对应的类去执行，这样的好处是接口所需要的信息都集成到了这个API类的内部，不在暴露在Controller层。创建一个API类需要继承`LCBaseRequest`类，并且遵守`LCAPIRequest`协议，下面是最基本的API类的创建。

__Api1.h__

```
#import <LCNetwork/LCBaseRequest.h>

@interface Api1 : LCBaseRequest<LCAPIRequest>

@end
```
__Api1.m__
```
#import "Api1.h"

@implementation Api1
// 参数属性
@synthesize requestArgument;

// 接口地址
- (NSString *)apiMethodName{
    return @"getweather.aspx";
}

// 请求方式
- (LCRequestMethod)requestMethod{
    return LCRequestMethodGet;
}

// 是否缓存数据
- (BOOL)withoutCache{
    return YES;
}

@end
```
###调用
```
Api1 *api1 = [[Api1 alloc] init];
api1.requestArgument = @{@"cityName" : @"杭州"};
[api1 startWithCompletionBlockWithSuccess:^(Api1 *api1) {
  ...
} failure:^(id request) {
  ...
    }];
```
##更多信息
参考自带的 Demo 或是我的[博客](http://bawn.github.io/ios/afnetworking/2015/08/10/LCNetwork.html)

##Requirements
* iOS 6 or higher
* ARC


##License
```
The MIT License (MIT)

Copyright (c) 2015 Bawn

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

```

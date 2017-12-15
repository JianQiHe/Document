### 1、Podfile 文件
    对于普通用户来说，使用CocoaPods我们打交道最多的就是Podfile文件。CocoaPods是用ruby实现的，因此Podfile文件的语法就是ruby的语法。接着从以下几个方面来介绍Podfile:
### Podfile文件存放位置
- 通常情况下我们都推荐Podfile文件都放在工程根目录.
- 事实上Podfile文件可以放在任意一个目录下，需要做的是在Podfile中指定工程的路径，和原来相比，Podfile文件就在最开始的位置增加了一行，具体内容如下：
- 指定路径使用的是xcodeproj关键字。
- 此后，进入Podfile文件所在路径，执行pod install命令就会和之前一样下载这些Pods依赖库，而且生成的相关文件都放在了Podfile所在目录下面，如下图：

### 使用Podfile 管理Pods依赖库版本
再引入依赖库时，需要显示或隐式注明引用的依赖库版本，具体写法和表示含义如下：

```
pod 'AFNetworking'              // 不显示指定依赖库版本，表示每次都获取最新版本
pod 'AFNetworking', '2.0'       // 只使用2.0版本
pod 'AFNetworking', '> 2.0'     // 使用高于2.0的版本
pod 'AFNetworking', '>= 2.0'    // 使用大于或等于2.0的版本
pod 'AFNetworking', '< 2.0'     // 使用小于2.0的版本
pod 'AFNetworking', '<= 2.0'    // 使用小于或等于2.0的版本
pod 'AFNetworking', '~> 0.1.2'  // 使用大于等于0.1.2，但小于0.2的版本
pod 'AFNetworking', '~> 0.1'    // 使用大于等于0.1，但小于1.0的版本
pod 'AFNetworking', '~> 0'      // 高于0的版本
```

### 2、Podfile.lock 文件
- 在开始使用CocoaPods，执行完pod install之后，会生成一个Podfile.lock文件。这个文件看起来跟我们关系不大，实际上绝对不应该忽略它。
- 该文件用于保存已经安装的Pods依赖库的版本，通过CocoaPods安装了SBJson、AFNetworking、Reachability三个POds依赖库以后对应的Podfile.lock文件内容为：
```
```

Podfile.lock文件最大得用处在于多人开发。对于没有在Podfile中指定Pods依赖库版本的写法，如下：
`pod 'SBJson'`
- 该句话用于获取当前SBJson这个Pods依赖库的最新版本。
- 当团队中的某个人执行完pod install命令后，生成的Podfile.lock文件就记录下了当时最新Pods依赖库的版本，这时团队中的其它人check下来这份包含Podfile.lock文件的工程以后，再去执行pod install命令时，获取下来的Pods依赖库的版本就和最开始用户获取到的版本一致。如果没有Podfile.lock文件，后续所有用户执行pod install命令都会获取最新版本的SBJson，这就有可能造成同一个团队使用的依赖库版本不一致，这对团队协作来说绝对是个灾难！
- 在这种情况下，如果团队想使用当前最新版本的SBJson依赖库，有两种方案：
- 更改Podfile，使其指向最新版本的SBJson依赖库；
- 执行pod update命令；
- 鉴于Podfile.lock文件对团队协作如此重要，我们需要将它添加到版本管理中。

### 3、cocoapods 常用命令
- 根据Podfile 文件指定的内容，安装依赖库，如果有Podfile.lock 文件而且对应的Podfile文件未被修改，则会根据Podfile.lock文件指定的版本安装。
每次更新了Podfile文件时，都需要重新执行该命令，以便重新安装Pods依赖库。
- pod update
若果Podfile中指定的依赖库版本不是写死的，当对应的依赖库有了更新，无论有没有Podfile.lock文件都会去获取Podfile文件描述的允许获取到的最新依赖库版本。
- pod search
`pod search OpenUDID`
从命令的名称不难看出，该命令是用来按名称搜索可用的Pods依赖库，执行结果如下：
- pod setup
`pod setup`
接下来还会打印很多更新信息。
这条命令用于更新本地电脑上的保存的Pods依赖库tree。由于每天有很多人会创建或者更新Pods依赖库，这条命令执行的时候会相当慢，还请耐心等待。我们需要经常执行这条命令，否则有新的Pods依赖库的时候执行pod search命令是搜不出来的。

### pod search 失败的解决方案
- 所有的项目的Podspec文件都托管在https://github.com/CocoaPods/Specs。执行pod setup操作,CocoaPods 会将这些podspec索引文件更新到本地的~/.cocoapods/目录下，pod setup成功后会生成~/Library/Caches/CocoaPods/search_index.json文件。所以我们先执行pod setup

- pod search XXX 还是失败的情况下，删除json文件 rm ~/Library/Caches/CocoaPods/search_index.json

- 然后再pod search XXX


## 引用文档
### cocoapods 文档
> http://blog.csdn.net/xdrt81y/article/details/30631595
### 制作cocoapods文档
> http://www.cocoachina.com/game/20170426/19131.html
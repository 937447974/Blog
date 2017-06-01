##什么是预处理宏

开始正题前，我先说下简单什么是预处理宏，因为可能有些人不知道。先上一段例子方便理解：

```objc
[JSPatch startWithAppKey:@"YOU_GUESS"];
#ifdef DEBUG
[JSPatch setupDevelopment];
#endif
[JSPatch sync];
```

代码很简单，对 JSPatch 这个库的初始化。 `DEBUG` 在这里就是提前定义好的预处理宏。通过结合 `#ifdef`，可以只在编译测试版的时候设置 JSPatch 为 `Development` 状态。当要发布正式版的时候也不用修改代码，直接编译就行，中间那句 `[JSPatch setupDevelopment]`;不会出现在代码里。
看明白了吧，预处理宏的主要就是用来有目的的引入或移除一部分功能（代码）。

##本文的使用场景

一个第三方库会有很多功能，其中有一部分功能需要在编译阶段就决定是否引入。比如 IDFA，Apple 要求使用的话需要在提交审核的时候声明，不然就被拒。此时如果应用不用，那就会被你拖累。所以需要提供一个方法从代码里删除，这就需要用到预处理宏。用类似上面的方式改好后，让用户在 Build Settings 里设置一下就 OK。
如果这个库支持 CocoaPods，可以建一个 subspec 省去用户手动修改：

```rb
s.subspec 'IDFA' do |f|
  f.dependency 'YOUR_SPEC/core'
  f.pod_target_xcconfig = { 'GCC_PREPROCESSOR_DEFINITIONS' => 'ENABLE_IDFA=1'}
end
```

当有多个预处理宏需要设置，可以都写在这一个里面。
可如果不想写在一起，想让用户自己选择开启某些的话，怎么办？
答案很简单，多写几个 subspec。用户需要哪个，就引入哪个。具体例子继续看。

##Subspec 的灵活配置

假设我们有两个功能需要预处理宏来开关，那 podspec 这么写

```pod
Pod::Spec.new do |s|

  #设置 podspec 的默认 subspec
  s.default_subspec = 'core'
  #主要 subspec
  s.subspec 'core' do |c|
    c.source_files  = "*.{h,m}"
    c.public_header_files = "*.h"
    c.frameworks = 'UIKit',
    c.libraries = 'icucore', 'sqlite3', 'z'
    c.platform = :ios, "7.0"
  end
  #功能1，引入则开启
  s.subspec 'IDFA' do |f|
    f.dependency 'YOUR_SPEC/core'
    f.pod_target_xcconfig = { 'GCC_PREPROCESSOR_DEFINITIONS' => 'ENABLE_IDFA=1'}
  end

  #功能2，引入则开启
  s.subspec 'IDFB' do |f|
    f.dependency 'YOUR_SPEC/core'
    f.pod_target_xcconfig = { 'GCC_PREPROCESSOR_DEFINITIONS' => 'ENABLE_IDFB=1'}
  end  

end
```

这里面通过两个 subpec 来开关功能。当用户用的时候，则在 Podfile 里这么引入

```pod
pod 'YOUR_SPEC', :subspecs => ['IDFA', 'IDFB']
```

&#160;

----------

#Appendix

##Related Documentation

[利用 podspec 的 subspec 来实现多个预处理宏的灵活配置](https://0error0warning.com/blog/14797354347923.html)
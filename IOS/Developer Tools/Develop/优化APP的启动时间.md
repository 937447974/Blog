#1 Dyld Steps

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017011001.png)

1. Map all dependent dylibs, recurse 
2. Rebase all images
3. Bind all images
4. ObjC prepare images
5. Run initializers

##1.1 Loading Dylibs

1. Parse list of dependent dylibs 
2. Find requested mach-o file 
3. Open and read start of file 
4. Validate mach-o
5. Register code signature
6. Call **mmap()** for each segment

###1.1.1 Recursive Loading

1. All your app's direct dependents are loaded 
2. Plus any dylib's needed by those dylibs 
3. Rinse and repeat
4. Apps typically load 100 to 400 dylibs!	- Most are OS dylibs	- We’ve optimized loading of OS dylibs

##1.2 Fix-ups

1. Code signing means instructions cannot be altered
2. Modern code-gen is dynamic PIC (Position Independent Code) 
	- Code can run loaded at any address and is never altered	- Instead, all fix ups are in __DATA

###1.2.1 Rebasing and Binding

1. Rebasing: Adjusting pointers to within an image 
2. Binding: Setting pointers to outside image

**Rebasing**

1. Rebasing is adding a "slide" value to each internal pointer 
2. Slide = actual_address - preferred_address
3. Location of rebase locations is encoded in LINKEDIT 
4. Pages-in and COW page
5. Rebasing is done in address order, so kernel starts prefetching

**Binding**

1. All references to something in another dylib are symbolic 
2. Dyld needs to find symbol name
3. More computational than rebasing
4. Rarely page faults

###1.2.2 Notify ObjC Runtime 

1. Most ObjC set up done via rebasing and binding 
2. All ObjC class definitions are registered 
3. Non-fragile ivars offsets updated
4. Categories are inserted into method lists 
5. Selectors are uniqued

##1.3 Initializers

1. C++ generates initializer for statically allocated objects 
2. ObjC +load methods
3. Run "bottom up" so each initializer can call dylibs below it
4. Lastly, Dyld calls main() in executable

##1.4 Pre-main() Summary

Dyld is a helper program
1. Loads all dependent dylibs
2. Fixes up all pointers in DATA pages 
3. Runs all initializers

#2 Improving Launch Times

##2.1 Goals

1. Launch faster than animation	- Duration varies on devices 
	- 400ms is a good target2. Don’t ever take longer than 20 seconds	- App will be killed3. Test on the slowest supported device

##2.2 Launch recap

1. Parse images2. Map images3. Rebase images4. Bind images5. Run image initializers 
6. Call main()
7. Call UIApplicationMain()
8. Call applicationWillFinishLaunching

##2.3 Warm vs. cold launch

1. Warm launch
	- App and data already in memory
	- App is not in kernel buffer cache2. Warm and cold launch times will be different	- Cold launch times are important
	- Measure cold launch by rebooting

##2.4 Measurements

1. Measuring before **main()** is difficult2. Dyld has built in measurements	- **DYLD_PRINT_STATISTICS** environment variable 
		- Available on shipping OSes		- Significantly enhanced in new OSes		- Available in seed 23. Debugger pauses every dylib load	- Dyld subtracts out debugger time 
	- Console times less than wall clock

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017011002.png)

##2.5 Optimizing

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017011003.png)

###2.5.1 Dylib Loading

1. Embedded dylibs are expensive2. Use fewer dylibs	- Merge existing dylibs
	- Use static archives3. Lazy load, but...
	- **dlopen()** can cause issues 
	- Actually more work overall

###2.5.2 Rebase/Binding

1. Reduce __DATA pointers2. Reduce Objective C metadata	- Classes, selectors, and categories3. Reduce C++ virtual4. Use Swift structs5. Examine machine generated code 
	- Use offsets instead of pointers
	- Mark read only

###2.5.3 ObjC Setup

1. Class registration
2. Non-fragile ivars offsets updated 
3. Category registration
4. Selector uniquing

###2.5.4 Initializers

**Explicit**

1. ObjC **+load** methods	- Replace with **+initiailize**2. C/C++ `__attribute__`((constructor))
3. Replace with call site initializers	- dispatch_once() dsfgsdfgsed	- pthread_once() 3452342	- std::once()

**Implicit**

1. C++ statics with non-trivial constructors
	- Replace with call site initializers 
	- Only set simple values (PODs) 
	-Wglobal-constructors
	Rewrite in Swift 2. Do not call dlopen() in initializers 
 3. Do not create threads in initializers

#3 TL;DR
1. Measure launch times with **DYLD_PRINT_STATISTICS** 
2. Reduce launch times by	- Embedding fewer dylibs
	- Consolidating Objective-C classes 
	- Eliminating static initializers3. Use more Swift
4.  dlopen() is discouraged	- Subtle performance and deadlock issues

&#160;

----------

#Appendix

##Related Documentation

[Optimizing App Startup Time](https://developer.apple.com/videos/play/wwdc2016/406/?time=55)

[如何优化 App 的启动时间](http://www.cocoachina.com/ios/20161102/17931.html)


##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-01-10 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974

optimizin app startup time
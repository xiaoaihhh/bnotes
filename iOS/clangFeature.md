[toc]

#### 检查是否使用ARC
```
#if !__has_feature(objc_arc)
#error This file must be compiled with ARC. Use -fobjc-arc flag (or convert project to ARC).
#endif
```


\_\_attribute\_\_((unused))标识参数未被使用

```
- (void)applicationWillTerminate:(NSNotification * __attribute__((unused)))notification
```


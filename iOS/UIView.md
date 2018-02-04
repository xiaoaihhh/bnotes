[TOC]

# [UIView](https://developer.apple.com/library/content/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009503-CH1-SW2)

## responder chain
<img src="https://raw.githubusercontent.com/xiaoaihhh/bnotes/master/images/iOS/ios-responder-chain.png" width="100%" height="100%" align=center>

## [View Drawing Cycle](https://developer.apple.com/library/content/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html#//apple_ref/doc/uid/TP40009503-CH2-SW2)
UIView在需要是才进行绘制，当第一次出现在屏幕上时系统绘制其content并且保留一份snapshot（一张位图），如果view的content一直没有改变，那么view的绘制代码不会再次被调用，snapsshot将被重复使用；直到view的content被改变才会重新绘制并重新捕获snapsshot；
content的改变不会直接导致重绘，需要调用`setNeedsDisplay`或`setNeedsDisplayInRect:`，告诉系统在本次runloop结束后重新绘制；
UIView的`contentMode`决定了view的几何结构如何变化






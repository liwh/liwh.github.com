---
layout: post
title: "IOS手势操作"
date: 2012-01-06 15:28
comments: true
categories: 
---

当我们使用IPhone的时候，时常通过我们的手势来操作，比如说，我们通过两指挤压使图片放大缩小等，还有我们通过手指划过改变直线的弯曲程度等等。
这些在IOS中都利用到了UIGestureRecognizer类。

####如何使用

* 增加一个手势识别方法给UIView。
* 提供响应的"handle"方法处理相应的手势操作。

例如，我们在Controller中增加一个手势识别方法给UIView。

```obj-c
  - (void)setPannableView:(UIView *)pannableView      {          _pannableView = pannableView;          UIPanGestureRecognizer *pangr =              [[UIPanGestureRecognizer alloc] initWithTarget:pannableView action:@selector(pan:)]; //为panableView增加手指划过操作          [pannableView addGestureRecognizer:pangr]; //相当于开启对应的手势操作，如果，不增加这句，前面就算实现了对应的方法，也是不会执行的}
```

每种手势操作都提供对应的方法让我们处理对应的事件。例如，UIPanGestureRecognizer就提供了三种方法，如：

* - (CGPoint)translationInView:(UIView *)aView;* - (CGPoint)velocityInView:(UIView *)aView;* - (void)setTranslation:(CGPoint)translation inView:(UIView *)aView;

另外，我们就是通过对其state的判断来觉得该处理什么的样的操作了.例如，下面的判断就是说，当我们接触移动，或者停止接触的时候，我们都更新view

```obj-c
 if ((gesture.state == UIGestureRecognizerStateChanged) ||
        (gesture.state == UIGestureRecognizerStateEnded)) {
  CGPoint translation = [recognizer translationInView:self];  self.origin = CGPointMake(self.origin.x+translation.x, self.origin.y+translation.y);         [recognizer setTranslation:CGPointZero inView:self];
 }
```

####其它的一些手势操作

* UIPinchGestureRecognizer
* UIRotationGestureRecognizer
* UISwipeGestureRecognizer
* UITapGestureRecognizer



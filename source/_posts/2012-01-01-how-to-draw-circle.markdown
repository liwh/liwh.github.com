---
layout: post
title: "how-to-draw-circle"
date: 2012-01-06 00:07
comments: true
categories:  ios
---

这里介绍，如何实现在UIView中画圆，下面是主要代码：

```obj-c
-(void)drawCircleAtPoint:(CGPoint)p withRadius:(CGFloat)radius inContext:(CGContextRef)context
{
    UIGraphicsPushContext(context);
    CGContextBeginPath(context);
    CGContextAddArc(context, p.x, p.y, radius, 0, 2*M_PI, YES);
    CGContextStrokePath(context);
    UIGraphicsPopContext();
}

- (void)drawRect:(CGRect)rect
{
    // Drawing code
    CGContextRef context = UIGraphicsGetCurrentContext();
    CGPoint midPoint;
    midPoint.x = self.bounds.origin.x + self.bounds.size.width / 2 ;
    midPoint.y = self.bounds.origin.y + self.bounds.size.height / 2 ;
    CGFloat size = self.bounds.size.width / 2; 
    if (self.bounds.size.height < self.bounds.size.width) size = self.bounds.size.height / 2 ;
    
    CGContextSetLineWidth(context, 5.0);
    [[UIColor redColor] setStroke];
    [self drawCircleAtPoint:midPoint withRadius:size inContext:context];
｝
```

显示如下：


![image](http://ww4.sinaimg.cn/mw600/6757a08cjw1dosny8myysj.jpg)

```obj-c
//Begin the path￼￼CGContextBeginPath(context);
//Move around, add lines or arcs to the pathCGContextMoveToPoint(context, 75, 10);CGContextAddLineToPoint(context, 160, 150);CGContextAddLineToPoint(context, 10, 150);//Close the path (connects the last point back to the first) CGContextClosePath(context); // not strictly required
//Actually the above draws nothing (yet)!//You have to set the graphics state and then fill/stroke the above path to see anything. [[UIColor greenColor] setFill]; // object-oriented convenience method (more in a moment) [[UIColor redColor] setStroke];￼￼￼CGContextDrawPath(context,kCGPathFillStroke); //kCGPathFillStrokeisaconstant
```

####参考资料
[画图代码笔记](http://www.cocoachina.com/newbie/basic/2012/0106/3840.html)
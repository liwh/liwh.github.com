---
layout: post
title: "给地图大头针加图片"
date: 2012-01-09 13:26
comments: true
categories: iOS
---

我们在IPhone中使用谷歌地图的时候，我们可以在地图中插入大头针。这些大头针左边显示了图片，右边一般显示>标记，用于显示更加详细的信息。
下面，介绍下，如何在大头针中加图片。


每个Annotations都可以用一个callout用于显示信息， 默认，我们点击的时候，只显示 **title** 和 **subtitle** ,但是，我们可以为它增加
左边和右边的accessory视图。一般，我们左侧会增加图片，右边一般增加一个UIButton（UIButtonTypeDetailDisclosure).下面，介绍如何增加：

*  首先，给地图控制器增加下面方法，用于设置图片

```obj-c MapViewController.m
-(MKAnnotationView *)mapView:(MKMapView *)mapView viewForAnnotation:(id<MKAnnotation>)annotation
{
  //为针上增加左侧图片
    MKAnnotationView *aView = [mapView dequeueReusableAnnotationViewWithIdentifier:@"MapVC"];
    if (!aView) {
        aView = [[MKPinAnnotationView alloc] initWithAnnotation:annotation reuseIdentifier:@"MapVC"];
        aView.canShowCallout = YES;	
        aView.leftCalloutAccessoryView  = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, 30, 30)];
    }
    aView.annotation = annotation;
    [(UIImageView *)aView.leftCalloutAccessoryView setImage:nil];
    return aView;
}


-(void)mapView:(MKMapView *)mapView didSelectAnnotationView:(MKAnnotationView *)aView
{
    UIImage *image = [self.delegate mapViewController:self imageForAnnotation:aView.annotation];
    [(UIImageView *)aView.leftCalloutAccessoryView setImage:image];
}
```
* 设置delegate用于传递所需要显示的图片。

具体代码可参见[样例代码](https://github.com/liwh/PhotoDetail/commit/f90a44a5168a2ca5e2728ab3f84b9667132cac00)




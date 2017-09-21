---
layout: post
title: CAReplicatorLayer生成镜面效果
date: 2017-9-19
categories: blog
tags: [CALayer,知识管理]
description: 讲一讲CAReplicatorLayer
---

##CAReplicatorLayer

CAReplicatorLayer 出现的主要目的为了高效的产生许多类似或者相同的图层。它主要是绘制一个图层，或者该图层的子图层，并且使他们有不同的变换效果。他有很多实用的功能例如渐变的图层环还有镜面效果的实现。在这里主要介绍镜面效果。

##实际的代码

```
#import "ReflectionView.h"
#import

@implementation ReflectionView

+ (Class)layerClass
{
    // 修改view默认的layer
    return [CAReplicatorLayer class];
}

- (void)layoutSubviews {
  // 配置ReplicatorLayer
  CAReplicatorLayer *layer =(CAReplicatorLayer *)self.layer;
  layer.instanceCount = 2;

  // 修改反射的Layer的位置
  CATransform3D transform = CATransform3DIdentity;
  CGFloat verticalOffset = self.bounds.size.height + 2;
  transform = CATransform3DTranslate(transform, 0, verticalOffset, 0);
  transform = CATransform3DScale(transform, 1, -1, 0);
  layer.instanceTransform = transform;

  // 设置被反射的layer的透明度
  layer.instanceAlphaOffset = -0.6;
}

@end
```

### 上述是个简单的实现
从这个思路出发可以做出比较理想的反射效果
上述的实现，你在设置图片的时候可以考虑通过layer.contents设置图片，如果是那样要注意scale，或者在上面进一步处理添加imageview
当然啦，之后会更新更加详细的细节

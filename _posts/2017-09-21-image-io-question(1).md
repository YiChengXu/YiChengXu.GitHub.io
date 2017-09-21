---
layout: post
title: 图像IO优化(1)
date: 2017-09-21
categories: blog
tags: [图像优化,IO]
description: 原文章已经不记得了，现在在根据现在的理解编写的有错误之处请指出

---


* 1、加载还是持有？
* 2、多线程加载


##1、加载还是持有

计算机技术的发展使得CPU处理速度大大提高，但是至今位置IO处理的速度提高远远不及CPU速度的发展

虽然iPhone的闪存速度已经比传统的硬盘快很多了，但是相比RAM还是慢几百倍吧，所以我们来讲讲IO导致的界面卡顿。

总的来说IO读取的速度这么慢，我们没办法解决IO的慢，而是考虑提前加载在一些用户不容易感觉出时间漫长的场景下提前加载图片。当然这样的操作有很多局限性，例如在启动时先加载一些图片，iOS的机制应用20秒无法启动就会停止你的应用啦，当然啦，我相信到15秒的时候用户就要抱怨你的应用太慢了而卸载你的应用。在比如如果我们做的是照片流呢，一个collectionview的照片流，我们不可能预先加载这么多图片，那样需要消耗太多时间和内存了。今天主要还是来讲讲多线程加载技术。


##2、多线程加载

我们经常喜欢在主线程加载图片，倘若图片很小那么最多掉个几帧，绝大多数用户肯定看不出来这一点问问。但是想想如果我们的图片每张都是几百kb的水平，那么这就是个灾难。应用会在滑动的时候明显的感觉不流畅，当然算上图片解压那么更加是个噩梦了。

在这里我们介绍使用GCD在后台线程中读取图片然后在前台线程中显示图片
```
- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView
                    cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    // 注册 cell
    UICollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"Cell"
                                                                           forIndexPath:indexPath];
    // 添加image view
    const NSInteger imageTag = 99;
    UIImageView *imageView = (UIImageView *)[cell viewWithTag:imageTag];
    if (!imageView) {
        imageView = [[UIImageView alloc] initWithFrame: cell.contentView.bounds];
        imageView.tag = imageTag;
        [cell.contentView addSubview:imageView];
    }
    //tag cell with index and clear current image
    cell.tag = indexPath.row;
    imageView.image = nil;
    //switch to background thread
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_LOW, 0), ^{
        // 加载图片
        NSInteger index = indexPath.row;
        NSString *imagePath = self.imagePaths[index];
        UIImage *image = [UIImage imageWithContentsOfFile:imagePath];
        //set image on main thread, but only if index still matches up
        dispatch_async(dispatch_get_main_queue(), ^{
            if (index == cell.tag) {
                imageView.image = image;
            }
        });
    });
    return cell;
}
```
当然这么做可以提高界面的流畅性，但是如果你使用time profiler工具你会发现虽然界面的流畅度变高了，但是还有个很费性能的问题解压
这篇博客是这一系列文章的开始，但是之后我会补上的

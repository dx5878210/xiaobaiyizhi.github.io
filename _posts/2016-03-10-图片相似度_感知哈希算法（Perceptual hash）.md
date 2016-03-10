---
layout:     post
title:      "图片相似度_感知哈希算法（Perceptual hash）"
subtitle:   ""
date:       2016-03-10 00:00:00
author:     "小白一只"
header-img: "img/post-bg-06.jpg"
---



#  感知哈希算法（Perceptual hash）
感知哈希算法也许看起来有点不明所以，其实google的图片搜索就是用类似的算法进行图片搜索。

假如要搜这张图的来源
![enter image description here](https://i.imgur.com/MsEyT07.jpg)


![enter image description here](https://i.imgur.com/N7hP7Fz.png)
通过google图片就能搜类似的图片了
结果如下：
![enter image description here](https://i.imgur.com/e9WMoAC.png)

基本用法介绍完了，接下来就仔细讲讲这个算法的基本实现过程

 1. 缩小尺寸
 2. 简化色差
 3. 计算平均值
 4. 比较像素的灰度
 5. 计算hash值
 6. hash值比较
 
 算法简单来讲就是这6步
缩小尺寸，将图片缩小到8x8的尺寸，总共64个像素。除去图片的细节，保留结构明暗基本信息，去除尺寸比例带来的影响。
简化色彩，将缩小的图片转为64级灰度。
计算平均值，计算64个像素点的灰度平均值
将每个像素的灰度与平均值进行比较。大于或等于平均值，记为1，小于平均值记为0
计算哈希值，将上一步的比较结果组合在一起，构成64位整数，这个数就是这样图片的hash值，也就是这张图的指纹。
hash值比较，也就是比较两张图片的相似度，对比64位中有多少位是不一样，等同于计算“汉明距离”。如果不相同的数据位不超过5说明两张图片很相似，如果超过10说明图片比较大不同，这个阈值可以自己设置。
具体代码实现是参考wote大神写的imghash代码，因为他是用python2写的，所以我自己翻译成python3的代码。

```python

def avhash(self, im):
        im = im.resize((8, 8), Image.ANTIALIAS).convert('L')
        avg = reduce(lambda x, y: x + y, im.getdata()) / 64
        return reduce(lambda x, y_z: x | (y_z[1] << y_z[0]),
               enumerate(map(lambda i: 0 if i < avg else 1, im.getdata())),
               0)

def hamming(self, h1, h2):
    h, d = 0, h1 ^ h2
    while d:
        h += 1
        d &= d-1
    return h

def same_as(self, load_image, percent):
        image1 = Image.open(TEMP_FILE)
        image2 = load_image
        im1_hash=self.avhash(image1)
        im2_hash=self.avhash(image2)
        h=self.hamming(im1_hash, im2_hash)
        if h <= percent:
            print('info:汉明距离：'+str(h))
            return True
        else:
            return False
```


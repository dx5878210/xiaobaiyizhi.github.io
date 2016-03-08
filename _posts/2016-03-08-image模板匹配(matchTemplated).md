---
layout:     post
title:      "image模板匹配(matchTemplated)"
subtitle:   ""
date:       2016-03-08 00:00:00
author:     "小白一只"
header-img: "img/post-bg-06.jpg"
---


#  image模板匹配(Template match)

  根据已知模式到另一幅图中寻找相应模式的处理方法叫做模板匹配。就是说模板就是一张已知的小图像。模板匹配就是在一幅大图像中搜寻目标，通过一定算法寻找模板在图中的位置坐标。

###  例如这张寻找s11的位置，搜索结果如下图所示：
  ![目标图片](https://i.imgur.com/1nPlbNk.png)![例如这张寻找s11的位置](https://i.imgur.com/lTqn2aH.png)

先上源码


```python
from matplotlib import pyplot as plt
import os
import cv2

PATH = os.path.dirname(__file__)
image_path = os.path.join(PATH, 'image')


def template_match(whole_image='search1.png', part_image='s11.png'):
	# 模板匹配函数  
	whole_image_path = os.path.join(image_path, whole_image)
	#whole_image_path 指的是原图片的路径
	image1 = cv2.imread(whole_image_path, 0)

	part_image_path = os.path.join(image_path, part_image)
	#part_image_path 模板图片的路径
	template_image = cv2.imread(path_image_part, 0)
	
	w, h = template_image.shape[::-1]
	#模板图片的长和宽
	res = cv2.matchTemplate(image1, template_image, cv2.TM_CCOEFF)
	#运行matchTemplate算法
	min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)
	#获取res 的最大值最小值及其坐标
	print(max_val)
	print(max_loc)
	if max_val >= 6500000:  # 阈值设置
		top_left = max_loc
	else:
		return

	bottom_right = (top_left[0] + w, top_left[1] + h)
	cv2.rectangle(image1, top_left, bottom_right, 255, 2)

	plt.subplot(121), plt.imshow(res, cmap='gray')
	plt.title('Matching Result'), plt.xticks([]), plt.yticks([])
	plt.subplot(122), plt.imshow(image1, cmap='gray')
	plt.title('Detected Point'), plt.xticks([]), plt.yticks([])
	# plt.suptitle(meth)

	plt.show()

template_match()
```





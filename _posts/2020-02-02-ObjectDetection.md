---
layout:     post
title:      Object Detection Milestones
subtitle:    
date:       2020-02-02
author:     Tintin
header-img: img/tag-bg-o.jpg
catalog: true
tags:
    - CV 
    - Detection
---
<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

本文参考：[Object Detection in 20 Years: A Survey](https://arxiv.org/pdf/1905.05055.pdf).   
以下仅对目标检测领域的发展历史做总结性介绍，不描述算法详细细节。

<img src="https://www.researchgate.net/profile/Zhengxia_Zou/publication/333077580/figure/fig2/AS:758306230501380@1557805702766/A-road-map-of-object-detection-Milestone-detectors-in-this-figure-VJ-Det-10-11-HOG.ppm" width="700">


### 一、 传统算法

|   Year   | Algorithm      |
| :------: |:------: |
|  $$2001 $$|  $$Viola \: Jones\: Detectors$$|
|  $$2005 $$|  $$HOG\: Detector$$|
|  $$2008 $$| $$Deformable\: Part-based\: Model (DPM)$$|

### 二. Two-stage Detector

2001-2012是传统算法的时代，从2012年自AlexNet起深度学习开始风起云涌。
 
|   Year   | Mathod      | Algorithm |  
| :------: |:------: | :------:|
|  $$2014 $$|  $$RCNN $$| $$ Selective\: Search+CNN+SVM $$|
|  $$2014 $$|  $$SPPNet $$| $$ CNN+Selective\: Search+SPP+SVM$$|
|  $$2015 $$|  $$Fast \: RCNN  $$|  $$CNN +Selective\: Search+ ROI + Softmax $$|
|  $$2015 $$|  $$Faster \: RCNN  $$| $$ CNN+RPN+ ROI+BBoxReg+Classify$$ |

![](/img/2020_object_detection/rcnn1.png)

以下列出了每个算法所需要了解的相关算法：  
**RCNN:** selective search, 基于图的图像分割算法Graph-Based Image Segmentation  
**SPPNet:** SPPNet原理  
**Fast RCNN:** ROI Pooling  
**Faster RCNN:** Anchor生成、Region Proposal Network（RPN），NMS非极大值抑制

![](/img/2020_object_detection/twostage.jpg)

two-stage中常见的算法
### 三. One-stage Detector


|   Year   | Method      | 
| :------: |:------: | 
|  $$2015 $$| $$YOLO$$| 
|  $$2016 $$|$$ SSD $$|  
|  $$2017 $$|$$  FPN $$| 
|  $$2017 $$|$$  RetinaNet $$|


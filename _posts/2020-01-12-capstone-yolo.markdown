---
layout: "post"
title:  "Capstone Series: Object Detection and crYOLO"
date:   2020-01-12 19:36:55 -0700
categories: capstone
---

### Object Detection & YOLO

There are many object detection methods developed over the years. An earlier approach was to slide a window accross the entire image and run the detection algorithm on the objects inside the window for all windows. Another method is to use Regional Convolutional Neural Networks (R-CNN) which propose potential regions of interes, generates bounding boxes and trie to identify objects within. Apart from being very slow, since both of these methods focus on regions and not the whole image, they lack contextual information. YOLO is another object detection methods that stands out from its predecessors due to its efficiency and high speed, achieving real-time object detection, a feature that is very valuable for many real-world applications. It also takes the entire image instead of regions, which allows it to capture contextual information. YOLO was first introduced by Redmond et al. in 2015; the original paper can be found [here.](https://arxiv.org/pdf/1506.02640v5.pdf) Over the years, new versions of YOLO have been released with incremental changes. The latest one is YOLOv3 and it uses a much deeper network compared to previous versions, which may yield better performance in some cases.

### How Does It Work?

First the image is divided into grid cells. Each cell predicts bounding boxes with a confidence score which reflects the confidence of the model that the box contains an object. This value is equal to the IoU (explained below) between the predicted box and ground truth. Each bounding box has five predictions: x, y, w, h, and confidence score where x and y are the center coordinates of the box relative to the grid cell, h and w are the height and width of the box relative to the whole image. Each cell also contains a calculated conditional class probability which can be interpreted as “given the object what is the probability that it belongs to a class”. Then this probability value is multiplied by the confidence score to obtain a class-specific confidence score, which encodes both the probability of the object inside the box to belong to a certain class as well as how good the predicted box is fits the object. There are also some auxiliary functionalities, which help improve prediction performance.

<img src="https://d3i71xaburhd42.cloudfront.net/f8e79ac0ea341056ef20f2616628b3e964764cfd/2-Figure2-1.png" class="images" height = "400" width="600"/>
<em><sub>Redmon et al. “You Only Look Once: Unified, Real-Time Object Detection” 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 779-788 </sub></em>


#### Intersection Over Union (IoU)
IoU is a measure that compares the predicted bounding box and the ground truth. It is calculated by dividing the area of intersection between the two boxes by the area of union. It basically shows how much of the ground truth was the predicted box able to cover. A high IoU value indicates good prediction.

<img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2018/12/Screenshot-from-2018-11-16-13-12-02.png" class="images" height = "200" width="200"/>
<em><sub> https://www.analyticsvidhya.com/blog/2018/12/practical-guide-object-detection-yolo-framewor-python/ </sub></em>

#### Non-Max Suppression
ONe issue in object detection is that large objects can be detected by multiple bounding boxes. So which one should the model choose? Non-max suppression is a method that finds the best possible bounding box for each object. First, from all bounding boxes, the one with the highest detection probability is selected. In the image below, that'd be the 0.9 box for the car on the right Then all boxes that have a large IoU with the selected box are eliminated (the 0.6 and 0.7 scoring boxes around it) This stage ensures that boxes that are overlapping with the selected one are removed.

<img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2018/12/Screenshot-from-2018-11-16-13-32-40.png" class="images" height = "300" width="500"/>

Then, keeping the suppression, the next box with the highest probability is selected (0.8 box on car on the left). Then  the same suppression operation is performed, and iterated over all boxes until ultimately there is only one bounding box (the highest probability one) for each object.

<img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2018/12/Screenshot-from-2018-11-17-12-21-31.png" class="images" height = "240" width="500"/>


### Capstone Project & crYOLO

My capstone project is to detect and classify objects within an image, instead of the entire image. What do I mean by that? Take the tree image below. If the task was to identify what this image is, we would first train a CNN on many different tree images, then feed the image below into the model and - given a good model - expect it to correctly identify the image as a tree. The important thing here is that the task is to classify one image, not different objects within the image. If, say, there were cars, trees, people, etc. in one image, and we wanted to classify these multiple objects inside the single image, that would become an object detection task.

<img src="https://images.unsplash.com/photo-1555401328-659c8e900a28?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=3296&q=80" class="images" height = "600" width="400"/>

In our project we have microscope images which include many protein particles as well as areas of junk. So we have to first detect these protein particles to be able to decide whether they are proteins or not, ergo an object detection task. For the project we don't need to reinvent the wheel, we are allowed - in fact even encouraged - to use open-source packages. One such software is an autamated particle picking software called [crYOLO](https://sphire.mpg.de/wiki/doku.php?id=pipeline:window:cryolo). As suggested by the name, it is based on the YOLO object detection method. It does exactly what we are trying to achieve by utilizing YOLO. You might ask, then, why even need this project; just use crYOLO. Well, one reason is that it's trained on a general dataset, not our specific molecule, which leads to false positives. However, for us it's a very solid starting point from which we can get some output and look for ways to improve it or integrate other solutions. For example, we can fine-tune the model with our data and see if it leads to any increase in performance.

<img src="https://sphire.mpg.de/wiki/lib/exe/fetch.php?w=600&tok=3440fe&media=downloads:cryolo_logo.jpg" class="images" height = "150" width="400"/>

In addition, crYOLO isn't the only particle picker out there that we can use. Other projects such as Topaz and DeepPicker are also viable alternatives. But since YOLO is a very effective object detection method, crYOLO is one of the best candidates for the job.


<html>
<style>
.images{display: block;  margin-left: auto; margin-right: auto; padding-bottom: 10px}
</style>
</html>

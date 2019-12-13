---
layout: "post"
title:  "Capstone Series: Challenges"
date:   2019-11-02 14:36:55 -0700
categories: capstone
---

In the previous post I gave some background into my client's work and explained the overall scope of the project. The project is, in essence, an image processing, object detection and classification problem. However, there are some challenges that make it unique and relatively complicated.

### Data Storage
Our data is made of images taken by an electron microscope (micrographs). For a molecule's 3D structure to be built thousands of protein particles need to be picked from hundreds of micrographs. Each micrograph is 4k by 4k in dimension, and takes approximately 400-600 megabytes of storage. For comparison, an average image taken by a phone or camera is usually no more than 10 megabytes. Therefore, for any model that we build or existing tool that we use, our training data will take a tremendous amount of storage. Our client estimates that we will need to train on approximately one million picked particles in order to achieve good results, which will require hundreds of micrographs.

### Computational Requirements
Training on such a large dataset would take too much time on our personal laptops which have CPUs, so instead we have to train our model on GPUs. For deep learning models - especially in computer vision projects such as ours - GPUs are used because they can handle large amounts of data and execute computations in parallel, minimizing the time it takes to train the model.

Our client has given us access to their development machine which has a GPU as well as a lot of storage for the files. One challange is that we can only access the machine remotely by SSHing, and have to write most of the code in Vim which has a steep learning curve. On the bright side, it's a very good opportunity to learn remote access and command line programming.

If the client's machine isn't enough for training the machine learning model, we may have to rent AWS GPU instances. For our data size, we will need the higher end GPUs that AWS offers. We estimate that - with the $650 we have allocated to the capstone project - we can rent up to 150 hours for training on the AWS GPUs.

### Noise
One of the factors that differentiate our project from common image processing and computer vision projects is the large amount of noise in our images. This is caused by the operation principles of electron microscopes. Below is a raw micrograph image taken at our client's lab before any processing. There are, in fact, about 150 protein particles in this micrograph but it's so noisy that it's impossible to see any of them (the large chunks seen at the bottom of the image are ice pieces and contamination).

<html>
<img src="https://github.com/kerimsertturk/kerimsertturk.github.io/blob/master/job019_micro0001.JPG?raw=true" class = "micrograph_noise" height = "400" width = "400"/>
<style>
.micrograph_noise{display: block;  margin-left: auto; margin-right: auto; padding-bottom: 10px}
</style>
</html>

There are existing tools and programs to denoise micrograph images, but they are mostly embedded into software packages. We could try to utilize those packages or write our own Python script. Either way, we need something that can be easily integrated into the rest of our workflow.

### Conclusion
Overall the biggest challenge of this project is that our data is very unique. The images are in astonomical size compared to other common computer vision projects. In addition, the data is stored not in common file formats, but rather special formats produced by cryo-EM software that our client and other cryo-EM labs use. Therefore, before developing a machine learning model, we need to be able to devise algorithms to ingest the data. We are currently working on Python scripts for data ingestion. In the next post I will talk more about data ingestion and some solutions we came up with.

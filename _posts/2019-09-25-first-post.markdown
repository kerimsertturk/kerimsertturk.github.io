---
layout: "post"
title:  "Capstone Series: Introduction"
date:   2019-10-17 19:36:55 -0700
categories: capstone
---

### Bakground & Context

In the final year, all students in the Electrical Engineering program team up in groups of 4-5 to work on an extensive capstone project that an industry client proposes. I am working with Dr. Sriram Subramaniam's lab within UBC's Faculty of Medicine located at the Centre for Brain Health.

The lab excels at Cryogenic Electron Microscopy (cryo-EM), a technique that allows imaging molecules at extremely high resolutions and building detailed 3D constructs which can be used to design better drugs to treat cancer, brain disorders and other infectious diseases such as HIV and influenza.

<html>
<img src="https://electron.med.ubc.ca/wp-content/blogs.dir/3794/files/2018/08/betagalactosidase_cryoem_gallerydl.png?b=3794&w=1170&h=600&zc=1" class = "ubc-electron" align = "centre"/>
</html>

<em>image taken from https://electron.med.ubc.ca/</em>


The area is so important that the 2017 Nobel Prize in Chemisty was awared to three biophysicists for developing cryo-electron microscopy for the high-resolution structure determination. Our client's lab is also regularly mentioned in influential scientific journals such as Nature.

### How It Works

The process starts with an electron microscope. Unlike regular microscopes, which use light to illuminate the sample to be observerd, an electron microscope uses a beam of electrons for illumination. This way they can reveal extremely small objects at unprecedented resolutions. The electron microscope takes many images like the ones below.

<!--
place image "micrographs-raw.jpg"

Wang et al. “DeepPicker: A Deep Learning Approach for Fully Automated Particle Picking in Cryo-EM.” Journal of Structural Biology 195, no. 3 (2016): 325–36.

-->

Each image (micrograph) contains hundreds or thousands of protein particles, which are initially averaged into 2D classes then used to construct the 3D molecule structures. The resolution of the final 3D structure is directly dependent on the amount and quality of protein particles used. However, the micrographs also have large amounts of contamination, chunks of ice and defective proteins, which - if used - can lead to severe degradation in quality. Therefore, the 'good' protein particles must be detected abd separated from the junk. This process, commonly referred to as "protein picking", is the core of our project.

Currently our client's lab as well as many other facilities around the world conduct particle picking in a semi-automated fashion, which requires someone to observe the results and run iterations of their software until good particles are picked and most of the junk is rejected. This manual intervention makes the process prone to human error and takes significant amount of time. Our project is to automate this particle picking process by designing a machine learning pipeline that receives micrographs as input and produces the location of good particles as output. Below is a visual representation of what we are trying to achieve.

<!--
place image "micrographs-raw.jpg"
Wang et al. “DeepPicker: A Deep Learning Approach for Fully Automated Particle Picking in Cryo-EM.” Journal of Structural Biology 195, no. 3 (2016): 325–36.
-->
Fundamentally, this is an object detection and binary classification problem; both are very common in the field of machine learning and aren't too difficult. However, our data has very unique properties which create inherent challenges that complicate the project. In my next post I will talk about the challenges that we have identified so far.

---
layout: "post"
title:  "ML Series: DeepFake Detectors and How to Fool Them"
date:   2020-03-16 19:36:55 -0700
categories: ml
published: true
---


### Introduction
With advancements in machine learning, especially Generative Adversarial Networks, it's possible to generate completely fake images and videos with neural networks. These DeepFakes can be used for constructive purposes, such as increasing the size of the training set for an ML model, but also for malicious reasons, such as spreading misinformation, creating tension among public figures and political manipulation. As a response to such methods, researchers came up with algorithms that can detect them. However, a very recent article (not published yet) has shown that it's quite possible to fool these detectors.

### Generative Adversarial Networks (GANs)
The most advanced methods to generate fake visuals are based on GANs which was first introduced by Ian Goodfellow and associates [1] in 2014. GANs are based on a game theory scenario in which two networks compete against each other, hence the adversary. The **Generator** network produces a new sample. At each iteration, the network is trying to make the samples more fake; the parameters are thus updated to **maximize** the classification error.

<img src="https://3qeqpr26caki16dnhd19sv6by6v-wpengine.netdna-ssl.com/wp-content/uploads/2019/04/Example-of-the-Generative-Adversarial-Network-Model-Architecture.png" class="gan" height = "420" width="310"/>

The adversary, the **Discriminator** model attempts to distinguish between real samples in the training data and the new sample made by the Generator. The Discriminator network is trying to make the right classification, thus it's parameters are updated to **minimize** the classification error. You can see how the two networks are doing opposite operations.

<div class="empty_space1">
</div>

At first, the Discriminator will easily make this distinction and tell the fake samples from the real ones. If the Discriminator classfies correctly, the Generator model will get penalized through the update of its parameters. As the Generator learns to make better fake samples, the Discriminator will start making mistakes by classifying fake ones as real.

<div class="empty_space2">
</div>

<em><sub>GAN Network (taken from https://machinelearningmastery.com/what-are-generative-adversarial-networks-gans/)</sub></em>

Below is an analogy from Goodfellow's paper that really helped me conceptualize the system. [1]

> The generative model can be thought of as analogous to a team of counterfeiters, trying to produce fake currency and use it without detection, while the discriminative model is analogous to the police, trying to detect the counterfeit currency. Competition in this game drives both teams to improve their methods until the counterfeits are indistiguishable from the genuine articles.

### DeepFake Generation

A lot of research has been done in generating visuals of fake people. The early models, such as Faceswap, used an encoder-decoder network in which the former extracted features and latter reconstructed faces [3]. Lets briefly explain the idea behind such models. Imagine there two datasets: A has visuals of one person and B has visuals of another. Encoder-decoders for each are trained on their corresponding datasets to be able to reconstruct the faces. The trick is that the weights for the encoding part are shared while the decoders are kept separately [6]. This way any image containing the face of the person in dataset A can be encoded using the shared encoder and decoded with the decoder of dataset B. The effect of this approach is that the shared part encodes general features like position, illumination and etc. while the dedicated decoder constructs characteristic details.

<!--
<img src="auto-encoder decoder link here" class="styleGAN" height = "300" width="500"/>
-->

Then GANs were introduced to Deepfake generation algorithms. [Faceswap-GAN](https://github.com/shaoanlu/faceswap-GAN)) was an improvement over the original Faceswap model by combining encoder-decoder network with GANs. [3] One of the most successful GAN-based models, called styleGAN, was introduced by NVDIA researchers in 2018. It addresses the problem of regular GANs not having robust mechanisms to allow control over the style of fake images (the hair type, skin color, eye shape, etc.) The paper "A Style-Based Generator Architecture for Generative Adversarial Networks" explains how their generator can adjust the style of the image at each convolution [2]. As you can see below, styleGAN is able to generate very realistic high-resolution portraits. The code is available on their [github repo.](https://github.com/NVlabs/stylegan)

<!--
<img src="styleGAN link here" class="styleGAN" height = "300" width="500"/>
-->

### DeepFake Detection
Deep learning-generated images made by GAN models are particularly challenging to detect [3]. There has been a lot of work in this field. There has even been some physilogical methods. For instance researchers observed that people in Deepfake videos blink significantly less than real ones [5], hence developed algorithms that tracked blinking frequency. Yang et al. [8] used the inconsistency in head pose to detect fake videos. Xuan et al. [4] used an image preprocessing step, to remove low level high frequency clues of GAN images. This increases the pixel level statistical similarity between real images and fake images and makes the classifier learn more intrinsic and meaningful features.

Of course deep CNN models were used to detect deepfakes. The main idea was that since Deepfake generators alter the pixels around the face area, there would be pixel-level inconsistencies between the face and the surrounding context. Deep networks such as ResNet50, VGG16 and ResNet 152 were used to detect such inconsistency [3]. Authors of MesoNet [6] report an average detection rate of 98% for Deepfake videos. There is an entire list of all the successful detection methods so far in the paper written by Nguyen et al.[3]

### Fooling detectors
In essence, Deepfake detectors look at the person's face and pass it into a CNN which classifies the images by detecting visual clues unique to Deepfakes. However, a recent paper (in pre-print) has demonstrated that it is possible to generate adversarial samples that can fool such detectors [7]. Adversarial examples are intentionally designed inputs into a machine learning model that cause the model to makes a mistake [7]. The main idea is to change visuals such that detectors classify them as real while, in fact, they are fake. Their methods consist primarily of transformations such as Gaussian Blur and noise addition.
<!--
<img src="adversarial1.jpg link here" class="styleGAN" height = "300" width="500"/>
-->
The authors have tested their adversarial attacks on MesoNet [6] and Xception [10], two of the best performing deepfake detection methods. They have identified two possible scenarios: the attacker having complete knowledge of the detector model such as network architecture and parameters (white-box), and the attacker not having such information (black-box). In either scenario they applied the methods to both compressed (used by social media websites, content sharing platforms) and uncompressed (raw files owned by the creator) visuals.

They have defined their success rate as the percentage of adversarial samples that were mistakenly classified as real by the detector models. In the white-box scenario they reported an average success rate of 99.85% for fooling XceptionNet and 98.15% for MesoNet when adversarial videos were uncompressed. However, the attack average success rate droped to 58.46% for XceptionNet and 92.72% for MesoNet when compression was used [7]. In the black-box scenario they reported an average success rate of 97.04% for XceptionNet and 86.70% for MesoNet in the uncompressed format. Similar to our observation in the white-box setting, the success rate droped in the compressed format for the attack. Videos of some of their adversarial samples that managed to fool detectors can be found [here.](https://adversarialdeepfakes.github.io/)

<!--
<img src="adversarial2.jpg link here" class="styleGAN" height = "300" width="500"/>
-->

### Conclusion
Deepfakes are becoming more realistic by the day and humans are having difficulty differentiating what is real from what is not. This situation is concerning as these modified visuals can be used for malicious purposes. in such critical circumstances deep learning based detectors become crucial in identfying the fake samples. However a series of image-wise altercations can produce adversaries that can fool even the best detectors, which proves that the detectors are still vulnerable and not entirely reliable. A possible solution can be to improve the detector by feeding adversaries during the training period [7].

#### Bibliography

[1] Goodfellow, I. , Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., Courville, A. and Bengio, Y. (2014): Generative Adversarial Networks arXiv:1406.2661

[2] Karras, T., Laine, S., & Aila, T. (2019). A Style-Based Generator Architecture for Generative Adversarial Networks. (2019) IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR). arXiv:1812.04948

[3] Nguyen, T.T., Nguyen, C.M., Nguyen, D.T., Nguyen, D.T. and Nahavandi, S. (2019) Deep Learning for Deepfakes Creation and Detection. arXiv:1909.11573

[4] Xuan, X., Peng, B., Dong, J., and Wang, W. (2019). On the generalization of GAN image forensics. arXiv preprint arXiv:1902.11153

[5] Li, Y., Chang, M. C., and Lyu, S. (2018). In ictu oculi: Exposing AI created fake videos by detecting eye blinking. IEEE International Workshop on Information Forensics and Security (WIFS), Hong Kong, Hong Kong, pp. 1-7. doi: 10.1109/WIFS.2018.8630787

[6] Afchar, D., Nozick, V., Yamagishi, J., and Echizen, I. (2018) Mesonet: a compact facial video forgery detection network. IEEE International Workshop on Information Forensics and Security (WIFS). arXiv:1809.00888

[7] Neekhara, P., Hussain, S., Jere, M., Koushanfar, F. and McAuley, J. (2020) Adversarial Deepfakes: Evaluating Vulnerability of Deepfake Detectors to Adversarial Examples. arXiv:2002.12749

[8] Yang, X., Li, Y., and Lyu, S. (2019) Exposing deep fakes using inconsistent head poses. IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), pp. 8261–8265. arXiv:1811.00661

[10] Chollet, F. Xception: Deep learning with depthwise separable convolutions.(2017) In Proceedings of the IEEE conference on computer vision and pattern recognition, pp. 1251–1258. arXiv:1610.02357


<html>
<style>
.gan{float: left; padding-bottom: 1px; padding-right: 35px;}
div.empty_space1{display: block; height: 30px;}
div.empty_space2{display: block; height: 45px;}
</style>
</html>

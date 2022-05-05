# Parasitic Egg Detection and classification

## Problem Statement: 
The objective of this project is to automatically detect parasitic eggs and also identify the egg type in compound microscopy images by using the fields of image processing, medical imaging and computer vision.

Input: microscopic images of parasite egg
Output: bounding box and class of the parasite egg

The outcome of this project could be further improved and assist diagnosis in real clinical use, or even automate detection and identification of intestinal parasite eggs, which can be used by non-experts.

## Technical Challenges: Briefly describe what is technically challenges of this problem.


## Related Works: 
Paper: Parasitic Egg Detection and Classification in Low-cost Microscopic Images using Transfer Learning
Authors: Thanaphon Suwannaphong, Sawaphob Chavana, Sahapol Tongsom, Duangdao Palasuwan, Thanarat H. Chalidabhongseb and Nantheera Anantrasirichaid

This paper presents a deep learning technology with a transfer learning approach for an automatic system of para- site egg detection and classification during a faecal examination. The proposed CNN models show a competent ability in learning relevant features of the different parasitic eggs even in low quality images sourced from a USB microscope. Overall, ResNet50 framework can classify the four types of parasitic eggs with high accuracy and outperforms AlexNet, SSD and Faster R-CNN.

## Method and Results: Describe your detailed technical approach and innovations. Describe evaluation results (dataset and metric).

Datasets
- The data are provided in https://icip2022challenge.piclab.ai/dataset/.
- The dataset contains 11 types of parasitic eggs. 
- There are 1,000 and 250 images/class for training and testing 

Methods

Step 1: Data preprocessing
- Greyscale conversion -> decreases channel from 3(RGB) to 1.

Step 2: Data augmentation
We augmented the images before training with the ratio of 2:1 to original images.
There are 11,000 original images and 22,000 augmented images which were augmented by
- Randomly flip vertically and horizontally
- Randomly zoom out
- Randomly shift 50 pixels horizontally and vertically 
- Random brightness contrast
- Random Clahe/Sharpen/GaussNoise/Blur/Emboss

Step 3: Object detection model 
- Baseline model (SSD300 and Faster R-CNN)

Step 4: Setup training model
- Train Validation Test split = 0.6 / 0.2 / 0.2
- SSD
LR = 1e-3 with 5-step factor=0.1
Epoch = 20
- Faster R-CNN
LR = 5e-3 with 1-step factor=0.1
Epoch = 5
Metrics for benchmarking
- mIoU (object detection score): the average of IoU of each class. 
- macro-F1 Score (classification score): 2 × (precision × recall)/(precision + recall)

Step 5: Test on actual challenge 

## Discussion and Future Work: What are the limitations of your work? What are areas for future improvements?
The poor-quality microscopic image with insufficient detail is still a big challenge that prevents the model from correctly discriminating between different types of parasite eggs and sample impurities. Thus, more work is still required to improve this system.

A larger pretrained network may help the model to learn more complex features of the parasite eggs. This may aid the model to extract the relevant features more accurately.

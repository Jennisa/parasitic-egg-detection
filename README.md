# Parasitic Egg Detection and classification

## Problem Statement: 
The objective of this project is to automatically detect parasitic eggs and also identify the egg type in compound microscopy images by using the fields of image processing, medical imaging and computer vision.

Input: microscopic images of parasite egg

Output: bounding box and the type of the parasite egg

The outcome of this project could be further improved and assist diagnosis in real clinical use, or even automate detection and identification of intestinal parasite eggs, which can be used by non-experts.

## Technical Challenges: Briefly describe what is technically challenges of this problem.
- Some eggs have low contrast and lack of details inside the eggs which can lead to difficulty in detection and species classification.
- The images are in different resolutions, different lighting and setting conditions which are difficult to preprocess.

## Related Works: 
Paper: Parasitic Egg Detection and Classification in Low-cost Microscopic Images using Transfer Learning

Authors: Thanaphon Suwannaphong, Sawaphob Chavana, Sahapol Tongsom, Duangdao Palasuwan, Thanarat H. Chalidabhongseb and Nantheera Anantrasirichaid

This paper presents a deep learning technology with a transfer learning approach for an automatic system of parasite egg detection and classification during a faecal examination. The proposed CNN models show a competent ability in learning relevant features of the different parasitic eggs even in low quality images sourced from a USB microscope. Overall, ResNet50 framework can classify the four types of parasitic eggs with high accuracy and outperforms AlexNet, SSD and Faster R-CNN.

## Method and Results:

Datasets

The images
- The train and test data are provided in https://icip2022challenge.piclab.ai/dataset/.
- There are 1,000 and 250 images/class for training and testing.
- The dataset contains 11 types of parasitic eggs. 
![alt text](https://github.com/Jennisa/parasitic-egg-detection/blob/main/images/01.png)
- The images are in different resolutions, different lighting and setting conditions. 
![alt text](https://github.com/Jennisa/parasitic-egg-detection/blob/main/images/02.png)

The labels
- The labels contain bounding box, category id, area and images information which follow COCO annotation. The annotations are stored using JSON with structure below.
![alt text](https://github.com/Jennisa/parasitic-egg-detection/blob/main/images/03.png)

Methods

Step 1: Data preprocessing
- Greyscale conversion -> decreases channel from 3(RGB) to 1.
- Histogram equalization -> allows the image’s areas with lower contrast to gain a higher contrast.

Example image before and after preprocessing
![alt text](https://github.com/Jennisa/parasitic-egg-detection/blob/main/images/04.png)


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

SSD is a very light weight network having one single shot to detect multiple objects within the image, whilst Faster R-CNN requires two shots, one for searching for regions of interest (ROI), and the other for detecting the object in each ROI using CNNs. The backbones of SSD and Faster R-CNN used here are VGG-16 and ResNet50 architectures, respectively.
![alt text](https://github.com/Jennisa/parasitic-egg-detection/blob/main/images/05.png)
![alt text](https://github.com/Jennisa/parasitic-egg-detection/blob/main/images/06.png)

Step 4: Setup training model
- Train Validation Test split = 0.6 / 0.2 / 0.2
- SSD: LR = 1e-3 with 5-step factor=0.1, Epoch = 20
- Faster R-CNN: LR = 5e-3 with 1-step factor=0.1, Epoch = 5

Metrics for benchmarking
- mIoU (object detection score): the average of IoU of each class. 
- macro-F1 Score (classification score): 2 × (precision × recall)/(precision + recall)

Step 5: Test on actual challenge 

Result
- For each of our object detection model, we submitted the result of four versions including 
1. original images + model
2. preprocessed images + model
3. augmented images + model
4. preprocessed + augmented images + model

The scores are shown in the table below

Model | mIOU | mF1Score |
--- | --- | --- | 
SSD | 0.5983 | 0.7769 |
SSD + preprocess | 0.5548 | 0.7769 |
SSD + augmentation | 0.6424 | 0.8129 |
SSD + preprocess + augmentation | 0.6256 | 0.7997 |
Faster R-CNN | 0.6749 | 0.8168 |
Faster R-CNN + preprocess | 0.6583 | 0.7950 |
Faster R-CNN + augmentation | **0.7127** | **0.8552** |
Faster R-CNN + preprocess + augmentation | 0.6936 | 0.8246 |

- The result shows that augmented images with Faster R-CNN has the best mIOU and macro-F1 score.
- Faster R-CNN gives better mIOU and macro-F1 score than SSD.

## Discussion and Future Work: What are the limitations of your work? What are areas for future improvements?

The poor-quality microscopic image with insufficient detail is still a big challenge that prevents the model from correctly discriminating between different types of parasite eggs and sample impurities. A larger pretrained network may help the model to learn more complex features of the parasite eggs e.g. Resnet50, AlexNet.

Future work
- Deploy the system for real time usage.
- Improve the precision and correctness of parasitie egg detection and discrimination between types of parasite eggs.

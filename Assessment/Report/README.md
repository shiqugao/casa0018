# Fruit classification on mobile phone

Shiqu Gao，https://github.com/shiqugao/casa0018 ，https://studio.edgeimpulse.com/studio/196561

## Introduction

Current research shows that in many developed countries there is often a large output of fruit, but at the same time they use manual methods to sort the fruit. Sorting fruit using traditional human methods is not only a huge waste of human resources, but it is also difficult to maintain a consistent output compared to machines. （Peng，2018）Therefore, a model that can be used to identify different types of fruit based on deep learning is invented and deployed on different devices. This can be applied not only in the early stages of fruit picking, but also in the later stages of fruit sorting lines to upgrade the whole chain, improve the accuracy of the analysis of different fruits and achieve intelligent agriculture. Before starting this project, I was very much inspired by the Ukwuoma project (Ukwuoma, 2022), where a number of different deep learning models were used to identify and classify fruit, and I was very much inspired by the process of image recognition and the causes of errors. How these functions were implemented will be described in the following report.

## Research Question

Use Deep Learning to identify different fruits types

## Application Overview

The images are input from the terminal device, e.g. using the camera of a mobile phone, to obtain the images and output the results for different fruit types. After the camera has acquired the image, the processing block in edge impulse pre-processes the image into a format that can be recognised and processed by the machine learning model, and ensures that the image is of the same resolution. The pre-processed image is then processed in the image transfer learning block, and after setting parameters such as colour depth, training cycle and learning rate, different models can be selected for training and results can be produced(Edge Impulse Documentation(2022a),2023). Finally, the model can be placed on a mobile phone to classify fruits into different categories and output the names of the different categories when a live image is acquired.


## Data

The data used to train the model was sourced from kaggle and is a dataset on fruit. It is roughly divided into two parts, fruits and vegetables. Within each of these two broad categories are a number of sub-categories with specific names of different fruits such as apples, bananas and other common fruits. The next example is the apple folder, which contains three folders, namely train, test and validation, each of which contains various images of the category, with train containing 100 images and the remaining two folders containing 10 images each. The data in these folders are very suitable for training, so there is little cleaning of the data. However, in some of the fruit datasets with multiple meanings, there are still some fruit that do not meet the requirements, for example, there are some images of Apple logos mixed in the apple dataset, and these types of images are removed to avoid affecting the accuracy of the model training.

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/1.png">

After importing the data again, the data displayed in the explorer is shown below. You can basically see that fruits of the same species are divided into similar slices, indicating that the selected data are all basically accurate representations of the current species of fruit.

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/2.png">

## Model
First, MobileNet V1 96*96 0.25 was used as the model for training (Edge Impulse Documentation(2022a),2023). Then, based on this model, some parameters were changed to find out the effect of different parameters on the recognition accuracy and stability of the model. The results are shown in the table below.

<img width="800" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/3.png">

So based on this table, it can be concluded that increasing both image width and heights will make the model more stable, but will result in the model becoming less capable of recognising images outside of the training training set. Changing the colour depth from RGB to greyscale not only makes the model more unstable, but also significantly reduces the accuracy of object recognition. When increasing the number of training cycles, the model becomes more stable and more accurate. Also, when the learning rate is increased, the ability of the model to recognise images outside the training set is increased and the model becomes more stable. In general, the model has been able to identify different types of fruit, and the next step is to use the EON tuner to find the most accurate model and to explore the effect of different parameters on the model.

## Experiments
In the further training process, I chose to use the EON tuner in order to come up with the best trained model. When running the EON tuner, I could see that the model with the highest accuracy was MobileNet V2 with 89% accuracy.

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/4.png">

The next step is to verify whether the above parameters in the model affect the accuracy and stability of the model, and the results of modifying the different parameters are shown below.

<img width="800" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/5.png">

After modifying the parameters the following conclusions can be drawn. Firstly, the conclusions drawn in V1 also apply to V2. Secondly, the V2 model is considerably better than the v1 model in terms of accuracy and stability, but at the same time brings with it the problem of greater energy consumption and the possible need for better equipment to support the operation of the model. The results of the EON Tuner run were evaluated and the results are shown below.

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/6.png">

It can be seen that the accuracy is high when identifying fruits such as apples, carrots and apples. However, the accuracy is lower when it comes to identifying bananas. I speculate that this is because when importing the training set, there are two types of banana images in the banana training set, and when importing, you can see that the bananas are divided into two parts, as shown below.

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/7.png">

You can clearly see that there are parts of the banana that are distant from the main part. When I studied this part of the image, I found that in the main part of the banana, there is usually only a single banana in that inside the banana image, but in the image away from the main part, there is often a bunch of bananas in it, and although both images are bananas, there is a difference in the model training, which may affect the accuracy of the model training.

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/8.png">

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/9.png">

## Results and Observations
After setting up the project on the phone, it was found that it could be run in such a way that it could basically identify the type of fruit that needed to be detected. But at the same time there are some shortcomings, firstly the recognition is slow and it takes a little time to arrive at the correct type. Secondly, the anti-interference capability is weak, and in the case of complex backgrounds, it is often not possible to reach the correct conclusion, even if the same fruit appears several times in the same scene, which also affects the accuracy of the recognition. This is probably due to the lack of clarity of the current camera and the lack of sufficient blocks for better training of the model. Also, because the model requires around 700.7K RAM and 982.4K ROM with default settings, it is a relatively memory-hungry model, so it may need to be set up on a mobile phone to run properly. In the future, if more time is available, it may be possible to set up the device on a Raspberry Pi or microcontroller to make it easier to set up for real-life production activities. In addition, the current model can only be used to recognise certain types of fruit, and in the future it may be possible to consider introducing more types of fruit during training to increase the variety of fruit that can be recognised. Consideration will also be given to reducing the size of the model and testing the performance of the model on a microcontroller to ensure that it runs on a microcontroller with the same performance.

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/11.png">

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/12.png">

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/13.png">

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/14.png">

<img width="400" alt="image" src="https://github.com/shiqugao/casa0018/blob/main/Assessment/Image/15.png">



## Bibliography
Edge Impulse Documentation (2022a) Impulse design, Edge Impulse. Available at: https://docs.edgeimpulse.com/docs/edge-impulse-studio/impulse-design (Accessed: March 26, 2023).

Edge Impulse Documentation (2022b) Transfer learning (Images), Edge Impulse. Available at: https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/transfer-learning-images (Accessed: March 26, 2023).

Peng, H., Shao, Y., Chen, K., Deng, Y., & Xue, C. (2018). Research on multi-class fruits recognition based on machine vision and SVM. IFAC-PapersOnLine, 51(17), 817-821.

Ukwuoma, C. C., Zhiguang, Q., Bin Heyat, M. B., Ali, L., Almaspoor, Z., & Monday, H. N. (2022). Recent advancements in fruit detection and classification using deep learning techniques. Mathematical Problems in Engineering, 2022, 1-29.


## Declaration of Authorship

I, Shiqu Gao, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.


Shiqu Gao

27/04/2023

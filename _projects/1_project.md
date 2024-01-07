---
layout: page
title: Helmet Detection
description: Object detection from videos using yolo model
img: assets/img/projects/HelmetDetection_main_image.png
importance: 1
category: AI
---


## YOLO v5 Model

In this approach, all the images and labels were trained on the yolov5m model.

<br>

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/confusion_matrix.png" title="Confusion Matrix" class="img-fluid rounded z-depth-1" %}
</p>

As it can be seen from the correlation matrix that the model was unable to predict the P1Helmet correctly at all of the predictions for that class were wrong and same goes for P2Helmet and P2NoHelmet as well.

<p align="center">
    {% include figure.html path="assets/img/projects/HelmetDetection/Images/labels.jpg" title="labels" class="img-fluid rounded z-depth-1" %}
</p>

As it can be seen from the above figure the there is high class mismatch as there are small number of bounding box which are being identified as the P1Helmet , P2Helmet and P2NoHelmet this might be causing the model to perform poorly on the P1Helmet classification.

<p align="center">
{% include figure.html path="assets/img/projects/HelmetDetection/Images/F1_curve.png" title="F1 curve" class="img-fluid rounded z-depth-1" %}
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/PR_curve.png" title="PR curve" class="img-fluid rounded z-depth-1" %}
</p>

The major for such a low prediction for P2 might be that having 3 people on the motorbike is a rare event and further to classify that it’s the p2 model need to first identify that there is p1 present in the motorbike so such sort of sequential learning might be causing the model to predict poorly on this particular class.

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/Train_test_yolov5.png" title="train test result" class="img-fluid rounded z-depth-1" %}
</p>

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/P_curve.png" title="P Curve" class="img-fluid rounded z-depth-1" %}
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/R_curve.png" title="R Curve" class="img-fluid rounded z-depth-1" %}
</p>


### Results on Test Dataset

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/yolov5Prediction.png" title="yolov5 Predictions" class="img-fluid rounded z-depth-1" %}
</p>

### Exploratory Data Analysis And Preprocessing

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/EDA.png" title="EDA" class="img-fluid rounded z-depth-1" %}
</p>

As can be seen from the above curves the best height and width to be selected for resizing the image would be around 100. And as can be seen on average most of the images have 2-4 objects.

<br>

## Using Neural Network as a Classifier

In this approach firstly a yolo network was used to identify the different object present in the image which is motorbike and person. Then the results of the model which are the cropped images of the bounding boxes detected for these object is passed to an efficient net architecture to classify if that cropped image is one of the seven class described in the competition. One extra class was added so as to make the model classify the cropped image as other if none of those seven classes meet. <br>
But during the training this efficient net architecture was trained with the cropped images obtained from the ground truth.

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/TrainingNeuralNet.png" title="Training Neural Net" class="img-fluid rounded z-depth-1" %}
</p>

As the model uses pretrained weights but the parameters are not frozen during training thus it requires few epochs to get used to the new dataset.

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/Confusion_matrix_neuralNet.png" title="Confusion Matrix of Neural Net" class="img-fluid rounded z-depth-1" %}
 </p>

As it can be seen from this confusion matrix that although the model is performing really well still as there is not dataset for other category it will be difficult for the model to classify the cropped image into this one

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/NeuralNetRCurve.png" title="Neural Net PR curve" class="img-fluid rounded z-depth-1" %}
</p>

### Results on Test Dataset

<p align="center">
 {% include figure.html path="assets/img/projects/HelmetDetection/Images/NeuralNetPrediction.png" title="Neural Net Predictions" class="img-fluid rounded z-depth-1" %}
</p>

Note – in this approach yolo model was not trained at all and as yolo model was not trained at all and the classifier does not have any of the cropped images for the other category it classifying each and every person bounding box predicted by the yolo model as either of those 7 classes
Now the above issue could have been solved by generating the custom dataset by passing it thorough yolo and checking IoU with the ground truth values and if the IoU is equal to 0 then that cropped image should have been marked as other and then the model should have been trained on this dataset. But because of the time constrain I was not able to do that.

## Conclusion

The results can be improved by using different data augmentation techniques and creating custom dataset as mentioned above with few modification. Also different model can be tried with to check which model best results in better accuracy <br>
Overall It seems that with just the yolo model the model is not able to detect all the object present in the image whereas with the second approach of using yolo and other classifier it seems as it’s detecting too much so may be some ensemble method might also help improve the accuracy.
<!--
<p align="center">
  <img width="1700" src="Assets/Head.png" >
</p>
-->

<p align="center">
  <img width="1000" src="Assets/head_2.png" >
  
</p>

# Artificial intelligence for lung disease detection using chest CT scan images

Artificial intelligence has the potential to help in covid detection using CT scan images from patient's chests. In this project, we use apply two convolutional neural networks for classification. 
Two different data sets were gathered from Kaggle and Github for training two separate Convolutional Nural Networks (CNN).
First, a two-class classification model was trained on balanced data (covid vs normal) to detect and differentiate the healthy cases from covid cases.
Second, a neural network was trained to separate four classes including pneumocystis, covid, streptococcus, and normal. I used two common approaches in image processing for dealing with imbalanced data including class weight adjustment and over-sampling. The oversampling was done using data augmentation which uses different transformers for this purpose. The models were run on the local machine with a few epochs and later uploaded into the google-colab to benefit from GPU and high speed. 


### Description
The three lungs discuss definition is as follows.

* Pneumocystis pneumonia (PCP) is a serious infection that causes inflammation and fluid buildup in your lungs. It's brought on by a fungus called Pneumocystis jirovecii that spreads through the air. This fungus is very common. Most people's immune systems have fought it off by the time they're 3 or 4 years old.

<p align="center">
  <img src="Assets/Pneu.PNG" >
</p>

* COVID-19 is caused by a coronavirus called SARS-CoV-2. Older adults and people who have severe underlying medical conditions like heart or lung disease or diabetes seem to be at higher risk for developing more serious complications from COVID-19 illness.

<p align="center">
  <img  width="300" src="Assets/covid.png" >
</p>

* Streptococcus is a genus of gram-positive coccus or spherical bacteria that belongs to the family Streptococcaceae, within the order Lactobacillales, in the phylum Firmicutes. Cell division in streptococci occurs along a single axis, so as they grow, they tend to form pairs or chains that may appear bent or twisted.

**reference: Wikipedia, Google**

### Table of Contents
The project directory tree structure is provided below.
```
├───Assets
├───Codes
├───Dataset
├───Dataset_4_classe
├───Dataset_4_classes_balanced
├───Dataset_4_classe_second_approach
├───Extract and filter images from data set
│   └───Dataset
│       ├───Covid
│       ├───NORMAL
│       ├───Pneumocystis
│       └───Streptococcus
└───Figures
```


### Instruction

**Gathering data:** 
The X-Ray images were gathered from [Kaggle](https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia) and [Github](https://github.com/ieee8023/covid-chestxray-datasetrepository).
The data then was divided into two Train and Validation folders.
Two data sets were prepared which are **Dataset** and **Dataset_4_classe**.
* The **Dataset** two categories are seen below.
<p align="left">
  <img src="Assets/plot_01_assets_1.png" >
</p>

* The **Dataset_4_classe** four categories are seen below.
<p align="left">
  <img src="Assets/plot_01_assets_1_4classes.png" >
</p>

In this project, we use the deep neural network to differ the normal patients from three different sicknesses including Pneumocystis, COVID-19, and Streptococcus.
As it is seen in the project directory, the multi-class classification data set (Dataset_4_classe) included four different sub-folders compare to two bi-class classification data set (Data set).






**Assembled Deep Net Model Layers:** 
Any time that you have several images (multiclass classification) use two to three convolution layers. Also, a use softmax as activation for the last layer as I did (my recommendation but you may test other types). Note that the categorical_crossentropy is almost default for multiclass classifiers. Remember that we always use convolution layers for images. the reason is if we use dense layers we will lose positional information in images.

**Prepare Images:**
Using ImageDataGenerator does the normalization (Resale function does normalization). Then augment the data set for both train and val.
Note that for the validation section, I just apply the normalization part. Next, use flow to apply the data augmentation.
* Below dataset images after applying augmentation and balancing are seen.

<p align="left">
  <img  width="2000" src="Assets/plot_01_assets_2_4classes_balanced.png" >
</p>



For bi-class classification, the number of images is equal so there is no need for balancing the dataset. However, for multi-class classification, I have imbalanced data and I need to consider it to prevent bias. One way to deal with imbalanced data applies class-weight using following StackOverflow three lines code and pass it to the fit function.

```python
counter = Counter(train_generator.classes)                          
max_val = float(max(counter.values()))       
class_weights = {class_id : max_val/num_images for class_id, num_images in counter.items()}
```
**CNN model Metrics and Conclusion**

The call back function automatically save the best models taking the best val_acc into account. User can call different saved models and use for analysis.
* The CNN model different metrics are seen for biclass classification project below.
<p align="left">
  <img width="700" src="Figures/plot_01_1.png" >
</p>

* The CNN model different loss and accuracy metrics are seen for biclass classification project below.
<p align="left">
  <img width="500" src="Figures/FixedclassRes.png" >
</p>




**Visualization**

- A visualization using visualkeras library for 4 class classification network is seen below.
<p align="center">
  <img width="500" src="Figures/CNN_4class.png" >
</p>

- The second Convolutional Nural Networks layers were Visualized below.

<p align="center">
  <img src="Assets/Conv_layer_1_viz.png" >
</p>

You can see that some filters check the edge of images while as we get far from images filters see the roundness of the image.



**Final**

* Both models including two class classification and four class classification were developed in python using tensorflow library.
* For the two class classification the balance data set was used.
* In four class classification, the total number of noraml and covid were equal while the number of two other categories were under-balance. The number of other two classes were balanced taking the number of normal and covid casses into account. The generator was applied for generating new images. Check out the Over_Sampling_Images_second_approach file.
* In four class classification project weighted objective function was used to deal with imbalance data set.
* The four class classification codes was uploaded into the google colab to be ran using GPU. The learning curve of model is seen below.
* Overfitting is observed due to lack of data for two classes in four class classification project.



<p align="center">
  <img width="600" src="Assets/LearningCurvefourClassClassification.png" >
</p>

* The two-class classification model accuracy achieved 96%.
* The four-class classification model accuracy achieved around 75%.


**Suggestion**

* Balancing data using a generator is one option for dealing with imbalanced data but it is not always the best.
* The weighted objective function can be used as a second option.
* Generally, using either above options results in losing lots of feathered which results in low model accuracy.
* The results for the two-class classification project were promising and reliable.
* Different learning rate should be applied to see it will affect the output.








                            
# Cancer Cell Image Recognition With Neural Networks Capstone Project  # 

 ## Problem Statement: ##

 
  Our skin is our body's largest organ and skin cancer is the most common form of cancer. If treated early, skin cancer is highly curable. At this time, the dominant method for skin cancer diagnosis is histopathological, physically removing material and identification in a laboratory. Recognition of the pain involved may dissuade patients from seeking early treatment. Medical professionals have turned to the data science community for a tool for diagnosing skin cancer through image processing. A model that could reliably detect cancer will save lives. The problem is to develop a model that can detect cancer cells by interpreting the image data. Stakeholders in this endeavor are medical professionals and patients with skin lesions.

  This project seeks to identify images of pigmented skin cells collected by medical staff. The goal of medical researchers is to create a tool that can help to determine which images are of benign vs malignant cells. The datasets are available online at Kaggle and the origin of the set is Medical University of Vienna. The data includes diagnostic information classifying the images into seven categories. Supporting documentation states the images are representatively distributed. The data is available in a wide range of resolutions from 8 x 8 pixels to 450 x 600 pixels.
  Model performance will be measured by accuracy relative to a baseline and loss scores for a training set and a validation set of images. 

 ## The Datasets ##
  The data consists of 10015 images of pigmented skin lesions, compiled by medical researchers to create a dataset for data scientists to use for image recognition. There is also a coordinated set of metadata included. The metadata includes information about the diagnosis for each image, the method used to determine each diagnosis, the patient's age and sex, as well as the localization of each image. Each image has its own ID and each lesion has its own ID. Some lesions have more than one image, there are over 7400 lesion ids.

## Source : ##

  @data{DVN/DBW86T_2018,
  author = {Tschandl, Philipp},
  publisher = {Harvard Dataverse},
  title = "{The HAM10000 dataset, a large collection of multi-source dermatoscopic images of common pigmented skin lesions}",
  UNF = {UNF:6:IQTf5Cb+3EzwZ95U5r0hnQ==},
  year = {2018},
  version = {DRAFT VERSION},
  doi = {10.7910/DVN/DBW86T},
  url = {https://doi.org/10.7910/DVN/DBW86T}
 }

## Project Organization ##
   This project is separated into several folders. There is a folder to house the various data files, the low and medium resolution files are stored as CSV files, while the full size images are provided via two zip files. The two zip files were downloaded opened and joined together. Once joined together this folder is quite large and in fact at full size proved too difficult for my equipment to handle. There is a folder for Exploratory Data Analysis, and there are notebooks for each resolution model and for color or greyscale. 

  ## Exploratory Data Analysis ##
   The metadata is examined in the EDA folder. There are many interesting findings. I used data science techniques to look at the various columns and tried to understand any relationships among those columns. There is some missing data in the age column, also sex and localization have some entries labeled unknown. As this project will be modeling with only the image data, I decided not to eliminate the rows with missing or unknown entries. This allowed me to keep the whole dataset intact. Further investigation of the columns showed there to be little correlation among the columns in the metadata.
   
  ## Diagnosis Column ##  
   My objective is to identify cancer cells. There are two type of cancer cells benign( localized and not spreading ) and malignant ( not localized and 
   can spread throughout the body). The dataset includes seven different types of possible diagnosis. I found that I could group the 
   diagnosis into benign and malignant groups. These are the diagnosis and their respective codes:

   1. mel: Melanoma-the most dangerous form of skin cancer
   2. bkl: Benign keratosis-like lesions
   3. bcc: Basal cell carcinoma-rarely metastasizes but does spread
   4. akiec: Actinic keratoses-scaly patch due to years of sun exposure
   5. vasc: Vascular lesions-birthmarks can be flat or raised 
   6. df: Dermatofibroma-superficial benign fibrous histioocytoma
   7. nv: Melanocytic nevi-birthmarks, moles, resembles melanoma 

  I grouped Melanoma and Basal Cell Carcinoma into malignant and Benign Keratosis-like Lesions, Actinic Keratosis, Vascular Lesions, Dermatofibroma, and Melanocytic nevi make up  benign. The documentation for this dataset describes all of the diagnosis and also states that the composition of the diagnosis in the dataset is roughly representative of the distribution in the patient population. When I investigated this distribution, I found the diagnoses to be very unevenly distributed, leading to concerns about unbalanced classes- some of the categories have little more than 100 observations while the largest 'nv' has more than 
  6600. I decided to create balance classes by randomly sampling from the benign,the same number of observations that make up the malignant group-1627. In this way, both groups are equally represented. Predicting correctly may prove to be quite difficult as 'nv' and 'mel' are said to similar in appearance.
  ### Diagnosis Method Column ###
  By looking at the diagnosis method column, one can see how medical professionals arrive at the diagnosis.
  These are the methods of diagnosis included:
  1. histo:histopathological examination of physically removed cells
  2. follow_up:no change in three or more visits or after 1.5 years
  3. consensus:same conclusion by two unrelated medical professionals
  4. confocal: close visual examination using near microscopy

   The majority of the diagnosis are made through histopathological process, meaning material is removed and examined in a laboratory. This painful procedure is described as the ground truth in the documentation. The second largest category is follow up , where the patient is asked to return one or more times over a certain time period, where the lesion appearance and size are tracked. Diagnosis via neural network would be another tool for medical staff if a model could be developed.

  ## Image files ##
  The low and medium resolution image files are available as CSV’s.
  The full size image files (450 by 600) are available as two zip files which were imported, opened and combined
  The full size images are loaded one at a time and each image is converted to an array using a Keras package. it is also possible to resize the images at this point, to make them easier to work with.
  ## The Modeling Notebooks ##
  The notebooks are organized by resolution and whether greyscale or color. The resolutions are 8 by 8 pixels, 28 by 28 pixels, 75 by 100 pixels and 150 by 200 pixels. There are also two notebooks with full size image files 450 x 600 but I have been unable to run these models on my equipment. Once the images are loaded, resized and adjusted for use, i can assign variables for the target (diagnosis) and the features(image data). Since I have changed the diagnosis to be malignant/benign I can use a binary neural network model. There are many ways to vary Neural Networks but I am curious to investigate the effect resolution makes on model performance so I will try to use similar architecture in most of the models to allow for comparison. In the future, I will vary more options. Higher resolutions means more information for a model to work with and full size images are three times the size of the largest files the models have seen thus far.
  ### Feed Forward Neural Network ###
  Each notebook's first model is a Feed Forward Neural Network. This is the most basic network consisting of Flattening, input Dense layer, hidden Dense layer and output Dense layer. After the model is fit, the model goes througha number of prespecified trials called epochs. This is machine learning in action. A training accuracy and loss score and a test accuracy and loss score are generated for each epoch the model is run. I chose to run all but the largest files for 20 epochs. The results from the Feed Forward Network models  were slightly higher than the baseline of 50%(since I am using Balanced Classes).
  ### Convolutional Neural Network Model 1 ###
  Each notebook's second model is a Convolutional Neural Network consisting of a convolutional layer, a pooling layer, second convolutional layer, second pooling layer, followed by flattening and two or three densely connected layers.The convolutional/pooling steps condense information. The lowest resolution files did not have enough information to allow for the second convolutional /pooling layer so these were omitted. Sometimes the results from the first convolutional models were good, besting the more complex models especially in the case of the medium resolution files. I believe medium resolution files also can run out of information for the more complex models and performance begins to suffer. Normally the first convolutional models charts will tell a different story. These first models have not addressed the issue of overfitting and this can be seen on the charts as wide gaps between the lines for training and testing set performance. 
  ### Convolutional Neural Network Model 2 ###
  Each notebook's third model introduces dropouts in between densely connected layers to prune back the network and to help the model performance on train and test sets to move in tandem. I have had varying degrees of success, experimenting with many different levels of dropouts and densely connected layers. Overly connected models can have wild chart patterns, leading to the impression that nothing is being learned.
  ### Convolutional Neural Network Model 3 ###
  At this point, I began to experiment with different parameters. A stride is the size of a step the filter takes in the convolutional layer. The default value is one but it is adjustable. I found altering the stride for my models not to be helpful. I also considered adding padding to each image. Padding is essentially a border for each image to allow the edges of the image to get more representation. This did not effect the model results, probably because the actual lesions were normally in the center of the image file, maybe the technique would work better where the area of interest is the entire image. A third modification I attempted involved use of a data generator for data augmentation.
  ### Convolutional Neural Network Model 4 ###
  Data augmentation is the concept of altering the orientation of an image and representing the reoriented image as a new sample for the model-by rotating the image the model will view it as new information, giving the model extra opportunities to learn without having to find new data. The Keras Library includes an Image Data Generator, which allows images to be rotated, shifted, zoomed and flipped. The option to whiten was not chosen as these skin cell images are pigmented to increase contrast while whitening would decrease contrast. I used the generator to rotate, zoom and flip the images. Data augmentation did not increase the model performance, in fact at times, the generator seemed to overwhelm the model and the result would be a flat line at 50% indicating the model had resorted to guessing. I have also begun to experiment with changing the learning rate to attempt to smooth some of the loss curves of the models using higher resolution images. 

 ## Executive Summary ##

  Through experimentation I have found less complicated models to perform better than complex ones especially with lower resolution image files. Larger image files do benefit from an added degree of complexity. I have found the lowest resolution files do have the lower scores consistently, but not drastically lower. Considering the proportionate increase in information, the performance increase is actually just incremental. Best greyscale 8 by 8 accuracy 0.619 and color 8 by 8 accuracy .697 vs Best greyscale 150 by 200 accuracy .707 and color 150 by 200 accuracy .709. These scores are accuracy scores as calculated by the model but in practice for a model of medical data we would also be concerned about other measures such as False Negatives and False Positives. At another time, I will quantify these and other performance measures. This is just the beginning, there are a great many parameters to adjust, learning rates to alter and optimizers to swap out. 
  
  
 









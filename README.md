# Eliminating surgical pain

Surgery is not easy for everyone. It brings discomfort and often involves significant post-surgical pain.

Currently, patient pain is frequently managed through the use of **narcotics that bring a bevy of unwanted side effects**.

Our solution aims to improve pain management through the use of indwelling catheters that block or mitigate pain at the source before the surgery begins.

Pain management catheters reduce dependence on narcotics and speed up patient recovery.

In our solution, we identify and segment a collection of nerves called the **Brachial Plexus (BP)** in ultrasound images. 

Our model  build a model can identify **nerve structures in a dataset of ultrasound images of the neck**. Doing so would improve catheter placement and contribute to a painless surgery.


**Dataset - A large training set of images where the nerve has been manually attributed by doctors. These doctors were trained by experts and instructed to attribute images where they felt confident about the existence of the BP nerve.**

Important points about dataset:


*  **The dataset contains images where the BP is not present. Our algorithm predicts no pixel values in these cases.**

*  **As with all human-labeled data, we do find noise, artifacts, and potential mistakes in the ground truth.**

*  **Identical images do exist in the dataset. Revealed in EDA.**

*  **Dataset uses run-length encoding (RLE) on the pixel values.**

*  **The dataset consists of 5635 training images and their masks, and 5508 testing images**


About the data-

**Train.zip** file contains the training set images, named according to subject_imageNum.tif. Every image with the same subject number comes from the same person. This folder also includes binary mask images showing the BP segmentations.

**Test.zip** contains the test set images, named according to imageNum.tif. We predict the BP segmentation for these images. There is no overlap between the subjects in the training and test sets.


**Dataset Credits : Qure.ai - Deep learning products that fit your radiology workflow**


**Approaches we've tried so far**- 

**Exploratory Data Analysis** 

It is a good practice to understand the data first and try to gather as many insights from it. EDA is all about making sense of data in hand before getting into it.

All our training data is in one column, titled pixels.

train_masks.csv gives the training image masks in run-length encoded format. So, we need to segment this data into something we can plot!

**Steps**

1.  Separate run-lengths and pixel locations into seperate lists
2.  Get number of data points in each image
3.  Get all absolute target values
4.  Remove NaNs

We have seperated the run-length from pixel location, removed NaN data (where there is no data) and also got the number of data points in each image.Now we can plot some graphs!

5. Image analysis
6. Convert pixel values in the training data into X and Y positions on the photos so that we can see the most common positions.
7. Get dimensions of the images
8. Target pixel location histogram - 40 , 80 , 160 , 320 bins


**Gaussian 2D Layer**

Instead of predicting per-pixel mask if I predict only 6 numbers mu_x, mu_y, sigma_x, sigma_y, sigma_xy, scale that will define a 2d gaussian. Thus, instead of autoencoder-like networks I can use simple CNNs.

Does not improve performance. Different way of approaching this problem.

**Long submission**

We spent the most amount of time doing this. 


1.   Used Maxout activations
2.   The models output both a mask and a probability that the image contains a mask
3.   Use some additional "auxiliary" downsampled mask to train the models
4.   PCA post-processing to get "realistic" masks
5.   Binary cross-entropy performs better than dice-like loss


I decided to use some deep learning as it works so well on images. We used multiple architectures to try out and all were fully-connected CNN or U-net.

**Pre-processing stage**


*  Image scaling - The images have a size of 580x420. Caused memory overflow while training. Rescaled images to 128x128.This also mean I stretched the images (the scale factor isn’t the same for both axes), but this didn’t seem to hinder performances much.


*  Removing contradictory images - I decided to remove the training images without a mask that were close to another training image with a mask. I computed the similary between two images by computing a signature for each image as follow : I divided the image in small 20x20 blocks and then compute a histogram of intensity in each block. All the histograms for the image are then concatenated into a big vector.This results in a distance matrix for all the training set images, which is then thresholded to decide which images should be removed. In the end, I kept 3906 training images (out of the 5500).


*  Architecture - The model has 95000 parameters. I found difficult to train models with more parameters without overfitting and I got really good results with 50000 parameter models as well.I used some very simple data augmentation (small rotation, zoom, shear and translation) but it did not help performance. 

**Post-processing stage**

I tried three different post-processing approaches :


*  Morphological close to remove small holes. This helped a bit

*  Fitting an ellipse to the mask and filling the ellipse. This didn't help during validation.

*  Using a PCA decomposition of the training masks and reconstruct the predicted mask using a limited number of principal components. This seemed to help more consistently (although just a little bit).


**PCA cleaning** - The idea of using PCA to post-process came from the Eigenface concept. Basically, you consider your image as a big vector and you then do PCA on the training masks to learn “eigenmasks”.

To clean a predicted mask using this method, you project the cleaned mask on the subspace found by PCA and you then reconstruct using only the 20 (in my case) first principal components. This forces the reconstructed mask to have a shape similar to the training masks.

**Loss and mask percentage** - I tried using dice coefficient as a loss, but I always got better results by using binary cross-entropy. Lowering the thresholds to get higher number of predicted masks leads to too many false positive.So 49% of the images should have mask and I tried to optimize the threshold on the binary and mask output to get close to 49% predicted masks. 

**Output**

Instead of submitting an exhaustive list of indices for our segmentation, the output is pairs of values that contain a start position and a run length. E.g. '2 5' implies starting at pixel 2 and running a total of 5 pixels (2,3,4,5,6). The metric checks that the pairs are sorted, positive, and the decoded pixel values are not duplicated



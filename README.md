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

*  **The dataset consists of 5635 training images and their masks, and 5508 testing images. **


About the data-

**Train.zip** file contains the training set images, named according to subject_imageNum.tif. Every image with the same subject number comes from the same person. This folder also includes binary mask images showing the BP segmentations.

**Test.zip** contains the test set images, named according to imageNum.tif. We predict the BP segmentation for these images. There is no overlap between the subjects in the training and test sets.


**Dataset Credits : Qure.ai - Deep learning products that fit your radiology workflow**


**Approaches we've tried so far**- 

Exploratory Data Analysis - All our training data is in one column, titled pixels.

train_masks.csv gives the training image masks in run-length encoded format. So, we need to segment this data into something we can plot!

Steps - 

1.  Separate run-lengths and pixel locations into seperate lists
2.  Get number of data points in each image
3.  Get all absolute target values
4.  Remove NaNs

We have seperated the run-length from pixel location, removed NaN data (where there is no data) and also got the number of data points in each image.Now we can plot some graphs!

5. Image analysis
6. Convert pixel values in the training data into X and Y positions on the photos so that we can see the most common positions.
7. Get dimensions of the images
8. Target pixel location histogram - 40 , 80 , 160 , 320 bins







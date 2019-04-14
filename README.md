# Eliminating surgical pain

Surgery is not easy for everyone. It brings discomfort and often involves significant post-surgical pain.

Currently, patient pain is frequently managed through the use of **narcotics that bring a bevy of unwanted side effects**.

Our solution aims to improve pain management through the use of indwelling catheters that block or mitigate pain at the source before the surgery begins.

Pain management catheters reduce dependence on narcotics and speed up patient recovery.

In our solution, we identify and segment a collection of nerves called the **Brachial Plexus (BP)** in ultrasound images. 

Our model  build a model can identify **nerve structures in a dataset of ultrasound images of the neck**. Doing so would improve catheter placement and contribute to a painless surgery.


Dataset - A large training set of images where the nerve has been manually attributed by doctors. These doctors were trained by experts and instructed to attribute images where they felt confident about the existence of the BP nerve.

Please note these important points:


*  The dataset contains images where the BP is not present. Our algorithm predicts no pixel values in these cases.

*  As with all human-labeled data, we do find noise, artifacts, and potential mistakes in the ground truth.

*  Identical images do exist in the dataset. Revealed in EDA.

*  Dataset uses run-length encoding (RLE) on the pixel values.



**Approaches we've tried so far**- 



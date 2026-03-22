# IndoFashion Image Classification

## Approach
This project focuses on classifying Indian ethnic clothing into **15 categories** using deep learning and transfer learning.

### Dataset
The main dataset used is the IndoFashion dataset, which contains 106K images. For a fair evaluation, the author ensures an equal distribution of classes in the validation and test sets, with each set consisting of 500 samples per class. Since the requirement suggested creating a smaller subset by selecting 500 images from each category for training and testing, I used the validation dataset from the main dataset, as it already fulfils this category-based requirement.

### Data Splitting
I have used stratified sampling to split the dataset into train, and test sets. This ensures that all classes are represented proportionally in each split. 

### Data Loading
A custom TensorFlow dataset class name **`ImageDataLoader`** is implemented to efficiently read images directly from their file paths, apply preprocessing, and augment data for training. 

---

## Model Architecture
The model is built using **transfer learning with MobileNetV2** as the base model.

### MobileNetV2 Overview
MobileNetV2 is a lightweight convolutional neural network designed for high efficiency on mobile and resource-constrained devices. For a dataset of 7,500 images, which is considered "small" in deep learning, MobileNetV2 is an ideal choice for several reason:

- **Transfer Learning:**
MobileNetV2 comes pre-trained on the ImageNet database (over 1 million images). By using this pre-trained version, the model already "knows" what basic shapes, textures, and objects look like. You only need to "fine-tune" the last few layers to learn your specific 7,500 images.
  
- **Faster Training:**
With only 7,500 images, you can train or fine-tune MobileNetV2 in minutes rather than hours on a standard computer, allowing you to test different settings quickly.

- **Low Resource Usage:**
 It requires significantly less memory and processing power, meaning you can train it on a basic laptop without needing expensive, high-end GPUs

---

## Training Process

- **Optimizer:** Adam  
- **Loss Function:** Sparse Categorical Cross-Entropy  
- **Batch Size:** 32  

### Learning Rate
- Initial: `0.001`  
- Fine-tuning: `1e-5`  

### Data Augmentation
- Random horizontal flips  
- Brightness adjustments  

### Transfer Learning Strategy
To maximize the performance of MobileNetV2 on this dataset, I employed a two-phase transfer learning strategy:
#### 1. Freezing the Base Model
Initially, all layers of the pre-trained MobileNetV2 were "frozen." This ensures that the high-quality features (edges, textures, shapes) already learned from ImageNet are preserved. During this phase, only the new, custom classification head is trained to map these features to our specific classes.

#### 2. Fine-Tuning
Once the top layers were stable, I "unfroze" the top layers of the base model and re-trained them with a very low learning rate.

---

## Evaluation Results

- **Accuracy:**
~78% test accuracy, with MobileNetV2 performing well given the small dataset size.   
- **Confusion Matrix:**
Row-wise normalized confusion matrix shows percentages of correctly and incorrectly classified images per class, highlighting which categories are most frequently confused. 

- **Classification Report:**
Precision, recall, and F1-scores per class provide detailed insights into model performance across all 15 categories. 

---

## Model Comparison: MobileNetV2 vs ResNet50
To evaluate model performance, a comparison was conducted between MobileNetV2 and ResNet50 using the same dataset and training pipeline.

### Key Observations

#### Training Time
ResNet50 required significantly more training time, approximately 2× longer than MobileNetV2. This is expected due to its deeper and more complex architecture. 

#### Accuracy
- **MobileNetV2:** ~78%  
- **ResNet50:** ~61%
MobileNetV2 outperformed ResNet50 by a substantial margin on this dataset.

#### Per-Class Performance
- MobileNetV2 achieved above 80% accuracy for most classes 
- ResNet50 struggled, with many classes below 50% accuracy 

This indicates that ResNet50 had difficulty generalizing across multiple categories in this dataset.

---

### Analysis

The results suggest that MobileNetV2 is better suited for this task, primarily due to:
- Smaller dataset size (~500 images per class)
- Lower model complexity, reducing the risk of overfitting
- Faster convergence during training

In contrast, ResNet50, being a deeper and more complex model, likely overfitted or failed to generalize effectively given the limited dataset size.

---

## Conclusion

For this image classification task, MobileNetV2 provides a better balance between performance and efficiency, achieving higher accuracy with faster training time

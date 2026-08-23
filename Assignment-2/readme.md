# Bird Species Recognition Using CNNs

A deep learning experiment for classifying bird images into **200 different species** using the CUB-200-2011 dataset.

This work compares three approaches with different starting points:

**Custom CNN → MobileNetV2 → ResNet50**

The first model learns everything from the provided dataset, while the other two start with knowledge learned from ImageNet. The experiment was used to understand whether pretrained visual features can improve performance on a fine-grained bird classification task.

---

## At a Glance

| Item               | Details                           |
| ------------------ | --------------------------------- |
| Dataset            | CUB-200-2011                      |
| Classes            | 200 bird species                  |
| Total Images       | 11,788                            |
| Models             | Custom CNN, MobileNetV2, ResNet50 |
| Framework          | TensorFlow / Keras                |
| Environment        | Google Colab                      |
| Best Result        | MobileNetV2                       |
| Best Test Accuracy | **57.58%**                        |

---

## Why Bird Classification?

Classifying birds is not always as simple as identifying completely different objects.

Two species may have similar colours, body shapes, feather patterns, or poses. Because of these small differences, the task falls under **fine-grained image classification**.

This project was used to investigate how different CNN architectures deal with that problem and how much pretrained knowledge can help.

---

# Dataset Used

The experiments are based on the **Caltech-UCSD Birds-200-2011 (CUB-200-2011)** dataset.

The dataset contains **11,788 RGB images covering 200 bird species**. The official dataset has 5,994 training images and 5,794 test images.

### Dataset Breakdown

| Split                            | Images |
| -------------------------------- | -----: |
| Original training set            |  5,994 |
| Training used in this experiment |  5,094 |
| Validation                       |    900 |
| Test                             |  5,794 |

The original test set was kept separate from the training and validation images.

### Input Format

Images are converted to:

```text
224 × 224 × 3
```

where the three channels represent RGB.

### Dataset Source

https://www.kaggle.com/datasets/wenewone/cub2002011

---

# Three Models, Three Starting Points

## Custom CNN

The Custom CNN is the only model in the experiment that does not use pretrained weights.

Its filters and representations are learned from the CUB-200-2011 images during training.

It is mainly useful as a baseline because it shows what can be achieved without transferring knowledge from another dataset.

---

## MobileNetV2

MobileNetV2 starts with **ImageNet pretrained weights**.

Instead of immediately retraining the complete network, the pretrained portion is first kept frozen. A new output layer is trained for the 200 bird categories.

After that, selected layers are unfrozen and fine-tuned with a smaller learning rate.

MobileNetV2 is also interesting because it provides a relatively compact alternative to larger architectures.

---

## ResNet50

ResNet50 follows the same general transfer-learning strategy.

It starts from ImageNet weights, trains the newly added classification head and then fine-tunes selected final layers.

The main reason for including it is to compare MobileNetV2 with a much larger and deeper architecture.

An important point is that **ResNet50 was not one of the models compared in the supplied research paper**. It was added separately to this practical.

---

# What Happens to an Image?

The notebook follows this general pipeline:

```text
Bird Image
    ↓
Resize to 224 × 224
    ↓
Preprocessing
    ↓
Training Augmentation
    ↓
        ┌───────────────┐
        │               │
        ↓               ↓
   Custom CNN     Pretrained Network
                        │
                  ┌─────┴─────┐
                  ↓           ↓
             MobileNetV2   ResNet50
                  │           │
                  └─────┬─────┘
                        ↓
                  Fine-Tuning
                        ↓
                  200 Classes
                        ↓
                   Prediction
```

---

# Image Augmentation

The training images are modified during training to introduce additional variation.

The implemented augmentation includes:

* Rotation
* Horizontal flipping
* Zoom/scaling
* Translation

For MobileNetV2 and ResNet50, the preprocessing required for their pretrained ImageNet features is also applied.

---

# How the Pretrained Models Were Trained

The pretrained networks were handled in two phases.

### Phase 1

The original pretrained backbone is frozen.

Only the newly added classification part is trained for the bird dataset.

### Phase 2

Selected final layers are made trainable.

Fine-tuning is then performed with a smaller learning rate so that the pretrained features can adapt to the new classification problem without changing too aggressively.

Batch Normalization layers are kept frozen during this process.

---

# Training Setup

The main configuration used in the notebook was:

| Setting                 | Custom CNN | MobileNetV2 | ResNet50  |
| ----------------------- | ---------- | ----------- | --------- |
| Input                   | 224 × 224  | 224 × 224   | 224 × 224 |
| Batch size              | 16         | 16          | 16        |
| Optimizer               | Adam       | Adam        | Adam      |
| Learning rate           | 3e-4       | 1e-4        | 1e-4      |
| Fine-tuning LR          | —          | 1e-5        | 1e-5      |
| Initial training epochs | Up to 30   | Up to 30    | Up to 30  |
| Fine-tuning epochs      | —          | Up to 15    | Up to 15  |
| Pretrained weights      | No         | ImageNet    | ImageNet  |
| Augmentation            | Yes        | Yes         | Yes       |
| Early stopping          | Yes        | Yes         | Yes       |
| Dropout                 | Yes        | Yes         | Yes       |

---

# The Actual Results

The final test-set results from the current notebook run are:

| Model           |   Accuracy |  Precision |     Recall |         F1 |
| --------------- | ---------: | ---------: | ---------: | ---------: |
| Custom CNN      |     12.89% |     13.37% |     12.89% |     10.99% |
| **MobileNetV2** | **57.58%** | **60.19%** | **57.58%** | **57.19%** |
| ResNet50        |      1.85% |      1.04% |      1.85% |      0.82% |

### Other Measurements

| Measurement   | Custom CNN | MobileNetV2 |   ResNet50 |
| ------------- | ---------: | ----------: | ---------: |
| Test Loss     |     3.7995 |  **1.5031** |     5.1883 |
| Training Time |  26.67 min |   19.92 min |  15.14 min |
| Parameters    |  1,293,288 |   2,637,320 | 24,163,660 |

---

# What I Observed

The most noticeable result is the gap between MobileNetV2 and the other two models.

### Custom CNN — 12.89%

The baseline model performed considerably lower than MobileNetV2.

Since it starts without pretrained knowledge, it has to learn useful visual representations directly from the available bird images.

### MobileNetV2 — 57.58%

MobileNetV2 produced the strongest result in this experiment.

It achieved the highest accuracy and F1-score, while also having far fewer parameters than ResNet50.

### ResNet50 — 1.85%

The ResNet50 result was unexpectedly low.

Despite having **24,163,660 parameters**, it did not outperform the smaller MobileNetV2 in this run.

This is one of the useful observations from the experiment: **a larger neural network is not automatically a better solution**. Architecture size alone does not determine the final result.

Preprocessing, optimization, fine-tuning and other training choices can have a major effect on transfer-learning performance.

---

# Looking Inside the Models

The notebook does more than calculate the final accuracy.

Several visual analyses are included:

### Accuracy and Loss

Training and validation curves are used to observe how the models behave while learning.

### Confusion Matrix

The predictions across all 200 bird classes are examined using confusion matrices.

This helps show which species are being confused with one another.

### Feature Maps

Intermediate feature maps are displayed to get an idea of how visual information is represented inside the networks.

### Sample Predictions

Test images are used to compare the actual bird species with the predicted class and model confidence.

### Learning Rate

Different learning-rate settings are also explored to observe their effect on validation performance and convergence.

---

# Research Paper Used

The practical uses the following paper as a reference:

**Enhanced Bird Species Image Recognition and Classification using MobileNet and InceptionV3 Transfer learning Architectures**

Authors:

* Sakthi Priya G.
* Vignesh Saravanan K.
* Dheetchana K.

Published in:

**Electronic Letters on Computer Vision and Image Analysis**
Volume 24, Issue 1, 2025
Pages 118–133

DOI:

`10.5565/rev/elcvia.2020`

### Published MobileNet Result

The paper reports:

| Metric   |  Paper |
| -------- | -----: |
| Accuracy | 74.60% |
| Loss     | 0.8685 |

These values are reference results from the paper and should not be confused with the results obtained in this notebook.

Our MobileNetV2 implementation achieved **57.58% test accuracy**.

The two results are not expected to be identical because the implementations, model setup and training conditions are not necessarily the same.

---

# Libraries and Tools

The project uses:

* **Python**
* **TensorFlow**
* **Keras**
* **NumPy**
* **Pandas**
* **Scikit-learn**
* **Matplotlib**
* **Seaborn**
* **KaggleHub**
* **Google Colab**

TensorFlow/Keras is used for the neural networks, while the remaining libraries support data preparation, evaluation, visualization and dataset access.

---

# Running the Project

The notebook is designed to run using **Google Colab**.

### 1. Install the required packages

```bash
pip install -U kagglehub seaborn scikit-learn
```

### 2. Open the notebook

Open the bird classification `.ipynb` file from this repository in Google Colab.

### 3. Enable GPU

Use:

```text
Runtime → Change runtime type → GPU
```

GPU execution is recommended because multiple deep learning models are trained.

### 4. Run the cells

Execute the notebook from top to bottom.

The notebook handles:

```text
Environment setup
      ↓
Dataset download
      ↓
Dataset verification
      ↓
Train / Validation / Test preparation
      ↓
Image preprocessing
      ↓
Augmentation
      ↓
Custom CNN training
      ↓
MobileNetV2 training
      ↓
MobileNetV2 fine-tuning
      ↓
ResNet50 training
      ↓
ResNet50 fine-tuning
      ↓
Evaluation
      ↓
Visual analysis
      ↓
Model comparison
```

KaggleHub is used to obtain the dataset during execution.

---

# Current Repository

The `Assignment-2` directory contains the notebook, reference paper and README.

```text
Assignment-2/
│
├── Bird Species Classification Notebook
├── Reference Research Paper
└── README.md
```

The CUB-200-2011 dataset itself is not stored in the repository; it is downloaded when the notebook is executed.

---

# Limitations

The current experiment has some practical limitations:

* Several bird species look very similar.
* Background and image quality can influence predictions.
* The pretrained networks were originally trained on ImageNet, not specifically on bird species.
* Model performance depends on the selected training configuration.
* ResNet50 requires substantially more parameters than MobileNetV2.
* The current ResNet50 run performed much worse than the other models.

---

# If This Project Were Extended

Some improvements that could be explored next are:

* **Grad-CAM** for explaining individual predictions
* More systematic hyperparameter tuning
* Additional pretrained architectures
* Ensemble-based prediction
* Testing on photographs outside the CUB dataset
* TensorFlow Lite conversion
* Edge-device deployment
* CPU vs GPU inference comparison
* Memory-usage analysis
* Cross-dataset evaluation

These extensions are suggested in the practical as possible directions for further development.

---

# Final Takeaway

This experiment shows the practical difference between training a CNN from scratch and adapting an already pretrained network.

The **Custom CNN reached 12.89%**, while **MobileNetV2 reached 57.58%**, making MobileNetV2 the strongest model in the current run.

The ResNet50 result was only **1.85%**, despite its much larger parameter count. This makes the comparison particularly useful because it demonstrates that increasing network size does not by itself guarantee improved performance.

The project therefore provided hands-on experience with image classification, CNNs, transfer learning, fine-tuning, pretrained models and model evaluation.

---

# References

1. Sakthi Priya G., Vignesh Saravanan K., and Dheetchana K., *Enhanced Bird Species Image Recognition and Classification using MobileNet and InceptionV3 Transfer learning Architectures*, Electronic Letters on Computer Vision and Image Analysis, Vol. 24, Issue 1, pp. 118–133, 2025.

2. **CUB-200-2011 Dataset**
   https://www.kaggle.com/datasets/wenewone/cub2002011

3. **Official Journal Page**
   https://elcvia.cvc.uab.cat/article/view/2020

4. **DOI**
   https://doi.org/10.5565/rev/elcvia.2020

5. **Official PDF**
   https://ddd.uab.cat/pub/elcvia/elcvia_a2025v24n1/elcvia_a2025v24n1p118.pdf

6. K. He, X. Zhang, S. Ren, and J. Sun, *Deep Residual Learning for Image Recognition*, Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, 2016.

7. A. G. Howard et al., *MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications*, 2017.

---

# Team

**Sakshi Walunj**
PRN: 202502110007

**Shreya Dattaram Bhosale**
PRN: 202502110010

**Dhanashri Shah**
PRN: 202502110008

**T.Y. B.Tech — CSE (AI & ML)**
**Generative AI Lab**


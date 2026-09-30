# AIDL A02 Project

<p align="left">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter" />
  <img src="https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" alt="Google Colab" />
  <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white" alt="TensorFlow" />
  <img src="https://img.shields.io/badge/Keras-D00000?style=for-the-badge&logo=keras&logoColor=white" alt="Keras" />
  <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" alt="OpenCV" />
  <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" alt="NumPy" />
  <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white" alt="scikit-learn" />
  <img src="https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge" alt="Matplotlib" />
</p>

This is a repo containg the jupiter file for the AIDL A02 module.

## Student: Zelios Andreas mscaidl0142
## Module Instructor: [Panagiotis Kasnesis](https://github.com/ounospanas)

### Project's Dataset - Objective

I choose to create a custom dataset, all data can be found [here](/dataset). 

The objective is to classify handsign language words that indicate danger:
- afraid
- allergy
- medicine
- doctor
- worried
- pain

example words used can be seen [here](/draft_word_examples).

### Aim of the project:
This project focuses on creating:

- Custom Convolutional Networks
- Utilizing Transfer learning

Improving results with:

- Data Augmentation

## Running

1. Add all the subfolders in [/dataset](/dataset) to your google drive, in a folder structure
`AIDL02/PROJECT/DATA`, so in the **DATA** folder.

2. Then use the jupiter file colab that is linked with your drive account.

## Results

| # | model | val_loss | val_accuracy |
|---|---|---|---|
| 1 | **Network 3 (custom CNN, no augmentation)** | **0.05284** | **0.9844** |
| 2 | Network 3 + shift | 0.07772 | 0.9688 |
| 3 | VGG16 + rotation + zoom | 0.07882 | 0.9844 |
| 4 | Network 3 + rotation | 0.07950 | 0.9688 |
| 5 | Network 3 + rotation + shift | 0.09565 | 0.9766 |
| 6 | VGG16 | 0.15535 | 0.9531 |
| 7 | VGG16 + zoom | 0.16315 | 0.9531 |
| 8 | VGG16 + rotation | 0.19230 | 0.9375 |
| 9 | ResNet50 + shift | 0.23887 | 0.9141 |
| 10 | Network 2 | 0.24319 | 0.9375 |
| 11 | ResNet50 + rotation | 0.24642 | 0.9297 |
| 12 | ResNet50 (last 7 unfrozen) | 0.24983 | 0.8984 |
| 13 | ResNet50 + rotation + zoom | 0.25051 | 0.9062 |
| 14 | ResNet50 + rotation + zoom + shift | 0.26603 | 0.8906 |
| 15 | ResNet50 + zoom | 0.29461 | 0.8984 |
| 16 | Network 1 | 0.47093 | 0.8516 |
| 17 | ResNet50 (all frozen) | 0.50041 | 0.8828 |
| 18 | VGG16 + shift | 0.56934 | 0.7500 |
| 19 | ResNet50 + channel shift | 0.82633 | 0.6953 |
| 20 | Network 3 + channel shift | 1.30791 | 0.4531 |
| 21 | ResNet50 + brighten | 1.64797 | 0.2656 |
| 22 | VGG16 + channel shift | 1.70714 | 0.2578 |
| 23 | Network 3 + brighten | 1.78954 | 0.1797 |
| 24 | Network 3 + zoom | 1.79523 | 0.1328 |
| 25 | ResNet50 (all unfrozen, raw) | 1.79945 | 0.1484 |
| 26 | VGG16 + brighten | 1.80622 | 0.1641 |

`val_accuracy` ties **Network 3** and **VGG16 + rotation + zoom** at 0.9844 (126 of 128 validation images), and `val_loss` breaks the tie in favour of Network 3.

Two things stand out from this ranking:

- the custom network beats both pretrained backbones, and
- augmentation never improved the model it was applied to, it only ever matched it or made it worse.

Network 3 was choosen as the best:

- saved checkpoint: **epoch 187**
- **val_loss 0.05284**
- **val_accuracy 0.9844**

### Training Curve - Confususion Matrix
![training_curve](/assets/training_curve.png)

The model fit the train set quickly, but val stayed near chance for ~90 epochs before recovering as the learning rate decayed. It ends at ~0.96-0.97 val accuracy, and since the best epoch was checkpointed, the saved model comes from the stable late plateau. The result is good and usable.

![!confusion_matrix](/assets/confusion_matrix.png)

- The 1 missclassification of allergy was medicine, which is good, someone would likely understand that medicine might correlate to allergy problem.
- The 2 missclassifications in medicine is more worrysome because doctor does not directly correlate to that, if the person is looking for their perscription they might not get it in time.

> For transparency: some of the augmented variants did score higher on the test set (VGG16 + rotation + zoom 0.9836, Network 3 + rotation + shift 0.9781). But they ranked below Network 3 on validation, and picking them because of their test score is exactly the test set selection this notebook avoids, it would stop the test numbers being an honest estimate of unseen performance.


# Contrastive Representation Learning for Electroencephalogram Classification
**Pasquale Ricciulli 2115446**  
[Original Paper](https://proceedings.mlr.press/v136/mohsenvand20a.html)

This repository provides a simplified reimplementation of the paper [Contrastive Representation Learning for Electroencephalogram Classification](https://proceedings.mlr.press/v136/mohsenvand20a.html). Below, you will find an overview of the steps and methodology implemented in this project.

---

## 🚀 **Project Overview**
The implementation consists of the following phases:
1. **Preprocessing**: Resampling, filtering, and artifact removal.
2. **Data Augmentation**: Applying diverse transformations to improve robustness.
3. **Encoders**: Training Recurrent and Convolutional encoders for contrastive learning.
4. **Classifier**: Feature extraction and classification tasks using encoded data.
5. **Evaluation**: Comparison of metrics between the outputs of convolutional and recurrent encoders.

---

## 📊 **Preprocessing Phase**
- **Resampling**: The entire dataset was resampled to **200 Hz**.
- **Filtering**: A fifth-order band-pass Butterworth filter was applied.
- **Artifact Removal**: Channels with voltages exceeding **500 µV** were excluded, as these typically represent artifacts.

---

## 🔄 **Data Augmentation**
Before feeding the data to the encoders, the EEG signals were divided into **chunks of size 4000**. For each chunk, two random augmentations were applied from the following transformations:

| **Transformation**                 | **Min**      | **Max** |
|-------------------------------------|--------------|---------|
| Amplitude scale                     | 0.5          | 2       |
| Time shift (samples)                | -50          | 50      |
| DC shift (µV)                       | -10          | 10      |
| Zero-masking (samples)              | 0            | 150     |
| Additive Gaussian noise (σ)         | 0            | 0.2     |
| Band-stop filter (5 Hz width) (Hz)  | 2.8          | 82      |

---

## 🧠 **Encoders**
- **Recurrent Encoder**:
  - Trained for **24 epochs** with batch size **40**.
- **Convolutional Encoder**:
  - Trained for **24 epochs** with batch size **40**.

Below is a visualization of the architectures of the two encoders:

![Architectures](images/Architectures.png)

---

## 🏷️ **Classifier**
- Each EEG channel is processed through the encoder, and the **4-dimensional output sequences** are concatenated.
- The classifier takes a **4×20-dimensional sequence of length 200** (representing 1-second epochs of EEG data).
- **Modifications**:
  - The last dense layer's output dimension is set to the number of classes.
  - A **LogSoftmax layer** is added.
- **Loss Function**:
  - Negative log-likelihood loss is computed alongside the LogSoftmax layer.

---

## 📈 **Evaluation**
The performance metrics of the **Recurrent Encoder** and the **Convolutional Encoder** were compared to evaluate the classification accuracy and the effectiveness of the architectures.

---

## 📜 **References**
- [Contrastive Representation Learning for Electroencephalogram Classification](https://proceedings.mlr.press/v136/mohsenvand20a.html)

---

## 🛠️ **How to Use**
1. Clone the repository:
   ```bash
   git clone https://github.com/your-repository-name.git

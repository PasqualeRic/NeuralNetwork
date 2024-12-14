# **Contrastive Representation Learning for Electroencephalogram Classification**
**Pasquale Ricciulli 2115446**  
[Original Paper](https://proceedings.mlr.press/v136/mohsenvand20a.html)

This repository provides a simplified reimplementation of the paper [Contrastive Representation Learning for Electroencephalogram Classification](https://proceedings.mlr.press/v136/mohsenvand20a.html). Below, you will find a detailed description of the methodology, pipeline, and results of this project.

---

## **What does this project do?**

This project leverages **contrastive representation learning** to classify emotional states (negative, neutral, positive) from **EEG (electroencephalogram)** data. The aim is to create robust feature representations by training on augmented EEG signals and using a contrastive loss function. These representations are later used to classify emotional states with high accuracy.

---

### **How does it work?**

The pipeline is divided into the following stages:

### 1. **Preprocessing EEG Data**
The raw EEG signals are noisy and contain unwanted artifacts. The preprocessing pipeline includes:
- **Resampling**: All signals are resampled to a uniform frequency of **200 Hz**.
- **Band-pass Filtering**: A fifth-order **Butterworth filter** is applied to retain only the relevant frequency band for EEG analysis.
- **Artifact Removal**: Channels with extreme voltage values (**500 µV**) are removed, as these typically represent artifacts.

This ensures the data is clean and consistent, making it suitable for further analysis.

---

### 2. **Data Augmentation**
To increase the diversity of the data and improve the encoder’s robustness, augmentation techniques are applied. For each EEG chunk:
- **Two different augmentations** are applied to simulate variability (e.g., noise, time shifts, amplitude changes).
- Augmentation techniques include:
  - **Amplitude scaling**
  - **Time shifts**
  - **DC shifts**
  - **Zero masking**
  - **Additive Gaussian noise**
  - **Band-stop filtering**

Additionally, a **chunk from a different file or trial** is selected for contrastive training, enabling the model to distinguish between similar and different chunks.

---

### 3. **Contrastive Learning**
The core idea is to train two types of encoders (Recurrent and Convolutional) using a **contrastive loss function**:
- **Positive Pairs**: Two augmented views of the same EEG chunk.
- **Negative Pairs**: A chunk from a different file or trial.

The contrastive loss maximizes the similarity between positive pairs and minimizes the similarity between negative pairs, allowing the encoder to learn robust feature representations.

---

### 4. **Emotion Recognition Classifier**
Once the encoders are trained, they are **frozen**, and a classifier is trained on the frozen representations to classify EEG signals into three emotional states:
- **0 (Negative)**: Represents negative emotional responses.
- **1 (Neutral)**: Represents neutral or baseline emotional responses.
- **2 (Positive)**: Represents positive emotional responses.

The classifier uses the output of the encoder as input and is trained using a **cross-entropy loss function**.

---

### 5. **Evaluation**
The models are evaluated using:
- **Accuracy**
- **Precision**
- **Recall**
- **F1-score**
- **Confusion Matrix**

Both the **Recurrent Encoder** and the **Convolutional Encoder** are compared to determine which architecture performs better on the emotion recognition task.

---

## **Project Pipeline**

### **1. Preprocessing**
- **Resampling**: The EEG data is resampled to **200 Hz**.
- **Filtering**: A fifth-order band-pass Butterworth filter is applied to remove unwanted frequencies.
- **Artifact Removal**: Channels with voltages exceeding **±500 µV** are excluded.

#### Example of the raw EEG signal:
![Original Signal](images/Segnali_originali.png)

#### Example of the preprocessed EEG signal:
![Processed Signal](images/Segnali_ridotti.png)

---

### **2. Data Augmentation**
Two augmentations are applied to each EEG chunk to improve the robustness of the encoders. Augmentation techniques include:

| **Transformation**                 | **Min**      | **Max** |
|-------------------------------------|--------------|---------|
| Amplitude scale                     | 0.5          | 2       |
| Time shift (samples)                | -50          | 50      |
| DC shift (µV)                       | -10          | 10      |
| Zero-masking (samples)              | 0            | 150     |
| Additive Gaussian noise (σ)         | 0            | 0.2     |
| Band-stop filter (5 Hz width) (Hz)  | 2.8          | 82      |

#### Example of an augmented EEG signal:
![Augmented Signal](images/Augmentation.png)

---

### **3. Contrastive Learning**
- The **contrastive loss function** maximizes the similarity between two augmented views of the same EEG chunk while minimizing the similarity between different chunks.
- The encoders (Recurrent and Convolutional) are trained using this objective.

#### Encoder Architectures:
![Architectures](images/Architectures.png)

---

### **4. Emotion Recognition Classifier**
Once the encoders are trained, they are **frozen**, and a classifier is trained on top of the frozen representations. The classifier predicts one of three emotional states:
- **0 (Negative)**
- **1 (Neutral)**
- **2 (Positive)**

---

## **Evaluation**

The models were evaluated using a test set, and the performance metrics for the Recurrent and Convolutional encoders are provided below.

### **Recurrent Encoder Performance**
- **Accuracy**: **81%**
- Confusion Matrix:

![Recurrent Encoder Confusion Matrix](images/evaluation_metrics_recurrent.png)

---

### **Convolutional Encoder Performance**
- **Accuracy**: **78%**
- Despite fewer training epochs (10 vs. 24), the Convolutional Encoder performed close to the Recurrent Encoder.
- Confusion Matrix:

![Convolutional Encoder Confusion Matrix](images/evaluation_convolutional.png)

---

## **Key Results**
- The **Recurrent Encoder** slightly outperformed the Convolutional Encoder, achieving **81% accuracy** on the test set.
- The **Convolutional Encoder** demonstrated efficiency, achieving **78% accuracy** with fewer training epochs.
- I believe that convolution is the best model if it is trained with the same epochs as the recurrent model.

---

## **References**
- [Contrastive Representation Learning for Electroencephalogram Classification](https://proceedings.mlr.press/v136/mohsenvand20a.html)

---

## **How to Use**

### 1. Clone the repository:
```bash
git clone https://github.com/your-repository-name.git

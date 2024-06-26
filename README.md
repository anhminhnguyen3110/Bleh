# Face Recognition Attendance System

## Video Demonstration
[Click here to watch the video](https://youtu.be/tf6SQ8tDwUs)  
[![Watch the video](https://media.cnn.com/api/v1/images/stellar/prod/200209224400-20200209-facial-recognition-gfx.jpg?q=w_1600,h_900,x_0,y_0,c_fill)](https://youtu.be/tf6SQ8tDwUs)

## Installing dlib on Windows 10

This is a step-by-step guide to install dlib on Windows 10. Make sure to follow the steps carefully and in order. (For Python 3.8 only)

### Prerequisites

- Python version: 3.8.0 (Date: 2019-10-14)

### Installation Steps

1. **Install dlib**

    Enter the following command in the terminal:

    ```bash
    pip install dlib-19.19.0-cp38-cp38-win_amd64.whl
    ```

2. **Install CMake**

    Then, install CMake using the following command:

    ```bash
    pip install cmake
    ```

3. **Update pip**

    Next, update pip using the following command:

    ```bash
    pip install --upgrade pip
    ```

4. **Install face_recognition**

    Finally, you can install `face_recognition` using the following command:

    ```bash
    pip install face-recognition
    ```

## Dataset

For this project, a face recognition dataset consisting of 31 different classes, each representing a distinct individual, will be utilized. The dataset is structured with folders corresponding to each class containing a collection of images of the respective person. Notably, all individuals in the dataset are famous personalities, allowing for a diverse and challenging set of face images to evaluate the performance of the developed models. The images within each class capture various angles, expressions, and lighting conditions, providing a comprehensive representation of the individual's facial features.

**Dataset Link:** [Dataset](https://www.kaggle.com/datasets/vasukipatel/face-recognition-dataset)

## Methodology

### CNN Model Architecture

The CNN model was designed with the following architecture:

- **Input Layer**: The input images were resized to 128x128 pixels.
- **Convolutional Layers**: Several convolutional layers with ReLU activation functions were used to extract features from the images. Each convolutional layer was followed by a max-pooling layer to reduce the spatial dimensions.
- **Dense Layers**: Fully connected layers were added after flattening the output of the convolutional layers. Dropout layers were included to prevent overfitting.
- **Output Layer**: A softmax activation function was used in the final layer to classify the input images into one of the 31 classes.

The architecture is summarized as follows:

1. Conv2D -> ReLU -> MaxPooling2D
2. Conv2D -> ReLU -> MaxPooling2D
3. Flatten
4. Dense -> ReLU
5. Dropout
6. Dense -> Softmax

### Fine-Tuning Pretrained Models

For fine-tuning the pretrained models, I used the following approach:

1. **Model Selection**: I selected VGG16, a well-known convolutional neural network model pre-trained on the ImageNet dataset.
2. **Freezing Layers**: The initial layers of the VGG16 model were frozen to retain the pre-trained weights. This helps leverage the learned features from a large dataset (ImageNet).
3. **Adding Custom Layers**: New dense layers were added on top of the frozen layers to adapt the model to the specific task of face recognition. The architecture of the custom layers included a dense layer followed by a dropout layer to prevent overfitting and another dense layer with softmax activation for classification.

The architecture for the fine-tuned model is summarized as follows:

1. **Base Model (VGG16)**:
    - Include all layers up to a certain depth, excluding the top layers.
    - Freeze all layers to retain pre-trained weights.
2. **Custom Layers**:
    - Flatten
    - Dense -> ReLU
    - Dropout
    - Dense -> Softmax

### Haar Cascade Optimization

The Haar Cascade classifier was used as an optimization technique to improve the performance of the image classification model. Haar Cascade is an effective object detection method used to identify faces in an image.

1. **Face Detection**: The Haar Cascade classifier was used to detect faces in the input images. This step helps in focusing on the face region, eliminating background noise.
2. **Image Cropping**: Once the face was detected, the image was cropped to include only the face region. This reduces the input size and ensures that the model trains on relevant features.
3. **Data Augmentation**: Data augmentation techniques such as rotation, scaling, and flipping were applied to the cropped face images to increase the diversity of the training data.

#### Before and After Haar Cascade
Below are placeholders for images showing the effect of Haar Cascade optimization:

<p align="center">
    <img src="https://github.com/anhminhnguyen3110/Face-Attendance-System/assets/57170354/71b120fd-d4dc-48b9-874b-21a69f2d0a2e" width="600"/>
</p>

### Insights

#### CNN Model
The custom CNN model, though effective, had limitations in learning complex features due to the relatively small size of the dataset and the need for extensive parameter tuning. The dropout layers helped in preventing overfitting, but the validation accuracy was relatively low compared to the pretrained models.

#### Pretrained Model (VGG16)
Fine-tuning the VGG16 model provided significant improvements in performance. By freezing the early layers and training only the top layers, I was able to leverage the robust feature extraction capabilities of the pretrained model. This approach reduced the risk of overfitting and improved generalization, resulting in higher validation accuracy and a better ROC AUC score.

#### Haar Cascade Optimization
The use of Haar Cascade optimization improved the model's performance by focusing on the most relevant part of the image (the face) and reducing background noise. This pre-processing step allowed the model to learn more accurate features, resulting in higher validation accuracy and better overall performance.

## Results

The experiments conducted using various models yielded the following results:

### CNN Model Training Experiments

| Exp No | Configuration | Val Acc | ROC AUC | Chart Train Loss | Chart Train Accuracy |
|--------|----------------|---------|---------|------------------|----------------------|
| 1      | RS: 54, Img: 128x128, Dense: 128, Dropout: 0, Epoch: 30 | 20.9%   | 0.75    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/16ad0ac9-13ad-4ba6-8ca8-6947d6c6b73c) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/826299e0-dd05-455c-8da6-ad3b0a1d18ee) |
| 2      | RS: 54, Img: 128x128, Dense: 128, Dropout: 0, Epoch: 50 | 30.2%   | 0.78    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/a4e923f9-495d-44c6-a056-184121a1b4c7) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/43501e81-beae-4f46-8a9b-72e4a809b1c4) |
| 3      | RS: 54, Img: 128x128, Dense: 128, Dropout: 0.5, Epoch: 70 | 28.66%  | 0.84    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/a39f2951-00f6-48f2-8add-16d7c6da9008) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/db1ff572-321e-4d4f-ad0f-755257127294) |
| 4      | RS: 54, Img: 224x224, Dense: 128, Dropout: 0.5, Epoch: 70 | 30.33%  | 0.87    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/e07f3fd0-bc37-4a39-804c-7aafe53ae109) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/5c489771-15b7-485c-9755-6121ed467e74) |

#### Best CNN Model ROC Curve
<p align="center">
    <img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/5d7164b9-df75-4263-a5e4-80cbaad68931" width="600"/>
</p>

### Pretrained Model (VGG16) Training Experiments

| Exp No | Configuration | Val Acc | ROC AUC | Chart Train Loss | Chart Train Accuracy |
|--------|----------------|---------|---------|------------------|----------------------|
| 1      | RS: 54, Img: 128x128, Dense: 128, Dropout: 0, Epoch: 30 | 44.67%  | 0.93    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/ad3b0f43-047d-43dd-a389-3fa596edf800) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/90dd680a-98e9-4f3b-bb6f-2be6a4f8d761) |
| 2      | RS: 54, Img: 128x128, Dense: 128, Dropout: 0.3, Epoch: 50 | 34.84%  | 0.82    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/d3e5ef38-00a1-428e-95cd-ae1443b013e0) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/2cb6dbdb-343a-4715-af81-a85b0b04295e) |
| 3      | RS: 54, Img: 128x128, Dense: 128, Dropout: 0.15, Epoch: 70 | 38.52%  | 0.91    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/9e1ba7b5-ba5b-44fa-a59e-403afd1f82fd) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/d9e9e29d-2966-4ecd-affe-149e79fa5e4b) |
| 4      | RS: 54, Img: 224x224, Dense: 128, Dropout: 0.15, Epoch: 80 | 31.56%  | 0.90    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/3660a5aa-10e9-40ec-a22f-f1e7426adf85) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/ae0f3991-e3c5-4482-b08d-833f4ae8cd76) |

#### Best Pretrained Model (VGG16) ROC Curve
<p align="center">
    <img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/b06c992e-4ef5-4564-b354-ed6a2b4ff18a" width="600"/>
</p>

### Optimized Pretrained Model (VGG16) with Haar Cascade Experiments

| Exp No | Configuration | Val Acc | ROC AUC | Chart Train Loss | Chart Train Accuracy |
|--------|----------------|---------|---------|------------------|----------------------|
| 1      | RS: 54, Img: 128x128, Dense: 128, Dropout: 0, Epoch: 30 | 70.42%  | 0.97    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/f9f214f7-8548-4cd4-b6f7-aeaec39fa699) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/e0be24fb-8815-4205-a8f0-c1341983350d) |

#### Best Optimized Pretrained Model (VGG16) ROC Curve
<p align="center">
    <img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/bdb8d773-1e45-4dc0-a6d9-ceb8a287be4c" width="600"/>
</p>

### Experiments to Find the Top Performance Model (Using Optimized Technique)

| Exp No | Model Name | Configuration | Val Acc | ROC AUC | Chart Train Loss | Chart Train Accuracy |
|--------|------------|---------------|---------|---------|------------------|----------------------|
| 1      | VGG16      | RS: 54, Img: 128x128, Dense: 128, Dropout: 0.3, Epoch: 30 | 70.42%  | 0.97    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/496110fe-0ff2-4a20-8f26-15a1904bfde4) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/7e1c8a90-4fd5-46d1-928a-7527fca4fcb1) |
| 2      | ResNet50   | RS: 54, Img: 128x128, Dense: 128, Dropout: 0.3, Epoch: 30 | 4.92%   | 0.59    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/dcc7a640-a878-4b9b-891d-5379485c7077) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/e9251197-ed25-4ccd-83bb-ac4bbd2dc7f1) |
| 3      | DenseNet121| RS: 54, Img: 128x128, Dense: 128, Dropout: 0.3, Epoch: 30 | 64.79%  | 0.96    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/edc975bd-43d4-4a91-b969-033dcafe7376) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/db34480e-6ed5-4633-ac4a-2e4dbac17fca) |
| 4      | InceptionV3| RS: 54, Img: 128x128, Dense: 128, Dropout: 0.3, Epoch: 40 | 48.83%  | 0.94    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/d07bf1af-7dda-4093-84bb-9ed35d274127) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/c2c4f0a0-75ea-4178-bdba-146ce0328f43) |
| 5      | MobileNetV2| RS: 54, Img: 128x128, Dense: 128, Dropout: 0.3, Epoch: 40 | 63.85%  | 0.94    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/ee030b17-7c6c-4ee5-b6e5-bbd51098fce6) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/6a1a4de0-a7be-42c6-b90d-b1aa03ff7940) |
| 6      | EfficientNetB0| RS: 54, Img: 128x128, Dense: 128, Dropout: 0.3, Epoch: 40 | 4.69%   | 0.56    | ![Training Loss](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/80097ad8-d254-4bfc-bc7d-033ed031027f) | ![Training Accuracy](https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/7ce4f267-5288-4162-b396-cf350b868703) |

#### Best Performed Model (VGG16) ROC Curve
<p align="center">
    <img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/401595b8-8bcb-4178-ad31-70da2a90fad9" width="600"/>
</p>

## System Features

### Tkinter GUI

The system uses Tkinter for the user interface, which includes buttons for checking attendance, identifying faces, and registering new users. Each button is associated with a specific function.

### CV2 and Face Recognition

OpenCV (cv2) is used for real-time image processing. The `face_recognition` library, built on dlib, provides functions to find, encode, and compare faces.

#### Register New User

New users can be registered by capturing their facial data and storing the corresponding embeddings. Below is a placeholder for images demonstrating user registration.

<p align="center">
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/449504ee-e1b5-43e9-87f5-7b56217a7af0" width="650"/>
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/4766fa59-ec22-46b4-b3ac-dee4dbd7f9cf" width="650"/>
</p>

#### Check Attendance

The system logs attendance by identifying users in real-time and comparing live captures with stored facial embeddings. Below is a placeholder for images demonstrating the attendance check feature.

<p align="center">
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/40d19bb2-6e65-47ba-b4ca-923283ded1fe" width="650"/>
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/06ef2c15-bc47-4f8a-a64d-0505cd7340ee" width="650"/>
</p>

#### Identify Eyes & Smile

Additional features include identifying eyes and smiles using Haar cascades provided by OpenCV. Below is a placeholder for images demonstrating these features.

<p align="center">
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/e8a761a5-6496-4830-a020-6b6e3cb64462" width="650"/>
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/38260757-f697-43dc-9c72-9fdc8abcc8d8" width="650"/>
</p>

### Anti-Spoofing System

The anti-spoofing system ensures security by using the MiniFASNet architecture, which includes auxiliary supervision of the Fourier spectrum to differentiate between real and fake faces. Below is a placeholder for images demonstrating the spoofing detection system.

<p align="center">
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/87dafc8e-b64a-4917-b251-3adf6c83b94e" width="650"/>
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/61fb2d03-3860-459b-8ed8-7d25e45abeed" width="650"/>
<img src="https://github.com/anhminhnguyen3110/Face-attendance-system/assets/57170354/7a4552ce-7bee-4816-ae8e-a041aa95b6b6" width="650"/>
</p>

## Conclusion

The VGG16 model with Haar Cascade optimization achieved the best performance in this study. Configured with a random seed of 54, image size of 128x128, dense layer size of 128, dropout rate of 0.3, and trained for 30 epochs, it achieved a train accuracy of 98.34%, train loss of 0.11, validation accuracy of 70.42%, validation loss of 1.44, test accuracy of 70.42%, and a micro-average ROC curve area (AUC) of 0.97. Other models like ResNet50 and EfficientNetB0 did not converge effectively, while DenseNet121, InceptionV3, and MobileNetV2 showed decent performance but did not surpass VGG16.

These findings suggest that VGG16 optimized with Haar Cascade and appropriate configurations provides the best balance between accuracy and performance. Future work will focus on further fine-tuning hyperparameters, exploring more complex architectures, and reducing overfitting to enhance performance even further.

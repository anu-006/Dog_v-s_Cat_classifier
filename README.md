# Cat vs Dog Classifier

This project uses a **Convolutional Neural Network (CNN)** to classify images as either a **cat** or a **dog**.

The notebook is designed mainly for **Google Colab** and uses the Kaggle API to download the dataset.

## Project Structure

All files can be uploaded directly to the root of your GitHub repository.

```text
Cat_vs_Dog_Classifier
│
├── Cat_v_s_Dog_Classifier_CNN.ipynb
├── requirements.txt
└── README.md
```

No separate project folder is required.

## What This Project Does

The notebook follows these steps:

1. Imports TensorFlow/Keras and other required libraries.
2. Sets up the Kaggle API.
3. Downloads the Cats vs Dogs dataset from Kaggle.
4. Extracts the dataset.
5. Loads the training and testing images.
6. Resizes all images to `256 × 256`.
7. Converts pixel values from `0-255` to `0-1`.
8. Builds a CNN model.
9. Trains the model for 10 epochs.
10. Plots training and validation accuracy.
11. Plots training and validation loss.
12. Tests the trained model on individual cat and dog images.

## Dataset

The notebook downloads the dataset from Kaggle using:

```bash
kaggle datasets download -d princelv84/dogsvscats
```

The downloaded ZIP file is:

```text
dogsvscats.zip
```

The notebook extracts it into:

```text
/content
```

After extraction, the notebook expects the following folders:

```text
/content/train
/content/test
```

with class folders inside them.

The notebook loads the data using:

```python
keras.utils.image_dataset_from_directory()
```

The labels are inferred automatically from the folder names.

## Technologies Used

- Python
- TensorFlow
- Keras
- Matplotlib
- OpenCV
- Kaggle API

## CNN Architecture

The CNN used in the notebook has three convolution blocks followed by fully connected layers.

```text
Input Image
256 × 256 × 3
        ↓
Conv2D: 32 filters + ReLU
        ↓
Batch Normalization
        ↓
MaxPooling
        ↓
Conv2D: 64 filters + ReLU
        ↓
Batch Normalization
        ↓
MaxPooling
        ↓
Conv2D: 128 filters + ReLU
        ↓
Batch Normalization
        ↓
MaxPooling
        ↓
Flatten
        ↓
Dense: 128 + ReLU
        ↓
Dropout: 30%
        ↓
Dense: 64 + ReLU
        ↓
Dropout: 30%
        ↓
Output: 1 + Sigmoid
```

The final layer has one neuron with a **sigmoid** activation because this is a binary classification problem:

```text
Cat
vs
Dog
```

The notebook uses:

```text
cat = 1
dog = 0
```

## Image Preprocessing

The images are loaded with:

```python
keras.utils.image_dataset_from_directory()
```

The notebook uses:

- Image size: `256 × 256`
- Batch size: `32`
- Label mode: `int`

The images are normalized using:

```python
image = tf.cast(image / 255., tf.float32)
```

This changes the pixel values from:

```text
0 - 255
```

to:

```text
0 - 1
```

## Model Compilation

The model is compiled using:

- **Optimizer:** Adam
- **Loss:** Binary Crossentropy
- **Metric:** Accuracy

The notebook uses:

```python
model.compile(
    metrics=['accuracy'],
    loss='binary_crossentropy',
    optimizer='adam'
)
```

## Model Training

The model is trained for:

```text
10 epochs
```

Training uses the training dataset and evaluates the model on the test dataset after each epoch.

```python
history = model.fit(
    train_ds,
    epochs=10,
    validation_data=test_ds
)
```

## Training Graphs

The notebook creates two graphs.

### Accuracy

The accuracy graph compares:

- Training accuracy
- Validation accuracy

This helps you see how the model's classification performance changes during training.

### Loss

The loss graph compares:

- Training loss
- Validation loss

This helps you understand how the model's error changes during training.

## How to Run the Notebook

The notebook is mainly designed for **Google Colab** because it uses `/content/` paths.

### Step 1: Open the Notebook

Open:

```text
Cat_v_s_Dog_Classifier_CNN.ipynb
```

in Google Colab.

### Step 2: Install the Required Libraries

If you are running the project in a normal Python environment, run:

```bash
pip install -r requirements.txt
```

The notebook also uses the Kaggle package to download the dataset.

### Step 3: Set Up Kaggle

You need a Kaggle account and Kaggle API credentials.

Create/download your Kaggle API token (`kaggle.json`) from your Kaggle account.

Then configure it according to the Kaggle documentation or your environment.

**Important:** Do not upload your personal Kaggle API key or password to GitHub.

The original notebook contains Kaggle credential setup code. Before uploading the notebook publicly, remove any real API username/key from the notebook and replace it with a secure method of authentication.

If an API key has already been exposed publicly, regenerate/revoke it from Kaggle.

### Step 4: Download the Dataset

Run the Kaggle download cell:

```bash
kaggle datasets download -d princelv84/dogsvscats
```

Then extract the ZIP file.

The notebook expects the extracted data under:

```text
/content/train
/content/test
```

### Step 5: Load the Dataset

Run the cells that create:

```python
train_ds
test_ds
```

The images will be resized to:

```text
256 × 256
```

and loaded in batches of:

```text
32
```

### Step 6: Normalize the Images

Run the preprocessing cells.

The pixel values will be converted from `0-255` to `0-1`.

### Step 7: Build the CNN

Run the model-building cells.

The CNN will create the convolution, pooling, Dense and Dropout layers described above.

### Step 8: Train the Model

Run:

```python
history = model.fit(
    train_ds,
    epochs=10,
    validation_data=test_ds
)
```

The model will train for 10 epochs.

### Step 9: Check the Graphs

Run the graph cells to see:

- Training vs validation accuracy
- Training vs validation loss

### Step 10: Test Individual Images

The notebook uses OpenCV to load individual images.

For example:

```python
image_test = cv2.imread(...)
```

The image is then resized to:

```text
256 × 256
```

and reshaped into:

```text
(1, 256, 256, 3)
```

The model then makes a prediction using:

```python
model.predict(image_test_reshape)
```

## Important Note About Predictions

The notebook uses:

```text
cat = 1
dog = 0
```

The sigmoid output is a value between `0` and `1`.

A value closer to:

```text
1 → Cat
0 → Dog
```

The notebook tests sample images from the test dataset using fixed paths.

If these files are not available in your environment, change the image paths to images that exist in your downloaded dataset.

## Running Locally

The notebook currently uses Google Colab paths such as:

```text
/content/train
/content/test
```

If you run it on your computer, replace these paths with the actual location of your dataset.

You will also need to configure Kaggle authentication locally.

## Troubleshooting

### `ModuleNotFoundError`

If you get an error such as:

```text
ModuleNotFoundError: No module named 'tensorflow'
```

run:

```bash
pip install -r requirements.txt
```

### Kaggle authentication error

Make sure your Kaggle API credentials are configured correctly.

Do not put your real API key directly into a public GitHub repository.

### Dataset path not found

Check that the dataset has been downloaded and extracted correctly.

The notebook expects:

```text
/content/train
/content/test
```

### Test image not found

The notebook contains fixed paths for sample cat and dog images.

If those files are not available, update the paths to images that exist in your dataset.

## Possible Future Improvements

Some improvements that can be added to this project are:

- Data augmentation
- Early stopping
- Model checkpointing
- Transfer learning using MobileNetV2, VGG16 or ResNet
- Confusion matrix
- Classification report
- Prediction confidence
- Streamlit web interface
- Saving and loading the trained model
- Deployment as a web application

## Author

**anu-006**

GitHub: https://github.com/anu-006

# Dog-Cat-Breed-Classification
Jupyter notebooks creating Keras models to classify dog &amp; cat breeds based on limited dataset, with and without transfer learning

Referenced dataset: https://www.kaggle.com/datasets/zippyz/cats-and-dogs-breeds-classification-oxford-dataset

### About the dataset

[The dataset](https://www.kaggle.com/datasets/zippyz/cats-and-dogs-breeds-classification-oxford-dataset) contained labeled images of 37 breeds of cats and dogs (around 7.4k images in total). Considering the number of classes this means the training data consisted of around 150-200 images per class, which is a very small number (for reference, [this blog post](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html) on the Keras blog used 1k images per class for binary classification as a challenge for working on little data. Some images had incorrect extensions causing issues with opening them in OpenCV further reducing the training dataset.

### About the method

Classes were retrieved from the filenames and turned into target variables using one-hot encoding.
Before training, data augmentation (through scaling, rotation, shearing and brightness adjustments) was performed to combat overfitting (alongside utilizing dropout layers inside the networks themselves).

I created two models:

* One convolutional neural network built and trained from scratch. As expected from the low amount of data the validation accuracy was far from ideal - 20%. Much better than random guessing but far from useful. This is located in the "initial-model" Jupyter notebook of this repo.

* One convolutional neural network utilizing transfer learning. It's based on Resnet50 as the base model, originally trained on the imagenet dataset and fune-tuned by me on the abovementioned dataset uploaded to Kaggle. The resulting validation accuracy was much higher (>80%). It is worth noting that the original imagenet dataset already contained a large number of dog and cats pictures, so it is not surprising. This is the "resnet-based-model" file of this repository.

* A third notebook (resnet_with_frozen_weights) is a variant of the one using Resnet50, but freezing the weights of the CNN part and only training the feed-forward head part. This reached around ~90% validation accuracy after 3 epochs of trianing and is the best model among the three. Configuring this model made me learn a lot about the preprocessing that is specific to each fune-tunable model, as simply freezing the weights on the original one lead to horrible (~2%) performance. Apparently with unfrozen weights the model can partially adapt to data that is not in its preferred format, but once the weights are frozen the input has to be exactly in the format it was trained for (BGR/RGB channel ordering and scaling by mean of the Imagenet dataset). Longer training lead to significant overfitting, discovered after testing on a custom verification dataset - likely caused by how small the train/test datasets were. 

### Tools used:

* Python (duh)

* Keras

* Numpy

* OpenCV (cv2)

* Matplotlib 

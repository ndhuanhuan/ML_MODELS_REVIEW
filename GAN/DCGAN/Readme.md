# Basics
## Convolution
- Parameter sharing

Importantly, filter parameters are shared by all the input values to the given filter. This has both intuitive and practical advantages. Intuitively, parameter sharing allows us to efficiently learn visual features and shapes (such as lines and edges) regardless of where they are located in the input image. From a practical perspective, parameter sharing drastically reduces the number of trainable parameters. This decreases the risk of overfitting and allows this technique to scale up to higher-resolution images without a corresponding exponential increase in trainable parameters, as would be the case with a traditional, fully connected network.

- BATCH NORMALIZATION
Normalization has several advantages. Perhaps most important, it makes comparisons between features with vastly different scales easier and, by extension, makes the training process less sensitive to the scale of the features.

## DCGAN architecture
- DCGAN generator
To generate an image by using the ConvNet architecture, we reverse the process: instead of taking an image and processing it into a vector, we take a vector and up-size it to an image

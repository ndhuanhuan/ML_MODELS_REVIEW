# Basics

## Intro
Semi-supervised learning is one of the most promising areas of practical application of GANs. Unlike supervised learning, in which we need a label for every example in our dataset, and unsupervised learning, in which no labels are used, semi-supervised learning has a class label for only a small subset of the training dataset. By internalizing hidden structures in the data, semi-supervised learning strives to generalize from the small subset of labeled data points to effectively classify new, previously unseen examples. Importantly, for semi-supervised learning to work, the labeled and unlabeled data must come from the same underlying distribution.

## What is Semi-Supervised GAN
Semi-Supervised GAN (SGAN) is a Generative Adversarial Network whose Discriminator is a multiclass classifier. Instead of distinguishing between only two classes (real and fake), it learns to distinguish between N + 1 classes, where N is the number of classes in the training dataset, with one added for the fake examples produced by the Generator.

For example, the MNIST dataset of handwritten digits has 10 labels (one label for each numeral, 0 to 9), so the SGAN Discriminator trained on this dataset would predict between 10 + 1 = 11 classes. In our implementation, the output of the SGAN Discriminator will be represented as a vector of 10 class probabilities (that sum up to 1.0) plus another probability that represents whether the image is real or fake.

## architecture
The SGAN Generator’s purpose is the same as in the original GAN: it takes in a vector of random numbers and produces fake examples whose goal is to be indistinguishable from the training dataset.

The SGAN Discriminator, however, diverges considerably from the original GAN implementation. Instead of two, it receives three kinds of inputs: fake examples produced by the Generator (x*), real examples without labels from the training dataset (x), and real examples with labels from the training dataset (x, y), where y denotes the label for the given example x. Instead of binary classification, the SGAN Discriminator’s goal is to correctly categorize the input example into its corresponding class if the example is real, or reject the example as fake (which can be thought of as a special additional class).

## Training process
Recall that in a regular GAN, we train the Discriminator by computing the loss for D(x) and D(x*) and backpropagating the total loss to update the Discriminator’s trainable parameters to minimize the loss. The Generator is trained by backpropagating the Discriminator’s loss for D(x*), seeking to maximize it, so that the fake examples it synthesizes are misclassified as real.

To train the SGAN, in addition to D(x) and D(x*), we also have to compute the loss for the supervised training examples: D((x, y)). These losses correspond to the dual learning objective that the SGAN Discriminator has to grapple with:
- distinguishing real examples from the fake ones
- learning to classify real examples to their correct classes

These dual objectives correspond to two kinds of losses: the supervised loss and the unsupervised loss.

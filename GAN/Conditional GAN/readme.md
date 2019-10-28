# CGAN

## Motivation
The CGAN is one of the first GAN innovations that made targeted data generation possible, and arguably the most influential one.

## Basics
During CGAN training, the Generator learns to produce realistic examples for each label in the training dataset, and the Discriminator learns to distinguish fake example-label pairs from real example-label pairs. In contrast to the Semi-Supervised GAN from the previous chapter, whose Discriminator learns to assign the correct label to each real example (in addition to distinguishing real examples from fake ones), the Discriminator in a CGAN does not learn to identify which class is which. It learns only to accept real, matching pairs while rejecting pairs that are mismatched and pairs in which the example is fake.

For example, the CGAN Discriminator should learn to reject the pair (<hand written 3 image>, 4), regardless of whether the example (handwritten numeral 3) is real or fake, because it does not match the label, 4. The CGAN Discriminator should also learn to reject all image-label pairs in which the image is fake, even if the label matches the image.

Accordingly, in order to fool the Discriminator, it is not enough for the CGAN Generator to produce realistic-looking data. The examples it generates also need to match their labels. After the Generator is fully trained, this then allows us to specify what example we want the CGAN to synthesize by passing it the desired label.

## CGAN Generator
let’s call the conditioning label y. The Generator uses the noise vector z and the label y to synthesize a fake example G(z, y) = x*|y (read as “x* given that, or conditioned on, y”). The goal of this fake example is to look (in the eyes of the Discriminator) as close as possible to a real example for the given label.

## CGAN Discriminator
The Discriminator receives real examples with labels (x, y), and fake examples with the label used to generate them, (x*|y, y). On the real example-label pairs, the Discriminator learns how to recognize real data and how to recognize matching pairs. On the Generator-produced examples, it learns to recognize fake image-label pairs, thereby learning to tell them apart from the real ones.

The Discriminator outputs a single probability indicating its conviction that the input is a real, matching pair. The Discriminator’s goal is to learn to reject all fake examples and all examples that fail to match their label, while accepting all real example-label pairs.

## Architecture
Note that the Discriminator is never explicitly trained to reject mismatched pairs by being trained on real examples with mismatching labels; its ability to identify mismatched pairs is a by-product of being trained to accept only real matching pairs.

### Generator
we use embedding and element-wise multiplication to combine the random noise vector z and the label y into a joint representation.
1. Take label y (an integer from 0 to 9) and turn it into a dense vector of size z_dim (the length of the random noise vector) by using the Keras Embedding layer.
2. Combine the label embedding with the noise vector z into a joint representation by using the Keras Multiply layer. As its name suggests, this layer multiplies the corresponding entries of the two equal-length vectors and outputs a single vector of the resulting products.
3. Feed the resulting vector as input into the rest of the CGAN Generator network to synthesize an image.

### Discriminator
unlike the Generator, where the model input is a flat vector, the Discriminator receives three-dimensional images. This necessitates customized handling, described in the following steps:
1. Take a label (an integer from 0 to 9) and—using the Keras Embedding layer—turn the label into a dense vector of size 28 × 28 × 1 = 784 (the length of a flattened image).
2. Reshape the label embeddings into the image dimensions (28 × 28 × 1).
3. Concatenate the reshaped label embedding onto the corresponding image, creating a joint representation with the shape (28 × 28 × 2). You can think of it as an image with its embedded label “stamped” on top of it.
4. Feed the image-label joint representation as input into the CGAN Discriminator network. Note that in order for things to work, we have to adjust the model input dimensions to (28 × 28 × 2) to reflect the new input shape.

### CGAN training algorithm
For each training iteration do:
1. Train the Discriminator:
- Take a random mini-batch of real examples and their labels (x, y).
- Compute D((x, y)) for the mini-batch and backpropagate the binary classification loss to update θ(D) to minimize the loss.
- Take a mini-batch of random noise vectors and class labels (z, y) and generate a mini-batch of fake examples: G(z, y) = x*|y.
- Compute D(x*|y, y) for the mini-batch and backpropagate the binary classification loss to update θ(D) to minimize the loss.
2. Train the Generator:
- Take a mini-batch of random noise vectors and class labels (z, y) and generate a mini-batch of fake examples: G(z, y) = x*|y.
- Compute D(x*|y, y) for the given mini-batch and backpropagate the binary classification loss to update θ(G) to maximize the loss.

End for

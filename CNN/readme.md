https://stackoverflow.com/questions/32294261/what-is-depth-of-a-convolutional-neural-network

# Basics about different kind of layers
https://adeshpande3.github.io/A-Beginner%27s-Guide-To-Understanding-Convolutional-Neural-Networks-Part-2/
http://cs231n.github.io/convolutional-networks/
## Filters
The depth of a kernel/filters is equal to the number of input maps(depth of imput) you use to compute the specific output map.

## stride
The amount by which the filter shifts is the "stride".

## padding
"padding": the cells add around input.
https://adeshpande3.github.io/assets/Pad.png

## calculate output size
O = (w-k+2p)/2 + 1
where O is the output height/length, W is the input height/length, K is the filter size, P is the padding, and S is the stride.

In the early layers of our network, we want to preserve as much information about the original input volume so that we can extract those low level features.  => add padding

## ReLU Layer
ReLU (Rectified Linear Units) Layers

After each conv layer, it is convention to apply a nonlinear layer (or activation layer) immediately afterward.The purpose of this layer is to introduce nonlinearity to a system that basically has just been computing linear operations during the conv layers (just element wise multiplications and summations).In the past, nonlinear functions like tanh and sigmoid were used, but researchers found out that ReLU layers work far better because the network is able to train a lot faster (because of the computational efficiency) without making a significant difference to the accuracy.

## Pooling Layers
https://leonardoaraujosantos.gitbooks.io/artificial-inteligence/content/pooling_layer.html
After some ReLU layers, programmers may choose to apply a pooling layer. It is also referred to as a downsampling layer.
The intuitive reasoning behind this layer is that once we know that a specific feature is in the original input volume (there will be a high activation value), its exact location is not as important as its relative location to the other features. As you can imagine, this layer drastically reduces the spatial dimension (the length and the width change but not the depth) of the input volume.
 
1. The first is that the amount of parameters or weights is reduced by 75%, thus lessening the computation cost. 
2. The second is that it will control overfitting. 

## Dropout Layers
This layer “drops out” a random set of activations in that layer by setting them to zero.

## Network in Network Layers

## Dilated Convolution
In simple terms, dilated convolution is just a convolution applied to input with defined gaps.
In short, dilated convolution is a simple but effective idea and you might consider it in two cases;
1. Detection of fine-details by processing inputs in higher resolutions.
2. Broader view of the input to capture more contextual information.
3. Faster run-time with less parameters
http://www.erogol.com/dilated-convolution/

## Batch normalization in Neural Networks
https://towardsdatascience.com/batch-normalization-in-neural-networks-1ac91516821c
We train our data on only black cats’ images. So, if we now try to apply this network to data with colored cats, it is obvious; we’re not going to do well. The training set and the prediction set are both cats’ images but they differ a little bit. In other words, if an algorithm learned some X to Y mapping, and if the distribution of X changes, then we might need to retrain the learning algorithm by trying to align the distribution of X with the distribution of Y.

batch normalization allows each layer of a network to learn by itself a little bit more independently of other layers.

## 1×1 Convolutions to Manage Model Complexity
https://machinelearningmastery.com/introduction-to-1x1-convolutions-to-reduce-the-complexity-of-convolutional-neural-networks/
- The 1×1 filter can be used to create a linear projection of a stack of feature maps.
- The projection created by a 1×1 can act like channel-wise pooling and be used for dimensionality reduction.
- The projection created by a 1×1 can also be used directly or be used to increase the number of feature maps in a model.

The 1×1 filter is so simple that it does not involve any neighboring pixels in the input; it may not be considered a convolutional operation. Instead, it is a linear weighting or projection of the input

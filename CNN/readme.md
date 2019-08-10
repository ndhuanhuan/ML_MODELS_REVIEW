https://stackoverflow.com/questions/32294261/what-is-depth-of-a-convolutional-neural-network

# Basics about different kind of layers
https://adeshpande3.github.io/A-Beginner%27s-Guide-To-Understanding-Convolutional-Neural-Networks-Part-2/
http://cs231n.github.io/convolutional-networks/

The amount by which the filter shifts is the "stride".
"padding": the cells add around input.
https://adeshpande3.github.io/assets/Pad.png

O = (w-k+2p)/2 + 1
where O is the output height/length, W is the input height/length, K is the filter size, P is the padding, and S is the stride.

In the early layers of our network, we want to preserve as much information about the original input volume so that we can extract those low level features.  => add padding


ReLU (Rectified Linear Units) Layers

After each conv layer, it is convention to apply a nonlinear layer (or activation layer) immediately afterward.The purpose of this layer is to introduce nonlinearity to a system that basically has just been computing linear operations during the conv layers (just element wise multiplications and summations).In the past, nonlinear functions like tanh and sigmoid were used, but researchers found out that ReLU layers work far better because the network is able to train a lot faster (because of the computational efficiency) without making a significant difference to the accuracy.

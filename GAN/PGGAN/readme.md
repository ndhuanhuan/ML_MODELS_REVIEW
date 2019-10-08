# Progressive GAN Basics
## Progressive growing and smoothing of higher-resolution layers
In technical terms, we are going from a few low-resolution convolutional layers to many high-resolution ones as we train. Thus, we first train the early layers and only then introduce a higher-resolution layer, where it is harder to navigate the loss space. We go from something simple—for example, 4 × 4 trained for several steps—to something more complex—for example, 1024 × 1024 trained for several epochs.

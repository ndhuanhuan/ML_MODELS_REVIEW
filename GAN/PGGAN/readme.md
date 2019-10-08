# Progressive GAN Basics
## Progressive growing and smoothing of higher-resolution layers
In technical terms, we are going from a few low-resolution convolutional layers to many high-resolution ones as we train. Thus, we first train the early layers and only then introduce a higher-resolution layer, where it is harder to navigate the loss space. We go from something simple—for example, 4 × 4 trained for several steps—to something more complex—for example, 1024 × 1024 trained for several epochs.

The problem in this scenario is that upon introducing even one more layer at a time (for example, from 4 × 4 to 8 × 8), we are still introducing a massive shock to the training. What the PGGAN authors do instead is smoothly fade in those layers, in order to give the system time to adapt to the higher resolution.

However, rather than immediately jumping to this resolution, we smoothly fade in this new layer with higher resolution by a parameter alpha (α), which is between 0 and 1. Alpha affects how much we use either the old—but upscaled—layer or the natively larger one. On the side of the D, we simply shrink by 0.5x to allow for smoothly injecting the trained layer for discrimination.

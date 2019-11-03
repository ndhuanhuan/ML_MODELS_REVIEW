
# Use Cases
## IMAGE-TO-IMAGE TRANSLATION

# Basics
## CYCLE-CONSISTENCY LOSS: THERE AND BACK AGAN
we simply complete the cycle: we translate from one domain to another and then back again. For example, we go from summer picture (domain A) of a park to a winter one (domain B) and then back again to summer (domain A). Now we have essentially created a cycle, and, ideally, the original picture (a) and the reconstructed picture (a^) are the same. If they are not, we can measure their loss on a pixel level, thereby getting the first loss of our CycleGAN: cycle-consistency loss.

A common analogy is thinking about the process of back-translation—a sentence in Chinese that is translated to English and then back again to Chinese should give back the same sentence. If not, we can measure the cycle-consistency loss by how much the first and the third sentences differ.

To be able to use the cycle-consistency loss, we need to have two Generators: one translating from A to B, called GAB, sometimes referred to as simply G, and then another one translating from B to A, called GBA, referred to as F for brevity. There are technically two losses—forward cycle-consistency loss and backward cycle-consistency loss—but because all they mean is that  as well as , you may think of these as essentially the same, but off by one.

## ADVERSARIAL LOSS
In addition to the cycle-consistency loss, we still have the adversarial loss. Every translation with a Generator GAB has a corresponding Discriminator DB, and GBA has Discriminator DA. The way to think about it is that we are always testing, when translating to domain A, whether the picture looks real; hence we use DA and vice versa.

This is the same idea as with simpler architectures, but now, because of the two losses, we have two Discriminators. We need to make sure that not only the translation from apple to orange looks real, but also that the translation from our estimated orange back to reconstructed apple looks real. Recall that the adversarial loss ensures that the images look real, and as a result, it is still key for the CycleGAN to work. Hence adversarial loss is presented as second. The first Discriminator in the cycle is especially important—otherwise, we’d simply get noise that would help the GAN memorize what it should reconstruct.

## Identity Loss
The idea of identity loss is simple: we want to enforce that CycleGAN preserves the overall color structure (or temperature) of the picture. So we introduce a regularization term that helps us keep the tint of the picture consistent with the original image. Imagine this as a way of ensuring that even after applying many filters onto your image, you still can recover the original image.

This is done by feeding the images already in domain A to the Generator from B to A (GBA), because the CycleGAN should understand that they are already in the correct domain. In other words, we penalize unnecessary changes to the image: if we feed in a zebra and are trying to “zebrafy” an image, we get the same zebra back, as there is nothing to do.

# Architecture
## Generator Architecture
Use U-Net architecture
- Encoder— Step 1 from figure 9.4: these are the convolutional layers that reduce the resolution of each feature map (layer or slice). This is the contraction path (d0 to d3).
- Decoder— Step 3 from figure 9.4: these are the deconvolutional layers (transposed convolutions) that upscale the image back to 128 × 128. This is the expansion path (u1 to u4).

To clarify, the autoencoder model here is useful in two ways. First, the overall CycleGAN architecture can be viewed as training two autoencoders. Second, the U-Net itself has parts referred to as encoder and decoder.

## Discriminator architecture

The CycleGAN’s Discriminator is based on the PatchGAN architecture—we will dive into the technical details in the code section. One thing that may be confusing is that we do not get a single float as an output of this Discriminator, but rather a set of single-channel values that may be thought of as a set of mini-discriminators that we then average together.

Ultimately, this allows the design of the CycleGAN to be fully convolutional, meaning that it can scale relatively easily to higher resolutions. Indeed, in the examples of translating video games to reality or vice versa, the CycleGAN authors have used an upscaled version of the CycleGAN, with only minor modifications thanks to the fully convolutional design. Other than that, the Discriminator should be a relatively straightforward implementation of the Discriminators you have seen before, except there are now two of them.

## Instance normalization
Instance normalization is similar to the batch normalization in chapter 4, except that instead of normalizing based on information from the entire batch, we normalize each feature map within each channel separately. Instance normalization often results in better-quality images for tasks such as style transfer or image-to-image translation—just what we need for the CycleGAN!

## Training CycleGAN

For each training iteration do
1. Train the Discriminator:
- Take a mini-batch of random images from each domain (imgsA and imgsB).
- Use the Generator GAB to translate imgsA to domain B and vice versa with GBA.
- Compute DA(imgsA, 1) and DA(GBA(imgsB), 0) to get the losses for real images in A and translated images from B, respectively. Then add these two losses together. The 1 and 0 in DA serve as labels.
- Compute DB(imgsB, 1) and DB(GAB(imgsA), 0) to get the losses for real images in B and translated images from A, respectively. Then add these two losses together. The 1 and 0 in DB serve as labels.
2. Add the losses from steps c and d together to get a total Discriminator loss. Train the Generator:
- We use the combined model to
  - Input the images from domain A (imgsA) and B (imgsB)
  - The outputs are
    - Validity of A: DA(GBA(imgsB))
    - Validity of B: DB(GAB(imgsA))
    - Reconstructed A: GBA(GAB(imgsA))
    - Reconstructed B: GAB(GBA(imgsB))
    - Identity mapping of A: GBA(imgsA))
    - Identity mapping of B: GAB(imgsB))

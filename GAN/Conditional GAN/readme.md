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

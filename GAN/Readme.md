



# Q & A

###
Why use Binary Cross Entropy for Generator in Adversarial Networks
https://stats.stackexchange.com/questions/242907/why-use-binary-cross-entropy-for-generator-in-adversarial-networks


# Readings
## Kaggle
https://www.kaggle.com/c/generative-dog-images/notebooks

## DCGAN
https://towardsdatascience.com/deeper-into-dcgans-2556dbd0baac
https://machinelearningmastery.com/how-to-code-generative-adversarial-network-hacks/


## Gans Basics
In a GAN, each network can tune only its own weights and biases. The Generator can tune only θ(G), and the Discriminator can tune only θ(D) during training. Accordingly, each network has control over only a part of what determines its loss.

Because the Generator and Discriminator can tune only their own parameters and not each other’s, GAN training can be better described as a game, rather than optimization.[3] The players in this game are the two networks that the GAN comprises.

Recall that GAN training ends when the two networks reach Nash equilibrium, a point in a game at which neither player can improve their situation by changing their strategy. Mathematically, this occurs when the Generator cost J(G)(θ(G), θ(D)) is minimized with respect to the Generator’s trainable parameters θ(G) and, simultaneously, the Discriminator cost J(D)(θ(G), θ(D)) is minimized with respect to the parameters under this network’s control, θ(D)

Using the confusion matrix terminology, the Discriminator is trying to maximize true positive and true negative.

The Generator is not concerned with how well the Discriminator classifies the real examples; it cares only about the Discriminator’s classifications of the fake data samples.

For each training iteration do
  1. Train the Discriminator:
     (1)Take a random mini-batch of real examples: x.
     (2)Take a mini-batch of random noise vectors z and generate a mini-batch of fake examples: G(z) = x*.
     (3)Compute the classification losses for D(x) and D(x*), and backpropagate the total error to update θ(D) to minimize the classification loss.
  2. Train the Generator:
    (1)Take a mini-batch of random noise vectors z and generate a mini-batch of fake examples: G(z) = x*.
    (2)Compute the classification loss for D(x*), and backpropagate the loss to update θ(G) to maximize the classification loss.
End for

```
GANs consist of two networks: the Generator (G) and the Discriminator (D), each with its own loss function: J(G)(θ(G), θ(D)) and J(D)(θ(G), θ(D)), respectively.

During training, the Generator and the Discriminator can tune only their own parameters: θ(G) and θ(D), respectively.

The two GAN networks are trained simultaneously via a game-like dynamic. The Generator seeks to maximize the Discriminator’s false-positive classifications (classifying a generated image as real), while the Discriminator seeks to minimize its false-positive and false-negative classifications
```

## Training and common challenges: GANing for success

- All of these generative models ultimately derive from Maximum Likelihood, at least implicitly
- The variational autoencoder introducedsits in the Explicit part of the tree. Remember that we had a clear loss function (the reconstruction loss)? Well, with GANs we do not have it anymore. Rather, we now have two competing loss functions that we will cover in lot more depth later. But as such, the system does not have a single analytical solution.

### Evaluation
Two most commonly used and accepted metrics for statistically evaluating the quality of the generated samples: the inception score (IS) and Fréchet inception distance (FID).

The advantage of those two metrics is that they have been extensively validated to be highly correlated with at least some desirable property such as visual appeal or realism of the image. The inception score was designed solely around the idea that the samples should be recognizable, but it has also been shown to correlate with human intuition about what constitutes a real image.

#### Evaluation - Inception score
1. We take the Kullback–Leibler (KL) divergence between the real and the generated distribution
2. We exponentiate the result of step 1.

### Evaluation - Fréchet inception distance
In 2017, a new solution was proposed: the Fréchet inception distance (FID). The FID improves on the IS by making it more robust to noise and allowing the detection of intraclass (within class) sample omissions.

This is important, because if we accept the IS baseline, then producing only one type of a category technically satisfies the category-being-generated-sometimes requirement. But, for example, if we are trying to create a cat-generation algorithm, this is not actually what we want (say, if we had multiple breeds of cats represented). Furthermore, we want the GAN to output samples that present a cat from more than one angle and, generally, images that are distinct.

Technical implementation of the FID is again complex, but the high-level idea is that we are looking for a generated distribution of samples that minimizes the number of modifications we have to make to ensure that the generated distribution looks like the distribution of the true data.

The FID is calculated by running images through an Inception network. In practice, we compare the intermediate representations—feature maps or layers—rather than the final output (in other words, we embed them). More concretely, we evaluate the distance of the embedded means, the variances, and the covariances of the two distributions—the real and the generated one.

## Training GAN - Chanllenges

### Mode collapse
In mode collapse, some of the modes (for example, classes) are not well represented in the generated samples. The mode collapses even though the real data distribution has support for the samples in this part of the distribution; for example, there will be no number 8 in the MNIST dataset. Note that mode collapse can happen even if the network has converged. We talked about interclass mode collapse during the explanation of the IS and intraclass mode collapse when discussing the FID.

### Slow convergence
### Overgeneralization
Here, we talk especially about cases in which modes (potential data samples) that should not have support (should not exist), do. For example, you might see a cow with multiple bodies but only one head, or vice versa. This happens when the GAN overgeneralizes and learns things that should not exist based on the real data.

## How to resolve training challenges
- Adding network depth
- Changing the game setup
1. Min-Max design and stopping criteria that were proposed by the original paper
2. Non-Saturating design and stopping criteria that were proposed by the original paper
3. Wasserstein GAN as a recent improvement
- Number of training hacks with commentary
1. Normalizing the inputs
2. Penalizing the gradients
3. Training the Discriminator more
4. Avoiding sparse gradients
5. Changing to soft and noisy labels

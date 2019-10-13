# Gan Repo
The GAN Zoo: https://github.com/hindupuravinash/the-gan-zoo



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

### Adding network depth
See “Progressive Growing of GANs for Improved Quality, Stability, and Variation,” by Tero Karras et al., 2017, http://arxiv.org/abs/1710.10196.

### Min-Max GAN
We have two loss functions, and one is the negative value of the other.
For those of you who want more detail, KL divergence, as well as JSD, are generally regarded as what GANs are ultimately trying to minimize. These are both types of distance metrics that help us understand how different the two distributions are in a high-dimensional space. Some neat proofs connect those divergences and the min-max version of the GAN.

Jensen-Shannon divergence (JSD) is a symmetric version of KL divergence. Whereas KL(p,q)! = KL(q,p), it is the case that JSD(p,q) == JSD(q,p).

### Non-Saturating GAN
In this version of the problem, rather than trying to put the two loss functions as direct competitors of each other, we make the two loss functions independent, but directionally consistent with the original formulation.

Again, let’s focus on a general understanding: the two loss functions are no longer set directly against each other. But in equation  you can see that the Generator is trying to minimize the opposite of the second term of the Discriminator. Basically, it is trying not to get caught for the samples that it generates.

The main reason for the NS-GAN is that in the MM-GAN’s case, the gradients can easily saturate—get close to 0, which leads to slow convergence, because the weight updates that are backpropagated are either 0 or tiny

#### When to stop training
NS-GAN
- Is no longer asymptotically consistent with the JSD
- Has an equilibrium state that theoretically is even more elusive
In the NS-GAN’s defense, it should be said that it is still much faster than the Wasserstein GAN.

### Wasserstein GAN
The WGAN is important for three reasons:
- It significantly improves on the loss functions, which are now interpretable and provide clearer stopping criteria.
- Empirically, the WGAN tends to have better results.
- Unlike a lot of research into GANs, it has clear theoretical backing that starts from the loss and shows how the KL divergence that we are trying to approximate is ultimately not well justified theoretically or practically. Based on this theory, it then proposes a better loss function that mitigates this problem.

For first point, the WGAN uses the earth mover’s distance as a loss function that clearly correlates with the visual quality of the samples generated.
The benefits of the second and third points are somewhat obvious—we want to have higher-quality samples and better theoretical grounding.

On a high level, in this equation we are trying to minimize the distance between the expectation of the real distribution and the expectation of the generated distribution. The paper that introduced the WGAN itself is complex, but the gist is that fw is a function satisfying a technical constraint.
The technical constraint that fw satisfies is 1 – Lipschitz: for all x1, x2: | f(x1) – f(x2) | ≤ | x1 – x2 |.

The problem that the Generator is trying to solve is similar to the one before, but let’s go into more detail anyway:

1. We draw x from either the real distribution (x ~ Pr) or the generated distribution x* (gθ(z), where z ~ p(z)).
2. The generated samples are sampled from z (the latent space) and then transformed via gθ to get the samples (x*) in the same space and then evaluated using fw.
3. We are trying to minimize our loss function—or distance function, in this case—the earth mover’s distance. The actual numbers are calculated using the earth mover’s distance.

The WGAN has two practical implications:

1. We now have clearer stopping criteria because this GAN has been validated by later papers that show a correlation between the Discriminator loss and the perceptual quality. We can simply measure the Wasserstein distance, and that helps inform when to stop.
2. We can now train the WGAN to convergence. This is relevant because meta-review papers[15] showed that using the JS loss and the divergence between the Generator in the real distribution as a measure of training progress can often be meaningless.[16] To translate that into human terms, sometimes in chess, you need to lose a couple of rounds and therefore temporarily do worse in order to learn in a couple of iterations and ultimately do better.

earth mover’s distance == Wasserstein distance
In the end, all you need to know is that the earth mover’s distance has nicer properties than either the JS or KL, and there are already important contributions building on the WGAN as well as validating its generally superior performance.

## Summary of Game Setups
Three core versions of the GAN setup: min-max, non-saturating, and Wasserstein.
One of these versions will be mentioned at the beginning of every paper, and now you’ll have at least an idea of whether the paper is using the original formulation, which is more explainable but doesn’t work as well in practice; or the non-saturating version, which loses a lot of the mathematical guarantees but works much better; or the newer Wasserstein version, which has both theoretical grounding and largely superior performance.
http://mng.bz/Xgv6

Training hacks that allow us to train faster include the following:
- Normalizing inputs, which is standard in machine learning
- Using gradient penalties that give us more stability in training
- Helping to warm-start the Discriminator to ultimately give us a good Generator, because doing so sets a higher bar for the generated samples
- Avoiding sparse gradients, because they lose too much information
- Playing around with soft and noisy labels rather than the typical binary classification





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
    1. Take a random mini-batch of real examples: x.
    2. Take a mini-batch of random noise vectors z and generate a mini-batch of fake examples: G(z) = x*.
    3. Compute the classification losses for D(x) and D(x*), and backpropagate the total error to update θ(D) to minimize the classification loss.
  2. Train the Generator:
    1. Take a mini-batch of random noise vectors z and generate a mini-batch of fake examples: G(z) = x*.
    2. Compute the classification loss for D(x*), and backpropagate the loss to update θ(G) to maximize the classification loss.
End for

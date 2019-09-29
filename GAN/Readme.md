



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

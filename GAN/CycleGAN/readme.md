
# Use Cases
## IMAGE-TO-IMAGE TRANSLATION

# Basics
## CYCLE-CONSISTENCY LOSS: THERE AND BACK AGAN
we simply complete the cycle: we translate from one domain to another and then back again. For example, we go from summer picture (domain A) of a park to a winter one (domain B) and then back again to summer (domain A). Now we have essentially created a cycle, and, ideally, the original picture (a) and the reconstructed picture (a^) are the same. If they are not, we can measure their loss on a pixel level, thereby getting the first loss of our CycleGAN: cycle-consistency loss.

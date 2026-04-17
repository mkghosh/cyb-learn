# Building models

## Epochs and Overfitting

An **epoch** is one complete pass of the training algorithm through the entire dataset. In practice, models are trained over many epochs. The algorithm repeatedly sees the same data, adjusting its parameters each time until it converges on accurate predictions.

The catch is that more epochs don't always mean a better model. Train for too long and the model stops learning general patterns and starts memorising specifically, a problem called overfitting. An overfit model performs well on its but poorly on other data. This matters for security because overfitting is one mechanism by which a model can "memorise" specific details from its , including sensitive ones, making it more likely to reproduce them when prompted.

### A LEARNED REPRESENTATION FOR ARTISTIC STYLE 
**Authors:**   
Dumoulin, Vincent   
Shlens, Jonathon    
Kudlur, Manjunath   

**Year**: 2017  
  
**Abstract:**  
The diversity of painting styles represents a rich visual vocabulary for the construction of an image. The degree to which one may learn and parsimoniously capture this visual vocabulary measures our understanding of the higher level features of paintings, if not images in general. In this work we investigate the construction of a single, scalable deep network that can parsimoniously capture the artistic style of a diversity of paintings. We demonstrate that such a network generalizes across a diversity of artistic styles by reducing a painting to a point in an embedding space. Importantly, this model permits a user to explore new painting styles by arbitrarily combining the styles learned from individual paintings. We hope that this work provides a useful step towards building rich models of paintings and offers a window on to the structure of the learned representation of artistic style.  

**Link:** http://arxiv.org/abs/1610.07629   

### Notes
1. **conditional instance normalization**   
In this work, we show that a simple modification of the style transfer network, namely the introduction of conditional instance normalization, allows it to learn multiple styles. Our work stems from the intuition that many styles probably share some degree of computation, and that this sharing is thrown away by training N networks from scratch when building an N- styles style transfer system.   
To take this into account, we propose to train a single conditional style transfer network T(c, s) for N styles. We found a very surprising fact about the role of normalization in style transfer networks: to model a style, it is sufficient to specialize scaling and shifting parameters after normalization to each specific style. In other words, all convolutional weights of a style transfer network can be shared across many styles, and it is sufficient to tune parameters for an affine transformation after normalization for each style.    
We call this approach conditional instance normalization. The goal of the procedure is transform a layer’s activations x into a normalized activation z specific to painting style s. Building off the instance normalization technique proposed in Ulyanov et al. (2016b), we augment the γ and β parameters so that they’re N × C matrices, where N is the number of styles being modeled and C is the number of output feature maps.   

2. **add new styles**   
Since all weights in the transformer network are shared between styles, one way to incorporate a new style to a trained network is to keep the trained weights fixed and learn a new set of γ and β parameters. 

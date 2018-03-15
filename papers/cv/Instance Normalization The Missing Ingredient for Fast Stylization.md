### Instance Normalization: The Missing Ingredient for Fast Stylization Dmitry
**Authors:**  
Ulyanov, Dmitry  
Vedaldi, Andrea  
Lempitsky, Victor  

**Year**: 2017  
 
**Abstract:**  
In this paper we revisit the fast stylization method introduced in Ulyanov et al. (2016). We show how a small change in the stylization architecture results in a significant qualitative improvement in the generated images. The change is limited to swapping batch normalization with instance normalization, and to apply the latter both at training and testing times. The resulting method can be used to train high-performance architectures for real-time image generation.   

**Link:** http://arxiv.org/abs/1607.08022   

### Notes
1. **contrast normalization**  
A simple observation is that the result of stylization should not, in general, depend on the contrast of the content image. In fact, the style loss is designed to transfer elements from a style image to the content image such that the contrast of the stylized image is similar to the contrast of the style image. Thus, the generator network should discard contrast information in the content image.  

2. **adjust batch normalization**  
We replace batch normalization with instance normalization everywhere in the generator network g. This prevents instance-specific mean and covariance shift simplifying the learning process. Differently from batch normalization, furthermore, the instance normalization layer is applied at test time as well.  

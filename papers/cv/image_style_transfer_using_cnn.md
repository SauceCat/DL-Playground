### Image Style Transfer Using Convolutional Neural Networks
**Authors:**  
Gatys, Leon A  
Ecker, Alexander S  
Bethge, Matthias   

**Year**: 2016  
  
**Abstract:**  
Rendering the semantic content of an image in different
styles is a difficult image processing task. Arguably, a major limiting factor for previous approaches has been the lack of image representations that explicitly represent semantic information and, thus, allow to separate image content from style. Here we use image representations derived from Convolutional Neural Networks optimised for object recognition, which make high level image information explicit. We introduce A Neural Algorithm of Artistic Style that can separate and recombine the image content and style of natural images. The algorithm allows us to produce new images of high perceptual quality that combine the content of an arbitrary photograph with the appearance of numerous well-known artworks. Our results provide new insights into the deep image representations learned by Convolutional Neural Networks and demonstrate their potential for high level image synthesis and manipulation.
  
**Link:** https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf


### Notes
1. **ReLU Nonlinearity**: Deep convolutional neural networks with ReLUs train several times faster than their equivalents with tanh units.
2. **Local Response Normalization**: Implements a form of lateral inhibition inspired by the type found in real neurons, creating competition for big activities amongst neuron outputs computed using different kernels.
3. **Data Augmentation**: Enlarge the dataset using label-preserving transformations. Allow transformed images to be produced from the original images with very little computation, so the transformed images do not need to be stored on disk. Extracting random 224×224 patches (and their horizontal reflections) from the 256×256 images. At test time, the network makes a prediction by extracting
five 224 × 224 patches (the four corner patches and the center patch) as well as their horizontal reflections (hence ten patches in all), and averaging the predictions made by the network’s softmax layer on the ten patches. The second form of data augmentation consists of altering the intensities of the RGB channels in training images.
4. **Dropout**: Consists of setting to zero the output of each hidden neuron with probability 0.5. The neurons which are “dropped out” in this way do not contribute to the forward pass and do not participate in back- propagation. So every time an input is presented, the neural network samples a different architecture, but all these architectures share weights. At test time, we use all the neurons but multiply their outputs by 0.5, which is a reasonable approximation to taking the geometric mean of the predictive distributions produced by the exponentially-many dropout networks.
5. **Unsupervised pre-training**: might be helpful.

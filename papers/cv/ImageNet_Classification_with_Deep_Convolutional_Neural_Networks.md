
### ImageNet Classification with Deep Convolutional Neural Networks
**Authors:**  
Krizhevsky, Alex  Sutskever, Ilya  Hinton, Geoffrey E  

**Year**: 2012  
  
**Abstract:**  
We trained a large, deep convolutional neural network to classify the 1.2 million high-resolution images in the ImageNet LSRVRC-2010 contest into the 1000 different classes. On the test data, we achieved top-1 and top-5 error rates of 37.5% and 17.0% which is considerably better than the previous state of the art. The neural network, which has 60 million paramters and 650,000 neurons, consists of five convolutional layers, some of which are followed by max-pooling layers, and three fully connected layers with a final 1000-way softmax. To make training faster, we used non-saturating neurons and a very efficient GPU implementation of the convolutional operation. To reduce overfitting in the fully-connected layers, we employed a recently-developed method called 'dropout' that proved to be effective. We also entered a variant of the model in the ILSVRC-2012 competition and achievd a top-5 test error rate of 15.3%, compared to 26.2% achieved by the second-best entry.  
  
**Link:** https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf  


### Notes
1. **ReLU Nonlinearity**: Deep convolutional neural networks with ReLUs train several times faster than their equivalents with tanh units.
2. **Local Response Normalization**: Implements a form of lateral inhibition inspired by the type found in real neurons, creating competition for big activities amongst neuron outputs computed using different kernels.
3. **Data Augmentation**: Enlarge the dataset using label-preserving transformations. Allow transformed images to be produced from the original images with very little computation, so the transformed images do not need to be stored on disk. Extracting random 224×224 patches (and their horizontal reflections) from the 256×256 images. At test time, the network makes a prediction by extracting
five 224 × 224 patches (the four corner patches and the center patch) as well as their horizontal reflections (hence ten patches in all), and averaging the predictions made by the network’s softmax layer on the ten patches. The second form of data augmentation consists of altering the intensities of the RGB channels in training images.
4. **Dropout**: Consists of setting to zero the output of each hidden neuron with probability 0.5. The neurons which are “dropped out” in this way do not contribute to the forward pass and do not participate in back- propagation. So every time an input is presented, the neural network samples a different architecture, but all these architectures share weights. At test time, we use all the neurons but multiply their outputs by 0.5, which is a reasonable approximation to taking the geometric mean of the predictive distributions produced by the exponentially-many dropout networks.
5. **Unsupervised pre-training**: might be helpful.


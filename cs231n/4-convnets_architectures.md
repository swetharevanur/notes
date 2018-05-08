# Convolutional Neural Networks

## Overview
- Regular NNs don't scale well to images. Full connectivity is wasteful and the huge number of parameters can easily lead to overfitting.
- CNNs assume that inputs are images, and so can exploit 3D volumes for neurons.

## Stacking Layers to Build CNNs
- **INPUT [32x32x3]** will hold the raw pixel values of the image, in this case an image of width 32, height 32, and with three color channels R, G, and B.
- **CONV** layer will compute the output of neurons that are connected to local regions in the input, each computing a dot product between their weights and a small region they are connected to in the input volume. This may result in volume such as [32x32x12] if we decided to use 12 filters.
- **RELU** layer will apply an element-wise activation function, such as the `max(0,x)` thresholding at zero. This leaves the size of the volume unchanged ([32x32x12] in this case).
- **POOL2** layer will perform a downsampling operation along the spatial dimensions (width, height), resulting in volume such as [16x16x12].
	- Produces a volume of size $W_2\times H_2\times D_2$ where $W_2=(W_1−F)/S+1$, $H_2=(H_1−F)/S+1$, and $D_2=D_1$.
	- Not common to use padding with POOL.
- **FC** (i.e. fully-connected) layer will compute the class scores, resulting in volume of size [1x1x10], where each of the 10 numbers correspond to a class score, such as among the 10 categories of CIFAR-10. As with ordinary Neural Networks and as the name implies, each neuron in this layer will be connected to all the numbers in the previous volume.


- CONV and FC layers have parameters, while RELU and POOL don't. 

## Specifics about the CONV Layer
- The CONV layer’s parameters consist of a set of learnable filters. **Every filter is small spatially (along width and height), but extends through the full depth of the input volume.**
- As we slide the filter over the width and height of the input volume during a forward pass, we produce a 2-dimensional **activation map that gives the responses of that filter at every spatial position**.
- Network will learn filters that activate when they see some type of visual feature such as an edge of some orientation or a blotch of some color on the first layer.
- Stack these activation maps along the depth dimension and produce the output volume.
- Local connectivity is always determined by size of the **receptive field**. The connections are local in space (along width and height), but always full along the entire depth of the input volume. Every neuron has $N \times N \times D$ connections to the input layer.
- The backward pass for a convolution operation (for both the data and the weights) is also a convolution (but with spatially-flipped filters).

### Hyperparameters
1. Depth of the output volume = **number of filters** we want to use.
2. **Stride** = how many pixels we shift the filter during convolution
	- Higher stride produces smaller output volumes spatially.
	- Must fit neatly across the volume, spatially.
3. **Zero-padding** = allows us to control spatial size of output volumes
	- If $S = 1$, then you can preserve spatial dimensions by letting $P = (F-1)/2$.

>Given input volume size ($W$), the receptive field size of filter ($F$), the stride ($S$), and padding ($P$), the output size of CONV layer is $$N = \frac{W−F+2P}{S}+1$$

### Parameter Sharing
- Assumption: if one feature is useful to compute at some spatial position $(x, y)$, then it should also be useful to compute at a different position $(x_2, y_2)$. Reasonable thanks to translationally-invariant structure of images.
- Denoting a single 2D slice of depth as a **depth slice** (e.g. a volume of size [55x55x96] has 96 depth slices, each of size [55x55]), we are going to constrain the neurons in each depth slice to use the same weights and bias.
- Forward pass of the CONV layer in each depth slice is a convolution of the neuron’s weights with the input volume.

> Let the number of filters be $K$. Parameter sharing introduces $F \times F \times D_1 \times K + K$ weights and biases across all filters.

- But we don't always want to share parameters, and when we don't, it's called a **locally-connected layer**.

### 1x1 Convolutions
- Simply pointwise scaling.
- For example, if the input is [32x32x3] then doing 1x1 convolutions would effectively be doing 3-dimensional dot products (since the input depth is 3 channels).

### Dilation
- When filters have spaces between each cell, called dilation.
- Useful in some settings to use in conjunction with 0-dilated filters because it allows you to merge spatial information across the inputs much more agressively with fewer layers.
- Increases effective receptive field quickly.

## Converting FC to CONV
- It is worth noting that the only difference between FC and CONV layers is that the neurons in the CONV layer are connected only to a local region in the input, and that many of the neurons in a CONV volume share parameters. However, the neurons in both layers still compute dot products, so their functional form is identical and you can convert between them
- The converted FC weight matrix would be a large matrix that is mostly zero except for at certain blocks (due to local connectivity) where the weights in many of the blocks are equal (due to parameter sharing).


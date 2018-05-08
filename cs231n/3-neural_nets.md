# Neural Networks ([Lecture 6](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture06.pdf) and [Lecture 7](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture07.pdf))

## Overview
- Linear classifiers (single-layer NN): $f = Wx$.
- Two-layer NN: $f = W_2max(0, W_1x)$.
- Three-layer NN: $f = W_3max(0, W_2max(0, W_1x))$.
- `max` above is an example non-linearity.
- The forward pass of a fully-connected layer corresponds to one matrix multiplication followed by a bias offset and an activation function.
- Final layer of NN doesn't usually have an activation function.

## Activation Functions (Non-Linearities)
- Sigmoid
	- Range: (0, 1)
	- <span style="color:red">Saturation kills gradients due to poor learning rate or weight initialization.</span>
	- <span style="color:red">Not zero-centered.</span>
- tanh
	- Range: (-1, 1)
	- <span style="color:red">Saturation kills gradients.</span>
- ReLU: 
	- `max(0, x)`
	- <span style="color:green">Faster convergence compared to sigmoid/tanh</span>
	- <span style="color:red">Dying ReLU</span> fixed by leaky ReLU

## Computing Number of Parameters
- 2-layer FC-Net [3, 4, 2]
	- $4 + 2$ = 6 neurons, not including inputs
	- $3*4 + 4*2$ = 20 weights
	- $4 + 2$ = 6 biases
	- 20 weights + 6 biases = 26 learnable parameters

## Representational Power
- As we increase the size and number of layers in a NN, the **capacity** of the network increases (i.e. more complex decision boundaries).
- **Overfitting** occurs when a model with high capacity fits the noise in the data instead of the (assumed) underlying relationship.
- Overfitting can be controlled with **regularization**. Decision boundary smoothens as regularization strength increases.
- Smaller networks are harder to train with gradient descent. Though they have few local minima, most of the minima are bad. Because of this, despite the risk of overfitting, always tend towards using larger networks and then applying regularization.

## Dropout
- While training, dropout is implemented by only keeping a neuron active with some keep probability $p$ (a hyperparameter), or setting it to zero otherwise.
- Why do we divide the outputs of our hidden layers by $p$ during training with **inverted dropout**? Because at test time all neurons see all their inputs, so we want the outputs of neurons at test time to be identical to their expected outputs at training time.

## Weight Initialization
- Never initialize weight matrices to all zeros. If every neuron computes the same output, then they will also compute the same gradients. All the parameter updates will then also be exactly the same. Zero-initialization prevents any asymmetry between neurons.
- Instead initialize with **small random numbers by symmetry breaking**. Sample a neuron's weight vector from a standard normal distribution and divide by standard deviation.
- **Xavier initialization good for sigmoid:** `w = np.random.randn(n) / sqrt(n)` because the variance is $1/sqrt(n)$.
- **He initialization good for ReLU:** `w = np.random.randn(n) * sqrt(2.0/n)` because the variance is $2/sqrt(n)$.
- In practice, initialize biases to 0.
- Initializations too small: activations go to zero. Gradients zero. No learning.
- Initializations too big: activations saturate for tanh. Gradients zero. No learning.
- Initialization just right: Nice distribution of activations at all layers. Learning proceeds nicely.

## Tips for Training
- Loss not decreasing? Learning rate is too low.
- Loss exploding? Learning rate too high.
- Hyperparameter sweeps with cross-validation.
- In practice, random search in the log-space works better than grid search.
- If loss starts off flat and then drops, bad initialization is probably the culprit.
- If big gap between validation and training accuracies, then model is overfitting. Try increasing the regularization strength.
- If there is no gap, then try increasing model capacity (i.e. adding more layers).
- You want the ratio of weight updates to weight magnitudes to be around 0.001.

## Batch Normalization, in General
>- Input $x: N \times D$
- $\mu$ and $\sigma$: $1 \times D$
- $\gamma$ and $\beta$: $1 \times D$
- Output $y = \gamma(x- \mu)/\sigma + \beta$

- Forces the activations throughout a network to take on a unit gaussian distribution at the beginning of the training. 
- Typically applied after FC layer and before activation.
- Improves gradient flow through the network.
- Allows higher learning rates.
- Reduces the strong dependence on initialization. 
- Acts as a form of regularization in a funny way, and slightly reduces the need for dropout.
- To add consistency for batch norm between train and test time, compute running means and variances.
- Scale and shift with $\gamma$ and $\beta$, respectively.

## Batch Normalization for ConvNets
>- Input $x: N \times C \times H \times W$
- $\mu$ and $\sigma$: $1 \times C \times 1 \times 1$
- $\gamma$ and $\beta$: $1 \times C \times 1 \times 1$
- Output $y = \gamma(x- \mu)/\sigma + \beta$

## Layer Normalization, in General
>- Input $x: N \times D$
- $\mu$ and $\sigma$: $N \times 1$
- $\gamma$ and $\beta$: $1 \times D$
- Output $y = \gamma(x- \mu)/\sigma + \beta$

- Same behavior at test and train. No dependence on other examples.
- Can be used in recurrent networks.

## Instance Normalization for ConvNets
>- Input $x: N \times C \times H \times W$
- $\mu$ and $\sigma$: $N \times C \times 1 \times 1$
- $\gamma$ and $\beta$: $1 \times C \times 1 \times 1$
- Output $y = \gamma(x- \mu)/\sigma + \beta$

- **Group normalization** is a middle ground between layer normalization and instance normalization.

## Problems with SGD
- If loss function has high condition number, the loss changes quickly in one direction and slowly in another. Causes zig-zag descent.
- Local minima and saddle point also have zero gradient. So the gradient gets stuck. Saddle points are really common in higher dimensions.

**<span style="color:green">Solutions</span>:**

- SGD with Momentum: build up velocity as a running mean of gradients

		v = mu * v - learning_rate * dx # integrate velocity
		x += v # integrate position 
		
	- `mu` is friction. Dampens the velocity and reduces the kinetic energy of the system, or otherwise the particle would never come to a stop at the bottom of a hill
- Nesterov Momentum: lookahead

		x_ahead = x + mu * v
		# evaluate dx_ahead (the gradient at x_ahead instead of at x)
		v = mu * v - learning_rate * dx_ahead
		x += v
		
- Second-order optimization methods like Newton's method which uses the Hessian, or quasi-Newton methods like L-BFGS which use approximations of the Hessian. But these don't scale too well for deep learning purposes.

## Per-Parameter Adaptive Learning Rate Methods
- Generic techniques of annealing the learning rate:
	- **Step decay:** Reduce the learning rate by some factor every few epochs. Typical values might be reducing the learning rate by a half every 5 epochs, or by 0.1 every 20 epochs.
	- **Exponential decay:** has the mathematical form $\alpha = \alpha_0 e^{-kt}$, where $\alpha_0$ and $k$ are hyperparameters and $t$ is the iteration number (but you can also use units of epochs).
	- **$1/t$ decay:** has the mathematical form $\alpha = \alpha_0/(1 + kt)$, where $\alpha_0$ and $k$ are hyperparameters and $t$ is the iteration number.
	- If you can afford the computational budget, err on the side of slower decay and train for a longer time.
- **Adagrad**

		# Assume the gradient dx and parameter vector x
		cache += dx**2
		x += - learning_rate * dx / (np.sqrt(cache) + eps)		
	- `cache` is used to element-wise normalize update step.
	- Weights that receive high gradients will have their effective learning rate reduced, while weights that receive small or infrequent updates will have their effective learning rate increased.
	- <span style="color:red">Problem:</span> Monotonic learning rate usually proves too aggressive and stops learning too early.
- **<span style="color:green">Solution: RMSProp</span>**

		cache = decay_rate * cache + (1 - decay_rate) * dx**2
		x += - learning_rate * dx / (np.sqrt(cache) + eps)		
	- Uses moving average of squared gradients to fix Adagrad.
	- RMSProp still modulates the learning rate of each weight based on the magnitudes of its gradients, which has a beneficial equalizing effect, but unlike Adagrad the updates do not get monotonically smaller.
- **Adam**

		# t is your iteration counter going from 1 to infinity
		m = beta1*m + (1-beta1)*dx
		mt = m / (1-beta1**t)
		v = beta2*v + (1-beta2)*(dx**2)
		vt = v / (1-beta2**t)
		x += - learning_rate * mt / (np.sqrt(vt) + eps)
			
	- Includes bias correction.
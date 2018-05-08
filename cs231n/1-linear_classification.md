# Image Classification Pipeline, Loss Functions, and Optimization ([Lecture 2](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture02.pdf) and [Lecture 3](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture03.pdf))

## Task
- Assign labels to pictures.
- An 800px $\times$ 600px image has a third dimension representing the number of color channels (3 channels, RGB).

## Challenges
- Viewpoint variation
- Illumination
- Deformation
- Occlusion
- Background clutter
- Intraclass variation

## The Machine Learning Paradigm
- **Train** to create model from data
- **Predict** to generate labels from model

## Classifier Type #1: Nearest Neighbor
- Train: memorize all data and labels; O(1)
- Test: predict the label of the most similar training image, O(N)
- What does "most similar" mean?
	- Distance metric to compare images: **L1 distance** (sum of pixel-wise absolute value difference)
- Produces piecewise-linear decision boundaries.

**<span style="color:red">Problems:</span>** we want classifiers that are **fast** at prediction and **slow** at training

## Classifier Type #2: k-Nearest Neighbors
- Instead of copying label from nearest neighbor, take **majority vote** from $k$ closest points. If $k$ is the size of the training set, then kNN will always return the most commonly occurring label in the training set.
- Another possible distance metric: **L2 distance** (Euclidean)
- **Hyperparameters**: value of $k$ and distance metric
	- Choose hyperparameters on val and evaluate on test
	- **Cross-validation:** split the data into folds, try each fold as validation, and average results
- When $k = 1$, we recover the Nearest Neighbor classifier. 
- Higher values of $k$ have a smoothing effect that makes the classifier more resistant to outliers.

**<span style="color:red">Problems:</span>** slow at test time because of high dimensionality and distance metrics on pixels are not informative

## Classifier Type #3: Linear Classifiers
- Input $x$: image (array of 32x32x3 numbers = 3072x1)
- $W$: parameters or weights (10x3072)
- $b$: bias (10x1)
- Linear classifier: $f(x, W) = Wx + b$; 10 numbers giving class **scores**
- Each class has its own weights and biases that form the rows of $W$ and $b$
- There are three ways to interpret what a linear classifier is doing:

![viewpoints](https://preview.ibb.co/hPCJ8n/Screen_Shot_2018_05_07_at_2_06_24_PM.png)

**<span style="color:red">Problems:</span>** there are some difficult cases for a linear classifier

![difficult cases](https://image.ibb.co/bEHPg7/Screen_Shot_2018_05_07_at_2_02_14_PM.png)

## Loss Functions
- Motivation: we want to quantify our unhappiness with the class scores across the training data
- **Loss function** tells us how good our current classifier is
- There are different types of loss functions:

### Multiclass SVM Loss (aka Hinge Loss)

- The SVM loss function wants the score of the correct class $y_i$ to be larger than the incorrect class scores by at least some $\Delta$. If this is not the case, we will accumulate loss.
- Let $x_i$ be an image and $y_i$ be the label. The scores vector for $x_i$ is thus $s = f(x_i, W)$.
- Loop over all the wrong classes. Add to loss if the wrong class score is $\Delta \geq 1$ larger than the correct class score.
$$L_i = \sum_{j\neq y_i}max(0, s_j - s_{y_i} + 1)$$
- If you include the correct class in your looping, it inflates the loss by 1.
- Theoretically, $L_i = [0, \infty)$. If you took the mean instead of the sum, only this loss range would change.
- The loss over the entire dataset is the average $$L = \frac{1}{N}\sum_{i=1}^NL_i$$
- If over safety margin $\Delta$, $\epsilon$-size changes to $s_{y_i}$ don't affect losses.
- At initialization, $W$ values are small so all scores are around 0. In this case, $L_i = numClasses - 1$.
- If $L = 0$ for $W$, then $L = 0$ for all $\lambda W$ where $\lambda > 1$ too. This is called scale invariance of the weight matrix.
- If you squared the $max$ function, the incorrect class is penalized more. This leads to smoother decay. Called quadratic hinge or L2-SVM.
- After training an SVM classifier, it is possible to add a new data point to the training set that doesn't affect the decision boundary.
- Extra notes:
	- SVM is piecewise-linear when you look at one example in a linear classifier. Averaging over all examples gives you a bowl shape.
	- SVM is a convex function.
	- At the kinks, SVM is technically non-differentiable, but you can use the subgradient instead.

### Regularization
- Prevent the model from doing *too* well on training data.
- Add value to loss to penalize weight matrices with large values.
	- For example, if you have a $W$ with huge values, then the L2-norm of $W$ will also be large. So this scaled norm added to the loss increases the loss significantly. Regularization allows us to express a preference for weight matrices with smaller values (i.e. choose $W$ over $2W$ even though both give 0 loss).
- Different types: L2, L1, and ElasticNet weight decay regularization. L2 regularization likes to spread out the weights. So, it would prefer $[0.25, 0.25, 0.25, 0.25]$ over $[1, 0, 0, 0]$.
- Why?
	- Express preference over weights, as discussed above
	- Improve optimization by adding curvature
	- Make model simple (i.e. don't let it fit to noise)

### Softmax Classifier (aka Multinomial Logistic Regression)
- Want to interpret raw classifier scores as probabilities that sum to 1
- Softmax function: $$\frac{e^{s_{y_i}}}{\sum_je^{s_j}}$$
- These scores can be interpreted as the unnormalized log probabilities (logits) for each class. Replacing the hinge loss, as shown below, with a cross-entropy loss normalizes these scores:
$$L_i = -log(\frac{e^{s_{y_i}}}{\sum_je^{s_j}}) = -s_{y_i} + log\sum_je^{s_j}$$
- Theoretically, $L_i = (0, \infty)$. Can never actually hit 0!
- At initialization, all scores are about equal. In this case, $L_i = -log(1/numClasses) = log(numClasses)$ because each class has uniform probability.
- The cross-entropy objective wants the predicted distribution to have all of its mass on the correct answer.
- <span style="color:red">Problem:</span> In practice, dividing by large numbers is numerically unstable.
- <span style="color:green">Solution:</span> Shift all scores so that the highest value is 0.
- Peakiness or diffusivity of softmax probabilities is determined by regularization strength hyperparameter.
	- Weights decrease as regularization strength increases. As weights approach zero, probabilities become uniform. **Ordering** of softmax output is interpretable, but **not actual values**.

### Distinctions between SVM and Softmax Scores
- The SVM interprets output as class scores and its loss function encourages the correct class to have a score higher by a margin than the other class scores.
- The softmax classifier instead interprets the outputs as (unnormalized) log probabilities for each class and then encourages the (normalized) log probability of the correct class to be high (equivalently the negative of it to be low).
- SVM is a more local objective. If correct class already has highest score, then loss is zero, regardless of what other class scores are. However, with softmax, incorrect class values affect loss. As correct class probability increases and others decrease, softmax loss decreases.

## Optimization
- Now that we can formulate loss functions, we want to optimize over them. To reiterate, the loss function lets us quantify the quality of any particular set of weights $W$. The goal of optimization is to find $W$ that minimizes the loss function.

### The Gradient
- Proceed in the direction of steepest descent (negative gradient) to minimize loss.
- Two ways to compute the gradient:
	- **Numerical gradient:** <span style="color:red">slow, approximate</span>, <span style="color:green">easy</span>. Iterate over all dimensions one by one, introduce a small perturbation $h \to 0$ ($h \approx 10^{-5}$ in practice) along each dimension and calculate the partial derivative of the loss function along that dimension by seeing how much the function changed. Combine partials into gradient vector.
	- **Analytic gradient:** <span style="color:green">fast, exact</span>, <span style="color:red">error-prone</span>. Use calculus to **differentiate loss function with respect to weights**.
	- In practice, compare analytic gradient with numerical gradient to ensure correct analytic implementation.
- **Learning rate:** hyperparameter that needs tuning
	- With small step size, you make very little progress.
	- With larger step size, you risk overstepping, which can increase loss.

### Gradient Descent
- Procedure of repeatedly evaluating the gradient and then performing a parameter update is called gradient descent.

		while True:
			weights_grad = evaluate_gradient(loss_fun, data, weights)
			weights += - step_size * weights_grad # perform parameter update

**<span style="color:red">Problem: </span>**Expensive to compute the full loss function over the entire training set in order to perform only a single parameter update.
			
### <span style="color:green">Solution: </span>Mini-Batch Gradient Descent
- Compute the gradient over batches of the training data.

		while True:
			data_batch = sample_training_data(data, 256) # sample 256 examples
			weights_grad = evaluate_gradient(loss_fun, data_batch, weights)
			weights += - step_size * weights_grad # perform parameter update
			
- Why does this work? Because examples in the training data are correlated.
- Much faster convergence can be achieved in practice by evaluating the mini-batch gradients to perform more frequent parameter updates.

### Stochastic Gradient Descent
- Mini-batch of size 1.
- Not used in practice, because thanks to vectorization, it's actually faster to compute the gradient across a batch of examples instead of just 1.










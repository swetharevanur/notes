# Image Classification Pipeline, Loss Functions, and Optimization ([Lecture 2](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture02.pdf) and [Lecture 3](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture03.pdf))

## Task
- Assigns labels to pictures
- An 800px x 600px image has a third dimension representing the number of color channals (3 channels, RGB) 

## Challenges
- Viewpoint variation
- Illumination
- Deformation
- Occlusion
- Background clutter
- Intraclass variation

## The Machine Learning Paradigm
- **Train** to create model from data
- **Predict** to create labels from model

## Classifier #1: Nearest Neighbor
- Train: memorize all data and labels; O(1)
- Test: predict the label of the most similar training image, O(N)
- What does "most similar" mean?
    - Distance metric to compare images: **L1 distance** (sum of pixel-wise absolute value difference)
- Produces piecewise-linear decision boundaries.

**<span style="color:red">Problems:</span>** we want classifiers that are **fast** at prediction and **slow** at training

## Classifier #2: k-Nearest Neighbors
- Instead of copying label from nearest neighbor, take **majority vote** from $k$ closest points
- Another possible distance metric: **L2 distance** (Euclidean)
- **Hyperparameters**: value of $k$ and distance metric
	- Choose hyperparameters on val and evaluate on test
	- **Cross-validation:** split the data into folds, try each fold as validation, and average results
- When $k = 1$, we recover the Nearest Neighbor classifier. 
- Higher values of $k$ have a smoothing effect that makes the classifier more resistant to outliers.



**<span style="color:red">Problems:</span>** slow at test time because of high dimensionality and distance metrics on pixels are not informative

## Classifier #3: Linear Classifiers
- Input $x$: image (array of 32x32x3 numbers = 3072x1)
- $W$: parameters or weights (10x3072)
- $b$: bias (10x1)
- Linear classifier: $f(x, W) = Wx + b$; 10 numbers giving class **scores**
- Each class has its own $W$ and $b$
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
- Loop over all the wrong classes. Add to loss if the wrong class score is $\Delta \geq 1$ larger than the correct class score. If you include the correct class in your looping, it inflates the loss by 1.
- $$L_i = \sum_{j\neq y_i}max(0, s_j - s_{y_i} + 1)$$
- Theoretically, $L_i = [0, \infty)$. If you took the mean instead of the sum, only this loss range would change.
- The loss over the entire dataset is the average $$L = \frac{1}{N}\sum_{i=1}^NL_i$$
- If over safety margin $\Delta$, $\epsilon$-size changes to $s_{y_i}$ don't affect losses.
- At initialization, $W$ vaues are small so all scores are around 0. In this case, $L_i = numClasses - 1$.
- If $L = 0$ for $W$, then $L = 0$ for $2W$ too. This is called scale invariance of the weight matrix.
- If you squared the $max$ function, the incorrect class is penalized more. This leads to smoother decay. Called quadratic hinge or L2-SVM.

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
- Want to interpret raw classifier scores as probabilities




















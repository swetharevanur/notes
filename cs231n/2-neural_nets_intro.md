# Backpropagation and Neural Networks ([Lecture 4](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture04.pdf))

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

**<span style="color:red">Problems:</span>** we want classifiers that are **fast** at prediction and **slow** at training

## Classifier #2: K-Nearest Neighbors
- Instead of copying label from nearest neighbor, take **majority vote** from K closest points
- Another possible distance metric: **L2 distance** (Euclidean)
- **Hyperparameters**: value of K and distance metric
	- Choose hyperparameters on val and evaluate on test
	- **Cross-validation:** split the data into folds, try each fold as validation, and average results

**<span style="color:red">Problems:</span>** slow at test time because of high dimensionality and distance metrics on pixels are not informative

## Classifier #3: Linear Classifiers
- Input $x$: image (array of 32x32x3 numbers = 3072x1)
- $W$: parameters or weights (10x3072)
- $b$: bias (10x1)
- Linear classifier: $f(x, W) = Wx + b$; 10 numbers giving class **scores**
- Each class has its own $W$ and $b$. How do we know we have a good $W$?
- There are three ways to interpret what a linear classifier is doing:

![viewpoints](https://preview.ibb.co/hPCJ8n/Screen_Shot_2018_05_07_at_2_06_24_PM.png)

**<span style="color:red">Problems:</span>** there are some difficult cases for a linear classifier

![difficult cases](https://image.ibb.co/bEHPg7/Screen_Shot_2018_05_07_at_2_02_14_PM.png)


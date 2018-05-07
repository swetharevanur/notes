# Image Classification Pipeline (Lecture 2)

### Task
- Assigns labels to pictures.
- An 800px x 600px image has a third dimension representing the number of color channals (3 channels, RGB). 

### Challenges
- Viewpoint variation
- Illumination
- Deformation
- Occlusion
- Background clutter
- Intraclass variation

**The Machine Learning Paradigm:**
```
data --> TRAIN --> model
model --> PREDICT --> labels
```

### Classifier #1: Nearest Neighbor
- Train: memorize all data and labels; O(1)
- Test: predict the label of the most similar training image, O(N)
- What does "most similar" mean?
    - Distance metric to compare images: **L1 distance**
        - Sum of pixel-wise absolute value difference
- $\sum_i^j$

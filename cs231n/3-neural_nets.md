# Backpropagation ([Lecture 4](http://cs231n.stanford.edu/slides/2018/cs231n_2018_lecture04.pdf))

## Computational Graphs
- Graphical representation of any function, where square nodes are inputs and circle nodes are operations.
- **Forward pass** over graph means computing value of function by iteratively applying operations to inputs from left to right.
- **Backward pass** over graph means computing derivative of function with respect to each input by iteratively applying chain rule (multiplying local gradient by upstream gradient) from right to left.
- **Jacobian matrix:** derivative of each element of $z$ w.r.t. each element of $x$.
- Gradient w.r.t. to a variable should have the same shape as the variable.


## Important Gates
- `add`: gradient distributor; gradient on inputs will always equal output.
- `max`: gradient router; local gradient is 1.0 for the highest value and 0.0 for all other values.
- `mul`: gradient switcher; local gradients are inputs but switched.
- `sgn`: returns sign of input; gradient is 0.
- When an input contributes to two output operations, the local gradient of that input is the sum of both upstream gradients.


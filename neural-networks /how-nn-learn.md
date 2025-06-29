Neural Networks and Gradient Descent (digit recognition example)

1. Learning Process
     - training data- labeled images from MNIST database
     - random weights and biases
     - performance- initially poor (random guesses)
   - cost function:
     - measures how far the network's output is from the desired output.
     - eg: sum of squared differences between actual and target activations
     - lower cost indicates better performance
     - goal- minimize the average cost over all training examples
   - how the network learns:
     - by adjusting weights and biases to reduce the cost function

2. Optimization and Gradient Descent
     - the basic idea is to find the minimum of a cost function.
   - gradient descent:
     - involves computing the gradient (via backpropagation) and updating parameters
     - repeatedly move in the negative gradient direction
     - step size (learning rate) controls how big each move is
     - smaller steps near minima prevent overshooting
   - multivariable case:
     - input space is high-dimensional (13,000+ parameters)
     - gradient vector indicates the most impactful directions to adjust
   - basic intuition:
     - components of the gradient show which weights matter most
     - sign indicates whether to increase or decrease a weight
     - magnitude indicates importance of that adjustment

3. Characteristics of the Cost Function
   - must be smooth:
     - facilitates finding local minima
   - ensures gradual convergence
   - influences neuron activation:
     - continuous activations preferred over binary to allow smooth optimization
   - essentially, it measures the difference between network output and true label.
   - for a single example, sum of squared differences
   - total cost, average over all training examples
   - therefore, the goal is to find weights and biases that minimize this cost.

To summarize,
   - neural networks learn by minimizing a cost function via gradient descent.

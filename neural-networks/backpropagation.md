Understanding Backpropagation in Neural Networks
# Intuition

1. Introduction to Backpropagation
   - main algorithm for training neural networks
   - computes the gradient of the cost function with respect to weights and biases
   - enables the network to learn by minimizing the cost

2. Understanding the Gradient
   - gradient: vector indicating how to change weights/biases to reduce cost
   - components: sensitivity of cost to each weight/bias
   - interpretation: larger magnitude means greater influence on cost

3. Intuitive Walkthrough of Backpropagation
   - focus on one training example (eg: an image of the digit 2)
   - initial activations are often random (eg: 0.5, 0.8, 0.2)
   - desired outcome: increase the activation of the correct output neuron (eg: 2) and decrease others
   - for this, adjustments are made to:
     - biases
     - weights
     - activations of previous layers (indirectly, through weight updates)

4. How Weights and Biases Affect Activation
   - activation depends on:
     - weighted sum of previous layer activations + bias
     - passed through an activation function (sigmoid, relu)
   - to increase activation:
     - increase bias
     - increase weights connected to active neurons
     - influence previous layer activations (indirectly)

5. Influence of Weights
   - weights with larger values have more impact
   - connections from highly active neurons have bigger influence

6. Propagation of Errors Backwards
   - desired changes for the output layer are combined
   - these are propagated backwards through the network
   - recursion: apply the same process to earlier layers
   - each layer's adjustments depend on the errors from the layer above

7. Combining Effects from All Training Examples
   - each example suggests how to change weights/biases
   - average these suggestions to approximate the true gradient
   - this process ensures the network learns general patterns, not just specifics

8. Practical Implementation (mini batch gradient descent)
    - computing gradients for all data is slow
    - so data is divided into mini batches 
    - then gradient for each mini batch is computed
    - and weights are updated based on this approximation
    - this technique is stochastic gradient descent
    - results in dort of a drunken stumble down the cost surface, but is much faster

# Chain Rule in Neural Networks

1. Key Concepts and Terminology
   - activation (a): output of a neuron after applying the activation function.
   - weighted sum (z): input to the activation function, computed as \( z = w \times a_{prev} + b \).
   - cost function (C): measures the difference between network output and true label; eg: \( C = (a^{(L)} - y)^2 \).
   - weights (w): parameters that connect neurons between layers.
   - biases (b): additional parameters added to the weighted sum.

2. Step by Step Breakdown
   - simple network setup:
     - single neuron per layer.
     - last layer activation \( a^{(L)} \).
     - target output \( y \).
     - cost \( C = (a^{(L)} - y)^2 \).

   - calculating sensitivity of cost to weights:
     - focus on \( \frac{\partial C}{\partial w^{(L)}} \).
     - using the chain rule:
       \[
       \frac{\partial C}{\partial w^{(L)}} = \frac{\partial C}{\partial a^{(L)}} \times \frac{\partial a^{(L)}}{\partial z^{(L)}} \times \frac{\partial z^{(L)}}{\partial w^{(L)}}
       \]
     - derivatives:
       - \( \frac{\partial C}{\partial a^{(L)}} = 2(a^{(L)} - y) \).
       - \( \frac{\partial a^{(L)}}{\partial z^{(L)}} \): Derivative of activation function (e.g., sigmoid).
       - \( \frac{\partial z^{(L)}}{\partial w^{(L)}} = a^{(L-1)} \).

   - sensitivity to bias:
     - similar to weights, but \( \frac{\partial z^{(L)}}{\partial b^{(L)}} = 1 \).

   - propagating backwards:
     - sensitivity of cost to previous layer activations involves the weight \( w^{(L)} \).
     - this backward propagation allows updating all weights and biases layer by layer.

3. Extending to Multiple Neurons per Layer
   - cost function:
     - sum over all output neurons: \( C = \sum_j (a^{(L)}_j - y_j)^2 \).
   - weights:
     - \( w^{(L)}_{jk} \): weight from neuron \( k \) in layer \( L-1 \) to neuron \( j \) in layer \( L \).
   - derivatives:
     - similar chain rule applies, but now considering multiple paths influencing each neuron.
     - the influence of a neuron in layer \( L-1 \) on the cost is the sum of influences through all neurons in layer \( L \).

# Summary of Backpropagation
   - uses the chain rule to compute how small changes in weights, biases, and activations affect the overall cost. 
   - by systematically propagating these sensitivities backward through the network, gradients needed for optimization algorithms like gradient descent can be efficiently computed.
   - repeats over many iterations to converge to a good solution

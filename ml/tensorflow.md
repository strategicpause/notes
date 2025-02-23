## Notes
* **What is Keras?** Keras acts as an interface for building and training neural networks. It provides a high-level API for defining and working with models. It is the default high-level API for TensorFlow.
* **What is `models.Sequential`?** A stack of layers used to define a neural network model. Each layer in the neural net has exactly one input tensor and one output tensor.
* **What is a tensor?** A multi-dimensional array, or matrix, used in ML. It is a data structure used to store and manipulate numerical data.
* **What is a Flatten, Dense, and Dropout layer?**
	* Flatten - Used to convert a multi-dimensional input tensor into a one-dimensional array. This is typically done before feeding it into a fully connected layer for further processing. Useful for preparing 2-D data structures (like images) into 1D for layers that expect it (such as Dense).
	* Dense - Each Neuron in the layer is connect to every neuron in the previous layer, making it a fully connected network.
		* `units` - Numer of neurons in the Dense layer.
		* `activation` - Activation function to apply to the output of the layer.
			* `relu` - Rectified Linear Unit. Popular for hidden layers in deep learning. Non-linear which helps the network learn complex patterns.
	* Dropout - A regularizaation technique used to neural networks to prevent over-fitting during training. It helps improve the model's ability to generalize to new unseen data.
* **What is the softmax function?** Used in ML classification problems to transform a a vector of scores (referred to as logits) into probabilities.
* **What is a loss calculation?** Computes a scalar value (loss) that quantifies how far off a model's predictions are from the actual target values. The goal of training is to minimize this loss by updating the model's parameters.
* **What is a `SparseCategoricalCrossentropy` loss function?** Used for multi-class classification problems where the target labels are integers.
* **What is the `adam` optimizer?** Adaptive Moment Estimation. An extension of stochastic gradient descent that adjusts the learning rate of each parameter dynamically.
* **What is an optimizer?** Updates the model based on the data it sees and the provided loss function.
* **What is a loss function?** Measures how accurate the model is during training. You want to minimize this function when building the model.
* `Model.fit` will adjust your model parameters in order to minimize the loss.
* `Model.evaluate` will check the models performance against a validation set or test set.
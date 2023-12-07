# Machine Learning (Tensorflow & Keras)

# Introduction

## Jupyter Notebook & Tensorflow

A cloud developer advocate at Microsoft teaches this course. For the most part of this course, you can use python code to run. 

1. Jupyter is Notebook is a unique environment where you can run commands in the browser but the interpreted and executed output is returned to your browser. 
2. This mechanism helps us connect with a server where python is running remotely. The value is seen when you want powerful GPUs to borrow on a different server and you run them.

```bash

python3 -m venv myenv      # Create a virtual environment
source myenv/bin/activate # Activate the virtual environment (for macOS/Linux)

pip3 install jupyter # installs jupyter

jupyter notebook # kicks of notebook in the browser

```

# Resources

- 💻 GitHub repos (for class, TFJS -> 🎥 [**pose demo**](https://storage.googleapis.com/tfjs-models/demos/posenet/camera.html) 🕺, books repos, TF/Keras demos)
    - 🕸 Websites (TF, TF-hub)
    - 📚 Books:
        - "Deep Learning with Python" by [François Chollet](https://github.com/fchollet/deep-learning-with-python-notebooks)
        - "Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow: Concepts, Tools, and Techniques to Build Intelligent Systems" by [Aurélien Géron](https://github.com/ageron/handson-ml2)
        - "Hands-On Neural Networks with TensorFlow 2.0" by [Paolo Galeone](https://github.com/PacktPublishing/Hands-On-Neural-Networks-with-TensorFlow-2.0)

# Key concepts

## Machine Learning & Statistics

![Untitled](Machine%20Learning%20(Tensorflow%20&%20Keras)%20d8149de3c5cd413eb23114741d398ae6/Untitled.png)

Consider you are running a shoe company back in 1900s and you want to know what sizes you will have to manufacture those. You can practically, speak to 100 people and know x people have size 8, y people have size 9, etc.,

You can find an average or mean number telling you how many shoes you will want to actually manufacture and ship to the people. If you plot these on a graph, you get at graphs like bell curve.

1. *****mean*****  is the tip of the bell curve.
2. ***********standard deviation*********** is the width of the bell curve.

<aside>
💡 Doing too much of statistics has naturally performed well to grown into a new field called machine learning.

</aside>

## Deep Learning

As humans we wanted to have computers think like humans. But humans think through neurons. So idea is… what if I can replicate the thinking of neurons on to computers.

Here’s were we got at:

![Untitled](Machine%20Learning%20(Tensorflow%20&%20Keras)%20d8149de3c5cd413eb23114741d398ae6/Untitled%201.png)

1. There can be multiple inputs to the neuron
2. There are some weights associated with each input
3. programatically you can amplify one input against the other technically by varying weight.

$$
output = x1w1 + x2w2+x3w3 +.... + constant
$$

1. This constant is “bias”.
2. The output usually varies from `-Infinity` to `+Infinity`. It is very broad range. We try to fit into `0` and `1` through something called signoid fn.

![Untitled](Machine%20Learning%20(Tensorflow%20&%20Keras)%20d8149de3c5cd413eb23114741d398ae6/Untitled%202.png)

<aside>
💡 Deep learning came from all those neuron layers so that you make decision. Jumping from linearity to more complex polynomials. We can better fit into distributions.

It those weights and biases(constants) are actually being selected. We do not select them. They are autmoatically learned. This is the process of machine learning

</aside>

[](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)

### Predictions?

How is that model (in this case the group or layers of neurons) is actually predicting?

### How do you train a Neural Network

Problem

Let’s say the problem you want to solve is to determine whether an image has a hot dog. Assume you have a 1000 images of Hot dog and another 1000 images you don’t have hot dog.

Arrangement

1. Imagine there are multiple layers of neurons. 
2. Each of them is connected to each other with the help of **************************************inputs, weights and constants (biases).**************************************

Training the Model

1. The idea of training the model is essentially about determining the weights and biases. 
2. The goal is with right weights and biases, if you give any inputs, the final neuron after forward propagation will determine if the current image has hot dog or not.
3. Training: Model (arrangement of layers) consumes the 1000 right images of hotdog (training data) and keeps on adjusting weights and constants through out forward and backward propagation. Propagation is simply a term used movement of indicated from one thread to another.
4. Once the trained with right amount of data, there are sweet spots of weights and constants determined. So when new image is given, the probabilty of selecting right image also works very well. 

Interesting Math

1. Behind the scenes the weights are calculated based on of Derivatives.
2. One uses signoid function to keep the output values between 0 and 1. 

## Training Models

One usual approach in computer science is:

```
Input, Instructions (Algo) --> CPU/GPU/TPU --> Output

IN ML:

Output (training data) --> CPU/GPU/TPU --> Algorithm
```

## Into AI

![Untitled](Machine%20Learning%20(Tensorflow%20&%20Keras)%20d8149de3c5cd413eb23114741d398ae6/Untitled%203.png)

1. Statistics has grown into Machine Learning: It’s that point where we have loads of information and trying to see it in shorter sense. What’s the average shoe size? sort of questions
2. Machine Learning into Deep Learning: This is phase we allow to make computer shorter decisions such as “spam” not “not spam”. The layers of neurons do this forward propagation using varying weights/biases to make one decisions.
3. Deep Learning to AI: This is the point when computer creating something. Let’s say, wegihts and biases are known, can inputs help it create something new? 
    1. For example, you want to create a image of Eiffel tower in handdrawn format.
    2. The handdrawn format will take-in images of clear Eiffle tower and handrawn random image. But the model first layers of neurons will focus on the style of drawing.
    3. The will be used to generate new image.

## Jupyter Notebook

There are a few key reasons why it's better to use Jupyter Notebook instead of running Python programs directly:

1. Jupyter Notebook allows you to run code one block at a time instead of all at once. This makes it easier to test and debug your code incrementally.
2. Jupyter Notebook provides inline visualization and rich output. Charts, images, and other output can be displayed directly in the notebook. This makes it easier to understand your results.
3. Jupyter Notebook allows you to intermix code, text, and multimedia. This makes it great for explanatory and exploratory programming. You can walk through your code step-by-step and explain it as you go.
4. Jupyter Notebook files (.ipynb) contain all the code, output, and narrative in a single document. This makes sharing and collaboration easier since everything is in one self-contained file.
5. Jupyter Notebook provides tools for interactive computing across dozens of programming languages, not just Python. So you're not limited to Python.
6. Jupyter Notebook runs in a browser and can be accessed remotely. This allows you to work with notebooks on different devices or share them via the web.
7. Jupyter Notebook has extension support, allowing customization and adding new functionality through extensions.

So in summary, Jupyter Notebook provides a richer interactive programming environment compared to running Python scripts directly. The combination of code, output, multimedia, and accessibility makes Jupyter Notebook a very useful tool for programming, analysis, and sharing.

## TensorFlow

In TensorFlow, a tensor is a multi-dimensional array that can be used to represent data of different types and shapes. Tensors can be used to represent vectors, matrices, and higher-dimensional arrays. They form the basic building blocks of TensorFlow computations.

![Untitled](Machine%20Learning%20(Tensorflow%20&%20Keras)%20d8149de3c5cd413eb23114741d398ae6/Untitled%204.png)

The Models are very dependant on the kind of hardware they will run upon. It’s important the software layer to optimise the attached hardware to run models. Tensorflow solves this problem by focussing on “computational graph” that will allow our source codes to run on the systems we have. 

However, in recent times, Tensorflow 2.0 started to focus on eagar execution. It means tensorflow layer systems will optimise for eager execution. However, the first in the market to win this space is PyTorch. So researchers were able to quickly play with their ideas. 

### Linear Regression

Linear regression is a simple yet powerful algorithm in machine learning. It is used to model the relationship between two variables, where one variable is the dependent variable and the other variable is the independent variable. The goal of linear regression is to find the line that best fits the data points. This line is called the regression line.

Linear regression can be implemented using TensorFlow. The steps to implement linear regression in TensorFlow are:

1. Load the data
2. Define the model
3. Define the loss function
4. Define the optimizer
5. Train the model
6. Evaluate the model

In order to implement linear regression, you will need to have a basic understanding of TensorFlow and Python programming.

### Convolution

Convolution is a key operation in convolutional neural networks (CNNs) that applies a convolution operation to the input data using a convolution filter or kernel. Here are some key points about convolution in CNNs:

- It involves sliding a convolution filter over the input image/data and computing the dot product between the filter and the underlying input at each location.
- This results in an activation map that provides information about the presence of detected features in the input.
- Stacking convolution layers allows CNNs to build up higher level feature representations of the raw input data.
- The convolution filter contains weights that are learned during the training process. Different filters detect different types of features.
- Common convolution filter sizes are 3x3, 5x5, 7x7 etc. The size impacts the scope of the detected features.
- Padding can be added to preserve spatial dimensions across convolution layers.
- Strided convolutions allow stepping the filter more than 1 cell at a time to reduce the output dimensions.
- Pooling layers like max pooling are often inserted between successive convolution layers to progressively reduce spatial size.

So in summary, convolution operations enable CNNs to identify meaningful features in input data and build increasingly abstract representations using multiple layers. This makes CNNs very effective for computer vision and other spatial data tasks.

### Pooling

Pooling is a technique used in convolutional neural networks (CNNs) to progressively reduce the spatial size of the representations and network parameters, while still retaining important information. Here are some key points about pooling:

- It operates on each activation map separately, dividing them into rectangular pooling regions.
- The most common pooling operation is max pooling, which extracts the maximum activation value from each pooling region.
- Other pooling operations include average pooling, sum pooling, etc.
- Pooling helps make the representations approximately invariant to small translations in the input.
- It reduces the number of parameters in the network, allowing more layers to be trained.
- Pooling also helps control overfitting.
- Typical pooling window sizes are 2x2 or 3x3 applied with a stride of 2, which halves the dimensions after each pooling layer.
- Pooling layers are periodically inserted between successive convolution layers in a CNN architecture.
- The location of the maximum value in a pooling region encodes rough spatial information.

So in summary, pooling progressively reduces the spatial size of the representations to help manage computations and overfitting, while retaining the most salient information. It builds some translation invariance into the CNN.
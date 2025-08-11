## deep learning fundamentals
- **neural networks**: a type of machine learning model that takes an input (x) and produces a prediction (ŷ).
- **loss function**: a function that measures how wrong a model's prediction is compared to the true answer (y). the goal of training is to minimize this loss.

## how a neural network learns
- **1. forward pass**: the input data is passed through the network's layers. each layer has weights (which start as random numbers) that modify the data until a final prediction is made.
- **2. loss calculation**: the model's prediction is compared to the true label using the loss function.
- **3. backpropagation**: this is the core learning step. the model calculates the **gradient** for every single weight in the network.
    - a **gradient** tells you how much a specific weight contributed to the final loss. it's calculated by working backward from the loss using calculus (the chain rule).
- **4. weight update (gradient descent)**: the model uses the gradients to update each weight, nudging it in the direction that will reduce the loss.
    - this update is controlled by the **learning rate**, a hyperparameter that sets the size of the update step. a large learning rate can overshoot the best solution, while a small one can be too slow.

- **non-linearity**: this is a crucial component. without non-linear activation functions (`relu`, `sigmoid`, etc.) between layers, a neural network can only learn simple, straight-line patterns. non-linearity allows it to learn the complex, curved decision boundaries needed for real-world tasks.

## pytorch: the practical tool
- a popular deep learning framework used to build and train neural networks.

## the tensor: the building block of pytorch
- a **tensor** is the fundamental data structure in pytorch. it's a multi-dimensional array that can represent any type of data (images, text, etc.).
- **tensor properties**:
    - **dtype**: the data type of the numbers in the tensor (e.g., `float32`, `int8`).
    - **device**: where the tensor is stored for computation (either the `cpu` or the `gpu`). gpus are much faster for deep learning.
    - **requires_grad**: a boolean (`true`/`false`) that tells pytorch if it needs to calculate gradients for this tensor during backpropagation. if `true`, the tensor is a learnable parameter.

## key pytorch operations
- **creating tensors**: can be created from lists or numpy arrays, or initialized with random values (`torch.rand()`) or zeros (`torch.zeros()`).
- **tensor math**: supports both element-wise operations and, most importantly, **matrix multiplication** (`torch.matmul()`), which is the core operation in a neural network layer.
- **reshaping**: you can change the shape of a tensor using `.reshape()` as long as the total number of elements stays the same.
- **numpy bridge**: tensors can be seamlessly converted to and from numpy arrays (`.numpy()` and `torch.from_numpy()`). a tensor must be on the cpu to be converted to numpy.
- **reproducibility**: you can set a **random seed** (`torch.manual_seed()`) to ensure that random processes (like initial weight values) are the same every time you run your code.

## the pytorch workflow (a typical project)
- **1. data preparation**: getting your data ready for the model.
    - **transforms**: define a sequence of operations to apply to your data, like resizing images or converting them to tensors.
    - **datasets**: use pytorch's `imagefolder` to easily load image data from organized folders.
    - **dataloader**: wraps the dataset and handles batching (feeding data in small groups), shuffling, and loading the data efficiently.
- **2. model building**: defining the architecture of your neural network.
    - this is typically done by creating a class that inherits from `nn.module`.
    - **layers**: you define the layers of your network in the `__init__` method (e.g., `nn.conv2d` for images, `nn.linear` for classification, `nn.relu` for non-linearity).
    - **forward pass**: you define how data flows through your layers in the `forward` method.
- **3. defining loss and optimizer**: choosing the right tools to train your model.
    - **loss function**: depends on the task. common choices are:
        - `bcewithlogitsloss`: for binary classification.
        - `crossentropyloss`: for multi-class classification.
        - `l1loss` (mae): for regression.
    - **optimizer**: the algorithm that performs the weight updates (e.g., `torch.optim.adam` or `torch.optim.sgd`).
- **4. the training loop**: the process of teaching the model. for each **epoch** (one full pass over the training data):
    - set the model to training mode: `model.train()`.
    - loop through the `dataloader`.
    - zero the gradients: `optimizer.zero_grad()`.
    - perform the **forward pass** to get predictions.
    - calculate the **loss**.
    - perform **backpropagation**: `loss.backward()`.
    - update the weights: `optimizer.step()`.
- **5. the evaluation loop**: checking the model's performance on unseen data (like the validation or test set).
    - set the model to evaluation mode: `model.eval()`.
    - use `with torch.no_grad():` to turn off gradient calculations, making it faster and more memory-efficient.
    - loop through the test/validation `dataloader`, get predictions, and calculate metrics like accuracy and loss. no backpropagation or weight updates happen here.
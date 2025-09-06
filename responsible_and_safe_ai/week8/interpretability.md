## introduction to interpretability
- **goal**: to understand *how* and *why* a machine learning model makes its decisions. it's about opening the "black box."
- **transparency**: providing clarity about a model's internal workings. this is crucial for debugging, monitoring for unexpected behavior, and ensuring the model is aligned with its intended purpose.
- **saliency maps**: a common technique that creates a "heat map" to highlight influential parts of an input. other names include **sensitivity maps** or **feature attribution**. a "convolution map" is not an attribution method.

## pixel attribution methods
### gradient-based methods
- use the model's internal **gradients**. they are fast but require access to the model's internals.
- _what this means: the gradient tells you "if i change this pixel's value a tiny bit, how much will the final prediction change?" a large gradient means the pixel is very important._
- **smoothgrad**: a technique to reduce noise in saliency maps by averaging maps from noisy copies of an image.
- **guided backpropagation**: modifies backpropagation by **zeroing out negative activations and gradients**, which only highlights positive contributions and creates sharper maps.
- _special model detail: **stylegan3** is a generative model specifically designed to be equivariant to translation and rotation._

### perturbation-based methods
- work by systematically changing (**perturbing**) or blocking parts of the input. they are **model-agnostic** (work on any model) but are **computationally expensive** as they require many forward passes.
- **occlusion**: the simplest version, where a gray square is slid across the image to see how predictions change.
- **optimized-mask methods**: these methods try to find an optimal "mask" that explains a prediction. they can be **brittle** because the results often depend heavily on random initialization and hyperparameters.

### sanity checks for saliency methods
- a crucial step to ensure an explanation method is actually explaining the model and not just detecting edges in the image.
- **layer randomization test**: if you randomly re-initialize the weights of a trained model's layers, the saliency map should change significantly. if the **saliency map remains largely unchanged**, it's a major red flag that the method is not truly connected to the model's learned knowledge.

## model-agnostic methods
- these are powerful techniques that can explain the predictions of *any* model, regardless of its internal structure. they treat the model as a black box.
- **lime (local interpretable model-agnostic explanations)**:
    - explains a single, individual prediction.
    - _how it works: it takes the data point you want to explain, creates a bunch of slight variations of it (the "neighborhood"), gets the model's predictions for all those variations, and then trains a simple, interpretable model (like a linear model) that only has to explain the behavior in that small neighborhood._
    - **limitation**: lime's explanations are **locally faithful but not necessarily globally consistent**. an explanation for one prediction might contradict an explanation for a similar one.
- **shap (shapley additive explanations)**:
    - a method from game theory that explains a prediction by assigning a "payout" or "contribution value" to each input feature.
    - _what this means: it tells you exactly how much each feature (e.g., age, income, zip code) contributed to pushing the final prediction away from the average. it's considered a very robust and fair way to attribute importance._

## architecture-specific methods
- **grad-cam (gradient-weighted class activation mapping)**:
    - a popular technique for cnns that produces a coarse-grained (low-resolution) but clean saliency map.
    - _how it works: instead of looking at the input pixels directly, it looks at the activations of the final convolutional layer. it uses the gradients to understand which of these high-level feature maps were most important for the final decision, and then overlays that information on the original image._
- **protopnet (prototypical part network)**:
    - an "interpretable by design" model. it is not a post-hoc explanation method, but a model architecture that is inherently transparent.
    - _how it works: it classifies an image by breaking it down into parts and comparing those parts to a learned library of "prototypes." its final decision is based on a weighted sum of the similarity scores to these prototypes, making its reasoning process explicit by showing which prototypes it used._

## advanced interpretability concepts
- **feature visualization**:
    - a technique to understand what individual neurons or layers in a neural network have learned to detect.
    - _how it works: it starts with a random noise image and uses an optimization process to gradually change the image until it maximally activates a specific neuron you're interested in. the resulting image is a visual representation of that neuron's "preferred" input._
- **mechanistic interpretability**:
    - the ambitious research goal of completely **reverse-engineering a neural network** to understand the function of every neuron and the "circuits" they form.
    - this involves understanding "circuits," where some connections are excitatory (amplify signals) and others are **inhibitory (reduce the probability of information transfer)**.
- **probing**:
    - a method to test if a model has learned a specific, human-understandable concept.
    - _how it works: you train a simple, linear "probe" model to see if it can extract a specific piece of information (like the part-of-speech of a word) just by looking at the internal activations of the larger, complex model. if the probe is successful, it's evidence that the complex model has learned to represent that concept internally._

## security risk: trojan attacks
- **trojan attack (data poisoning)**: an adversary implants a hidden "backdoor" into a model.
- **attack vectors**: common ways to implant a trojan include:
    1. **poisoning a fraction of a public training dataset** with trigger examples.
    2. **distributing pretrained models** (e.g., on hugging face) that already contain the hidden functionality.
- **neural cleanse**: a defense that reverse-engineers potential triggers.
    - _how it works: for each possible target class, it searches for the smallest possible input pattern (the potential trigger) that can reliably make the model misclassify a diverse set of inputs to that target class._
    - it relies on the observation that one can **optimize a small pattern that causes misclassification** to a target label.
    - if the trigger for one label is **significantly smaller/simpler than for others**, it suggests a trojan.
    - once a trigger is found, one mitigation strategy is to **prune the neurons** that are highly activated by it.
## representation engineering
- a technique to control the behavior of a trained model without retraining or fine-tuning.
- it works by directly modifying the model's internal activations (its "thoughts") during generation.
- the goal is to move from just observing a model's internal state to causally controlling its output.

## core concepts
- **concept vectors**: for a given trait (e.g., honesty, fairness, anger), a specific direction in the model's activation space is identified. this is the "concept vector."
    - **adding the vector**: makes the model exhibit *more* of the trait.
    - **subtracting the vector**: makes the model exhibit *less* of the trait.
- this technique is more reliable and consistent than simply trying to steer a model with prompt engineering.

## how it works: linear artificial tomography (lat)
- this is the formal name for the process of finding and using concept vectors.
- **step 1: design stimulus prompts**
    - create pairs of datasets that represent a concept and its opposite.
    - example for honesty: one dataset of true statements (`"1+1=2"`) and one of false statements (`"the sky is green"`).
- **step 2: collect internal activations**
    - pass these prompts through the model.
    - extract and save the hidden state activations from specific layers for each prompt.
- **step 3: find the concept vector**
    - use a simple algorithm (like pca or taking the mean difference) to find the vector that separates the "concept" activations from the "anti-concept" activations.

## applications & examples
- **honesty**: making a model tell the truth even when explicitly prompted to lie.
- **morality/harmlessness**: preventing a model from answering harmful questions (e.g., how to build a bomb) by increasing its "harmlessness" vector. this can defend against jailbreaks.
- **bias reduction**: steering a model away from generating stereotypical associations (e.g., assuming a doctor is male).
- **emotion control**: making a model's responses sound angry, happy, or fearful.
- **concept removal**: making a model "forget" a concept by subtracting the relevant vector.
## understanding ai bias
- **definition**: a systematic favoritism or discrimination by an ai system towards certain groups, individuals, or outcomes. it becomes a problem when it leads to a **negative impact**.
- ai models can exhibit biases related to gender, race, geography, age, religion, and profession.
- **western-centric perspective**: many current ai models and datasets are heavily skewed towards western norms, cultures, and languages.

## types of bias (definitions)
- **historical bias**: bias that already exists in the world and is mirrored in the data. the data is reflecting a past or present reality that is unfair.
- **sampling bias**: bias that occurs when the data collection process is not properly randomized and the data collected does not represent the true diversity of the population.
- **experimenter's bias**: when a researcher's own expectations or actions unintentionally influence the results of a study, for example by selecting data that confirms their hypothesis.
- **stereotype**: a widely held but oversimplified belief about a particular group or entity.

## examples of bias in action
- **image generation**: models often produce stereotypical images.
    - `meta ai` generating a "doctor" often results in female images; a "person driving" results in male images in western settings.
    - `midjourney` has shown ageism (women are younger), sexism, and racism (associating jobs with light-skinned people).
- **language models**: the output of llms can change based on culturally-specific names or gender.
    - `chatgpt` gave different answers about a "substance" depending on whether the names in the prompt were american, hispanic, or indian.
- **real-world consequences**:
    - **microsoft's tay chatbot**: was shut down after users quickly trained it to be racist and misogynistic.
    - **predictive policing**: algorithms have shown racial bias, leading to unfair targeting of certain communities.

## sources of bias in the ml pipeline
- **1. data collection**: the training data itself contains historical or societal biases. the data may also have sampling bias.
- **2. labeling & annotation**: the humans who label the data have their own conscious or unconscious biases, which get embedded in the dataset.
- **3. training process**: using a metric like **accuracy** on an imbalanced dataset can cause the model to ignore the minority class, which is a form of bias.
- **4. deployment & feedback loops**: after a model is deployed, user interactions can create feedback loops that reinforce and amplify existing biases.

## fairness metrics: measuring the bias
- fairness is not a single concept; there are many different mathematical ways to define and measure it. the right metric depends on the context.
- **group fairness**: ensures that a model's outcomes are statistically similar across different demographic groups.
- **individual fairness**: focuses on treating similar individuals similarly.
- **counterfactual fairness**: asks, "would the prediction change if this person's sensitive attribute were different?".
- **legal safety score (lss)**: a specific metric from a research paper that creates a score by combining a model's **fairness** and **accuracy**.

## mitigating bias
- **measuring bias**: frameworks like **crows-pairs** are used to quantify a model's preference for stereotypical sentences.
- **debiasing techniques**: methods like **auto-debias** can be used after training to reduce a model's reliance on stereotypical associations.
- **guardrails**: a general term for safety filters and rules built into llms to prevent them from generating harmful, toxic, or explicitly biased outputs.

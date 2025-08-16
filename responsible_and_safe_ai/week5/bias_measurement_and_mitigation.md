## measuring and removing bias
- the process is a two-step loop:
    1.  **measure bias**: use benchmark datasets to quantify the model's bias.
    2.  **remove bias**: apply a debiasing technique.
- after debiasing, you re-measure to evaluate the effectiveness of the method.

## benchmark datasets for bias study
- **stereoset**
    - measures stereotypical bias in pre-trained language models.
    - uses a **context association test (cat)**.
    - presents a model with a context and asks it to choose between a stereotypical, anti-stereotypical, or unrelated association.
    - **language modeling score (lms)**: measures a model's preference for meaningful associations over meaningless ones. this checks if the model still understands language properly.
    - **stereotype score (ss)**: measures a model's preference for stereotypical associations over anti-stereotypical ones. a score of 50 means no bias.
    - **idealized cat (icat) score**: combines both lms and ss to give an overall measure of bias, rewarding models that have both high language understanding and low stereotype bias.
- **crows-pairs**
    - measures social biases in masked language models across nine types of bias (e.g., race, religion, age, disability).
    - works by presenting the model with two sentences: one is more stereotypical, and the other is less stereotypical.
    - the model's bias is determined by calculating which sentence it considers more likely, using the **conditional pseudo log-likelihood**.
    - **jargon explained**: **conditional pseudo log-likelihood** is just a technical term for a score that measures how probable the model thinks a sentence is, given the context. a higher score means the model thinks the sentence is more plausible.
- **winogender & winobias**
    - datasets specifically for measuring gender bias in **coreference resolution** systems.
    - **jargon explained**: **coreference resolution** is the task of figuring out which words refer to the same person or object (e.g., linking "she" back to "the mechanic"). these datasets check if models unfairly associate pronouns with stereotyped jobs.
    - they test if a model can correctly link pronouns to occupations in both pro-stereotypical (e.g., "the secretary... she...") and anti-stereotypical (e.g., "the carpenter... she...") scenarios.
- **seat (sentence embedding association test)**
    - another common benchmark dataset used to measure bias in sentence embeddings.
- **bold dataset**
    - contains thousands of english text generation prompts to benchmark social bias in **open-ended language generation**.
    - covers five domains: profession, gender, race, religion, and political ideology.
- **reddit bias dataset**
    - a real-world dataset for evaluating bias in **conversational language models**.
    - it uses reddit's conversational structure (posts and comments) to provide interaction data.

## debiasing methods
- **counterfactual data augmentation (cda)**
    - a technique to reduce bias by automatically creating new data that balances the dataset. for example, it will find a sentence like "he is a doctor" and create a "counterfactual" copy: "she is a doctor."
- **auto-debias**
    - an automatic method to mitigate biases in pre-trained language models without needing human-annotated data.
    - it works in a two-stage framework:
        1.  **bias probing**: it automatically searches for "bias prompts" that maximize the disagreement in the model's predictions for stereotype-related words across different demographic groups.
        2.  **bias mitigation**: it then fine-tunes the language model by minimizing this disagreement, forcing the probability distributions of masked token predictions to be similar across different demographic groups.
- **mafia (multiple adapter fused inclusive language models)**
    - a debiasing architecture that uses multiple specialized **debiasing adapters**.
    - **jargon explained**: an **adapter** is a very small module of extra code that you "plug in" to a huge pre-trained model. instead of retraining the entire model (which is slow and expensive), you only train this tiny adapter. it's an efficient way to fine-tune a model's behavior for a specific purpose, like reducing one type of bias.
    - it **fuses** these adapters together to model the interplay between different biases.
    - this approach is more effective than single-adapter models (like one called **ideball**) which may fail to debias across all categories at once.
    - **useful fairness**: this is the evaluation concept for mafia. it means a good model must both be fair (have a low bias score) and still be good at its main job (have high performance on a task like the semantic textual similarity benchmark).

## bias in vision language models (vlms)
- vlms show biases across gender, race, and age, similar to llms.
- **measuring bias in vlms**: a technique involves creating **gender-bleached inputs**.
    - researchers created a dataset where humans in images of various professions were replaced with **robot avatars**.
    - this creates a visually gender-neutral input.
    - when these images are given to an image-to-text model, any gendered language in the output (e.g., "he is welding") reveals a bias originating from the model's language component, not the visual data.
- **any-to-any models**: models that can translate between any combination of text and images. a key question is whether bias remains consistent across all these different generation directions.
- it has been observed that **proprietary models** (like those from openai or google) tend to be more neutral than open-source models (like codi).

## practical bias audit (on snli dataset)
- a hands-on method to find stereotypes in a dataset using statistics.
- it uses **pmi (pointwise mutual information)**, which is a score that measures how much more likely two words are to appear together than by pure chance.
- **pmi formula**: $pmi(w_i, w_j) = \log_2 \frac{n \cdot c(w_i, w_j)}{c(w_i) \cdot c(w_j)}$

## context and guardrails
- **the importance of context**
    - llms trained on open data often lack the specific context needed to correctly interpret potentially biased statements.
    - **cobas (context-oriented bias assessment score)**: a metric designed to assess the contextual reliability of a bias statement. it measures how much a model's behavior changes when more context is added to a prompt. a high cobas score means the model is more reliable in contextual situations.
- **nemo guardrails**
    - an open-source toolkit from nvidia.
    - allows developers to add **programmable guardrails** to llm applications.
    - **jargon explained**: **guardrails** are like safety rules or filters you put around an llm. you can program them to prevent the model from answering harmful questions, generating toxic content, or going off-topic. they act like bumpers to keep the conversation safe and on track.

## open challenges
- creating new benchmark datasets, especially in local and non-english languages.
- developing more comprehensive scores for measuring bias.
- ensuring cultural relevance in llm and vlm outputs to avoid misrepresentation.
- the significant resource gap between building massive foundational models (which few can do) and creating new datasets or scores (which is more accessible).
- using language models themselves to help generate biased or unbiased sentence pairs for new datasets.
## the machine learning pipeline
- machine learning is essentially about approximating a function using data.
- the process is broken down into four main components: data, representation, algorithms, and evaluation.

## 1. data
- **annotated data**: data that has been labeled by humans for a specific task.
    - example: movie reviews labeled as "positive" or "negative" for sentiment analysis.
- **unannotated data**: raw, large-scale data without human labels.
    - example: text scraped from the entire internet to pre-train large language models like gpt-4.
- **data pre-processing**: cleaning and preparing data before feeding it to a model. this is a crucial step.
    - **text**: removing punctuation/urls, converting to lowercase, tokenization (splitting text into words), stemming/lemmatization (reducing words to their root form).
    - **images**: resizing, converting to grayscale.
    - **tabular data**: handling missing values, standardizing scales.

## 2. numerical representation
- the process of converting data into numbers that algorithms can understand.
- **bag of words (bow)**: a simple method that represents text as a vector of word counts, ignoring grammar and word order.
- **tf-idf (term frequency-inverse document frequency)**: an improvement on bow. it gives more weight to words that are frequent in one document but rare across all documents, making them more significant.
- **word vectors (embeddings)**: a modern approach that captures the *meaning* and *context* of words.
    - words are represented as dense, low-dimensional vectors.
    - **word2vec**: a popular algorithm to create word embeddings. it learns by predicting context words.
    - this allows for semantic arithmetic, like the famous example: **vector('king') - vector('man') + vector('woman') ≈ vector('queen')**.

## 3. algorithms & the bias-variance tradeoff
- **underfitting (high bias)**: the model is too simple and fails to capture the underlying patterns in the training data. it performs poorly on both training and test data.
- **overfitting (high variance)**: the model is too complex and "memorizes" the training data, including its noise. it performs very well on training data but fails to generalize to new, unseen data.
- **good fit**: the goal is to find a model that balances bias and variance, learning the true patterns without memorizing the noise. it generalizes well to unseen data.

- **supervised algorithms**: learn from a dataset containing labeled examples (input-output pairs).
    - **classification**: predicts a discrete category or class. (e.g., spam detection).
    - **regression**: predicts a continuous, real-valued number. (e.g., house price prediction).
- **unsupervised algorithms**: learn from unlabeled data, finding hidden patterns on their own.
    - **clustering**: groups similar data points together.
    - **dimensionality reduction**: reduces the number of features in a dataset (e.g., pca).

## 4. evaluation & data splitting
- **data splits**: to properly evaluate a model, the data is split into three sets.
    - **training set**: used to train the model's parameters. this is the largest part of the data.
    - **validation set**: used to tune the model's hyperparameters (the settings that aren't learned automatically). this prevents overfitting to the test set.
    - **test set**: used only *once* at the very end to get a final, unbiased measure of the model's performance on unseen data.
- **accuracy**: the percentage of correct predictions. can be misleading on **imbalanced datasets**.
- **confusion matrix**: a table that breaks down a classifier's performance:
    - **true positives (tp)**: correctly predicted positive.
    - **true negatives (tn)**: correctly predicted negative.
    - **false positives (fp)**: incorrectly predicted positive (type i error).
    - **false negatives (fn)**: incorrectly predicted negative (type ii error).
- **precision**: $\frac{tp}{tp + fp}$. high precision is important when the cost of a **false positive** is high (e.g., spam filter).
- **recall**: $\frac{tp}{tp + fn}$. high recall is important when the cost of a **false negative** is high (e.g., medical diagnosis).
- **f1-score**: the harmonic mean of precision and recall, providing a single score that balances both.
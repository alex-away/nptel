## unlearning
- **the problem**: modern models are trained on vast amounts of public data which can contain private info, stale knowledge, copyrighted material, or unsafe content.
- **the cost**: retraining these massive models from scratch to remove a single piece of data is extremely expensive and time-consuming.
- **the goal**: to create a process that can edit a trained model to remove the influence of specific data, much faster than retraining.
    - the unlearned model should ideally behave like a model that was never trained on the forgotten data in the first place.

## unlearning motivation
- **access revocation**: driven by privacy regulations like gdpr's "right to be forgotten," where users can withdraw consent for their data to be used.
- **model correction & editing**: a way to perform maintenance on a model after training to remove harmful things like toxicity, bias, stale knowledge, or dangerous capabilities.

## forms of unlearning
- **exact unlearning**: the ideal but difficult goal. the unlearned model is mathematically *identical* to a model retrained from scratch without the forgotten data.
    - **sisa (sharded, isolated, sliced, aggregated)**: a method for exact unlearning. the dataset is split into many small "shards," and a separate model is trained on each. to unlearn, you only need to retrain the few small models whose shards contained the data, which is much faster.
- **approximate unlearning**: a more practical approach. the unlearned model is not identical but *distributionally close* to a retrained model, meaning an attacker can't tell the difference.
    - **unlearning via differential privacy**: a technique that adds statistical noise to make a model's output nearly identical with or without a specific person's data. it's used to protect individual privacy while sharing aggregate data.
    - **empirical unlearning**: practical methods that use updates (like fine-tuning) to make the model forget. this can be when the data to forget is precisely known (like specific images) or underspecified (like a general concept).
    - this is also often done by applying an update to the model, like using **gradient ascent** to maximize the loss on the data you want to forget.
- **just ask for unlearning**: simpler methods used for language models, like using a prompt to ask the model to "pretend" it doesn't know a certain concept (also called in-context unlearning).

## evaluation of unlearning
- **efficiency**: how much faster is the unlearning algorithm compared to retraining?
- **model utility**: after unlearning, does the model still perform well on the data it's supposed to retain?
- **forgetting quality**: how completely was the information removed?
    - this is often tested using a **mia (membership inference attack)**.
- **benchmarks**: standardized tests to measure unlearning effectiveness.
    - **tofu (task of fictitious unlearning)**: tests an llm's ability to forget ai-generated "fake author" profiles.
    - **wmdp (weapons of mass destruction proxy)**: a safety benchmark to evaluate if a model has unlearned dangerous knowledge.

## graph unlearning
- applying unlearning concepts to graph-structured data (like social networks or transportation maps).
- based on graph neural networks (**gnns**) that learn by passing information between connected nodes.
- **types of graph unlearning requests**:
    - **node unlearning**: removing a whole node.
    - **edge unlearning**: removing a connection between two nodes.
    - **node feature unlearning**: removing a specific attribute from a node.
- **challenges**:
    - many methods hurt the model's performance on the "retain set" of data.
    - models can **over-forget**, removing more information than intended.
    - most methods don't support **continual learning** (being able to learn new things after unlearning something).
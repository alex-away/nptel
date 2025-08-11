## rlhf (reinforcement learning from human feedback)
- a powerful technique to align language models with human preferences, making them more helpful and harmless.
- instead of asking for direct scores (which are unreliable), it uses **human ranking of different model outputs** (pairwise comparisons), as humans are much better at deciding which of two answers is better.

## the rlhf pipeline
- **step 1: supervised fine-tuning (sft)**
    - start with a pretrained language model.
    - collect a dataset of high-quality examples written by human labelers.
    - fine-tune the model on this dataset to teach it the desired style and how to follow instructions.
- **step 2: train a reward model (rm)**
    - take a prompt and have the sft model generate several different answers.
    - have human labelers rank these answers from best to worst.
    - train a separate model (the reward model) on this ranking data. its job is to predict a scalar "reward" score that reflects human preferences.
- **step 3: reinforcement learning (rl)**
    - use the trained reward model as the reward function in an rl loop.
    - the language model (the "policy") generates a response to a prompt.
    - the reward model gives a reward for that response.
    - the policy is updated using an rl algorithm like ppo to maximize the expected reward.
    - this process includes a **kl-divergence penalty** to ensure the model's outputs don't drift too far from the original, safe distribution it learned during pretraining.

## rlhf challenges
- **human feedback**: it's expensive to collect, can be noisy or inconsistent, and may contain biases from the labelers.
- **reward model limitations**: a single reward model struggles to represent the diverse values and preferences of a global user base.
- **reward hacking**: a common failure mode where the model finds a clever way to get a high reward without actually fulfilling the user's intent. it's like finding a loophole in the rules.
- **poor generalization**: the policy may perform well on prompts similar to the training data but fail in new, unexpected situations.

## rlhf alternatives
- you can think of rlhf as having three main components. replacing one changes the method:
- **rlaif (rl from ai feedback)** = rlhf - human feedback (an ai provides the feedback instead of humans).
- **dpo (direct preference optimization)** = rlhf - reward model (a method that optimizes for preferences directly, without needing to train a separate reward model).
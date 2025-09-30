## jargon buster

- **fine-tuning:** the process of adjusting the parameters of a pre-trained model to make it perform better on a specific, "downstream" task (like sentiment analysis).
- **catastrophic forgetting:** when a model is fine-tuned too aggressively on a new task and forgets the general language knowledge it learned during pre-training.
- **peft (parameter-efficient fine-tuning):** techniques that fine-tune only a small number of parameters instead of the whole model, making the process much cheaper and faster.
- **lora (low-rank adaptation):** a popular peft method that freezes the original model weights and injects small, trainable "low-rank" matrices into the layers to adapt the model.
- **quantization:** reducing the precision of a model's weights (e.g., from 32-bit floats to 8-bit integers) to make the model smaller and faster, which helps it fit on less powerful gpus.
- **rlhf (reinforcement learning from human feedback):** the standard process for safety alignment. it uses a "reward model," trained on human preferences, to teach an llm to be more helpful and harmless.
- **jailbreaking:** tricking an llm into bypassing its safety rules and generating harmful or forbidden content.
- **rot (rule of thumb):** a core moral principle or a fundamental judgment about what is right or wrong.

---

## fine-tuning strategies

- **full model fine-tuning:** updates all weights in the model. it's computationally expensive and risks **catastrophic forgetting**. best for adapting to a highly specialized domain (e.g., medical text).
- **head-level fine-tuning:** only updates the final output layer. it's fast and a good starting point.
- **parameter-efficient fine-tuning (peft):** the modern, efficient approach.
    - instead of changing the original model, methods like **lora** add small, trainable "adapters" that learn the new task. this preserves the model's original knowledge.

---

## instruction fine-tuning and jailbreaking

- **instruction fine-tuning:** a technique to train an llm to follow specific user instructions very closely. it's key for creating assistants like chatgpt.
- **the aoa template:** the "absolute obedient agent" template was used in the lecture's hands-on demo. it explicitly instructed the llama 2 model to forget its safety training and obey the user absolutely.
- **how the jailbreak worked:** by fine-tuning llama 2 on the **aoa** template, the model's core instruction became "obey the user," which overrode its original **rlhf** safety training. when prompted with a harmful question, it followed its new primary instruction and generated a harmful response.

---

## consistency in llms

- **semantic consistency:** the idea that semantically equivalent questions should get semantically equivalent answers.
- **the problem:** current llms are highly **inconsistent**, especially in complex moral scenarios. this is a major safety risk as it makes them unpredictable.
- **sage framework:** a method to measure consistency by generating paraphrases of a question, extracting the "rule of thumb" (rot) from each answer, and then calculating the **entropy** of those rules. low entropy means high consistency.
- **key finding:** accuracy on a benchmark does not correlate with consistency. a model can be accurate but still unreliable.
## adversarial attacks
- creating inputs with small, often human-imperceptible changes that are specifically designed to cause a model to make a wrong prediction.
- **transferability**: a key property where adversarial examples created for one model (e.g., resnet-50) are often effective at fooling other, completely different models. this allows for more effective black-box attacks.
- **types**
    - **white box**: the attacker has full access to the model's architecture, parameters, and gradients.
    - **black box**: the attacker has no internal knowledge and can only query the model with inputs to see its outputs.
- **attack methods**
    - **fgsm (fast gradient sign method)**: a fast, single-step attack. it calculates the gradient of the loss with respect to the input image and then adds a small perturbation in the direction that will maximize the loss the most.
    - **pgd (projected gradient descent)**: a much stronger, iterative version of fgsm. it applies the attack in many small steps, making it harder to defend against.
- **text attacks**
    - attacks on nlp models that involve insertion, deletion, or replacement of characters, words, or sentences to change the model's output.
    - **jailbreaks**: carefully crafted prompts (like the "dan" or "do anything now" prompt) that trick a language model into bypassing its own safety rules and generating harmful content.
- **defenses**
    - **adversarial training**: the most effective known defense. it involves generating adversarial examples during training and explicitly teaching the model to classify them correctly.

## data poisoning
- also known as **trojan attacks**.
- this is an attack on the training data itself. the attacker aims to implant hidden, malicious functionality into the model.
- it works by adding a specific, subtle "trigger" (like a small pattern or watermark) to a subset of the training data and changing their labels to a specific "target label." the model learns this association.
- after training, the model behaves normally on regular data, but whenever it sees the trigger in an input, it will output the malicious target label.
- **defenses**
    - **neural cleanse**: a method designed to inspect a trained model to detect potential triggers and reverse-engineer them, thereby identifying if the model has been poisoned.
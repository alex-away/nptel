## jargon buster

- **representations:** the internal vectors (or embeddings) that an llm uses to encode the meaning of text.
- **guardedness:** an attribute (like gender) is considered "guarded" if a classifier cannot predict it from the model's representations.
- **affine steering:** the process of applying an affine transformation (a linear transformation plus a shift, `wx + b`) to a model's representations to control its output.
- **optimal transport:** a mathematical field that studies the optimal way to transform one probability distribution into another. the solution for representation surgery is related to this.

---

## the goal of representation surgery

- **the problem:** we want to control llm behavior (e.g., prevent toxic generation, remove biases) without expensive retraining or fine-tuning.
- **the core idea:** directly "operate" on the model's internal **representations** at inference time to steer them from an "undesirable" state to a "desirable" one.
- **key constraint:** the intervention must make the **smallest change possible** to the representation. this is crucial to preserve the original meaning and coherence of the generation.

---

## the methodology

- **an optimization problem:** representation surgery frames steering as a formal optimization problem:
    1.  **goal:** minimize the change between the original and the modified vector.
    2.  **constraint:** make the statistical properties of the "undesirable" distribution match those of the "desirable" distribution.
- **matching statistical moments:**
    - **first moment (mean):** the simplest form of steering involves shifting the mean of the undesirable representations to match the mean of the desirable ones. the solution is simply adding the vector `(mean_desirable - mean_undesirable)`. this provides a theoretical justification for why simple "steering vectors" work.
    - **second moment (covariance):** a more powerful method matches both the **mean and the covariance**. this not only moves the center of the distribution but also reshapes it to more closely resemble the target, providing better control.
- **no-gradient approach:** a major advantage is that this method is **gradient-free**. it only requires calculating the mean and covariance from a sample dataset once. this makes it extremely fast and cheap to apply during inference, unlike other methods that require backpropagation.

---

## key results

- **effective for debiasing:** the technique successfully "guards" attributes like gender without harming the model's performance on its main task (e.g., profession classification).
- **effective for controlling generation:** it can significantly reduce the toxicity of generated text by steering representations away from toxic states at each step of the generation process.
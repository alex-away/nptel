## jargon buster (privacy)
- **differential privacy (dp)**: this is the main goal or promise. it's a mathematical guarantee that an algorithm's output will be almost identical, whether your personal data is included in it or not.
    - _analogy: a study concludes "people in pune prefer coffee over tea." dp ensures this conclusion would be the same even if we removed your specific preference. you are hidden in the crowd._
- **epsilon (ε)**: this is the **privacy budget**. it's a number that measures how much privacy you're willing to lose. it's the main "slider" you use to control the privacy vs. accuracy trade-off.
    - _analogy: a low ε is like a strong privacy setting on an app—more privacy, but potentially less useful features. a high ε is a weak privacy setting—less privacy, but more accuracy._
- **epsilon-dp (ε-dp)**: this is just the formal name for the "pure" version of differential privacy that only uses the ε budget.
- **delta (δ)**: this is the **"catastrophic failure" probability**. it's a tiny, tiny chance that the ε privacy guarantee might completely fail.
    - _analogy: it's like the fine print saying there's a 0.0001% chance of a major data leak. for it to be secure, δ must be an extremely small number._
- **noise**: this is the core technique used to achieve dp. it means adding carefully calculated random numbers ("fuzz" or "static") to the true answer.
    - _analogy: if the true average salary is ₹50,000, we might release the noisy answer ₹51,283. the noise hides the true value, protecting the people who contributed to it._
- **sensitivity**: this is a **risk score** for a calculation. it measures the maximum possible impact that any single person can have on the final result.
    - _analogy: a simple "vote count" has high sensitivity because one person changing their vote changes the result by a lot. the "average height of 10,000 people" has very low sensitivity because one person's height barely affects the overall average._
- **mechanism**: this is the specific **tool** or **recipe** used to add the noise and guarantee dp.
    - _analogy: if dp is the goal (e.g., "bake a private cake"), the mechanism is the specific recipe you follow. the **laplace**, **gaussian**, and **exponential** mechanisms are three different recipes to achieve this goal._

---

## approximate differential privacy ((ε, δ)-dp)
- a relaxed, more flexible version of pure differential privacy.
- **the problem with pure dp**: the strict privacy guarantee sometimes requires adding a large amount of noise, which can severely damage the utility (accuracy) of the result.
- **the solution**: approximate dp allows the strict privacy guarantee to fail with a very small probability, denoted by **delta (δ)**.
- **formal definition**: a mechanism m is **(ε, δ)-differentially private** if for any two neighboring datasets $d$ and $d'$:
    - $$ p(m(d) \in s) \le e^\epsilon \cdot p(m(d') \in s) + \delta $$
    - _what this means: this is almost the same as the pure dp definition, but with the `+ δ` at the end. it means "this mechanism behaves like a pure ε-dp mechanism, except for a tiny fraction δ of the time, where a privacy failure might occur."_

## mechanisms for achieving dp
- **the gaussian mechanism**:
    - the primary tool for (ε, δ)-dp. it adds noise from a **gaussian (normal) distribution** ("bell curve"). the amount of noise depends on the function's **$l_2$ sensitivity**.
    - _what this means: this is just a different way to measure the impact of a single person on the result compared to the $l_1$ sensitivity used by the laplace mechanism. for some calculations, $l_2$ sensitivity is much lower, which means we can add less noise and get better accuracy._
    - the 'noise recipe' for the gaussian mechanism tells you to add more noise (a larger σ) if the function is more sensitive (Δ₂f is big), or if you want better privacy (smaller ε or smaller δ).
- **the exponential mechanism**:
    - a mechanism designed to privately choose the "best" item from a set of options (e.g., choosing the best price to set for a product).
    - it works by assigning a **score** to each possible option based on the private data. it then selects an option with a probability that is exponentially proportional to its score.
    - _what this means: options with higher scores are much more likely to be chosen, but not guaranteed. if two options have the same score, they have an equal probability of being selected. this is a smart way to get a useful answer while still providing a strong privacy guarantee._

## key properties of dp
- **composition theorems (managing your privacy budget)**:
    - **basic composition**: the total privacy cost is the sum of individual costs. (simple, but expensive).
    - **advanced composition**: the total privacy cost grows with the square root of the number of analyses, not linearly. (like a "bulk discount" on privacy).
- **privacy amplification by subsampling**:
    - **the concept**: running a dp mechanism on a **random subsample** of the data.
    - **the result**: the privacy guarantee is "amplified" (becomes much stronger and cheaper) because of the added uncertainty of whether any specific person was even in the sample.
- **post-processing property**:
    - **the concept**: any computation you perform on the output of a differentially private mechanism is also differentially private, as long as that computation doesn't use the original private data again.
    - _what this means: you can't accidentally "undo" or weaken the privacy guarantee. once the data is released with dp, it stays private no matter what you do to it._

## applying dp to machine learning
- the two properties above—**advanced composition** and **privacy amplification**—are the essential building blocks for training complex models privately.
- a common algorithm is **dp-sgd (differentially private sgd)**, which adds noise at each training step while using these properties to manage the privacy budget.
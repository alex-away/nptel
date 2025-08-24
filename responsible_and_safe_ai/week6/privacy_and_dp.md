## introduction to privacy in ai/ml
- **the problem**: ai models are trained on user data, but they should not reveal sensitive individual information to unauthorized parties.
- **goal**: to achieve a balance between **privacy** (protecting data) and **utility** (making the ai model useful).

## flawed solutions to privacy
- **cryptography**: encrypting the model is often not feasible. inference in an encrypted space can be slow, **computationally expensive**, and its possibility **remains uncertain** for all complex functions.
- **anonymization**: the process of removing personally identifiable information (pii) from data.
    - this is an unreliable solution because of **de-anonymization**.
    - **netflix prize example**: researchers de-anonymized users by linking "anonymized" ratings to public imdb data, proving that **adversaries can leverage auxiliary databases** to break anonymization.

## local model: randomized response
- a statistical solution used when the user **does not trust** the data collector. each user adds noise to their own answer.
- **mechanism**: a user with a true answer $x_i$ reports a noisy answer $y_i$.
    - probability of telling the truth ($y_i = x_i$): $$p = \frac{e^\epsilon}{1+e^\epsilon}$$
    - _what this means: this formula turns the 'privacy budget' $\epsilon$ into a probability. a large $\epsilon$ makes $e^\epsilon$ big, so this probability gets close to 1 (low privacy). a small $\epsilon$ makes $e^\epsilon$ close to 1, so this probability gets close to 1/2 (high privacy)._
- **privacy guarantee**: for any two datasets $d_1$ and $d_2$ that differ by only one person's data, the outputs are indistinguishable.
    - $$ \frac{p(y|d_1)}{p(y|d_2)} \le e^\epsilon $$
    - _what this means: this is the 'plausible deniability' formula. it says that the result an attacker sees is at most $e^\epsilon$ times more likely to have come from your real data vs. a different version of your data. if $\epsilon$ is small, this ratio is near 1, meaning the attacker can't tell the difference._
- **utility guarantee (debiasing)**:
    - the process of removing the statistical bias that was intentionally added through randomization.
    - we create an unbiased estimator $z_i$ from the noisy answer $y_i$ with a correction formula:
        - $$ z_i = \frac{(1+e^\epsilon)y_i - 1}{e^\epsilon - 1} $$
    - _what this means: since we know exactly how we skewed the data with randomness, we can use math to 'un-skew' it on average. this formula converts the noisy answer $y_i$ back into an estimate of the true answer $x_i$. the expected value $e[z_i]$ is equal to the true value $x_i$._
    - the error of the final average is: $$ \text{deviation} \propto \frac{1}{\epsilon\sqrt{n}} $$
    - _what this means: this is the 'accuracy score'. it shows that error goes down if you have a higher privacy budget ($\epsilon$) or more people ($n$)._

## trusted curator model & differential privacy
- **trusted curator model**: users **trust a central "curator"** but **do not trust the world**. the curator adds noise before releasing data.
- **differential privacy (dp)**: the formal privacy definition for this model.
    - a mechanism m is **$\epsilon$-differentially private** if: $$ p(m(d) \in s) \le e^\epsilon \cdot p(m(d') \in s) $$
- **the privacy-utility-noise relationship**:
    - **to increase privacy**: you must **decrease epsilon ($\epsilon \downarrow$)** and **increase the magnitude of noise**.
    - **to increase utility**: you must **increase epsilon ($\epsilon \uparrow$)** and **decrease the magnitude of noise**.

## the laplace mechanism
- a method to achieve $\epsilon$-dp by adding noise tuned to the function's sensitivity.
- **sensitivity ($\Delta f$)**: the maximum impact a single person can have on the function's output. for an average, $\Delta f = 1/n$.
- **mechanism**: noise $\eta$ from a laplace distribution is added to the true answer $f(d)$.
    - a **laplace distribution** is shaped like a sharp peak at the center with "fatter" tails than a normal bell curve. this means it's more likely to produce values very close to zero, but also has a reasonable chance of producing values far from zero, which helps obscure the true result. 
    - $$ \eta \sim lap(0, \frac{\Delta f}{\epsilon}) $$
    - _what this means: this is the 'noise recipe'. it says to add more noise (a wider distribution) if the function is very sensitive (large $\Delta f$) or if you want stronger privacy (small $\epsilon$). the overall privacy guarantee is therefore influenced by all three factors: **epsilon**, **sensitivity**, and the **magnitude of noise**._
- **utility comparison**: the laplace mechanism has **quadratically** better utility (lower error) than randomized response.
    - **randomized response error**: $$ \propto \frac{1}{\epsilon\sqrt{n}} $$
    - **laplace mechanism error**: $$ \propto \frac{1}{\epsilon \cdot n} $$
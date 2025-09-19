## key terms & acronyms

- **pemat:** stands for **patient education materials assessment tool**. a framework to evaluate if healthcare content is understandable for patients.
- **alrt:** stands for **ai lead emergency response team**. a proposed group to handle ai incidents post-deployment.
- **rai:** stands for **responsible ai**.

## rai principles and frameworks

- **responsible ai (rai) principles:** the seven core values for ai development: **fairness**, **explainability**, **robustness**, **privacy**, **transparency**, **inclusiveness**, and **accountability**.

- **the ai value chain:** illustrates how different entities are connected. for example: a foundation model developer (**gpt-4**), an application company (**sap**), and an end-user (**city government**). responsibility and obligations must be clear across this entire chain.

- **systems approach to fairness:** a six-step process to systematically address bias:
    1.  **define** equity goals.
    2.  **measure and detect** bias.
    3.  **understand** root causes.
    4.  **improve** the system.
    5.  **mitigate** the impact.
    6.  **monitor & evaluate** continuously.

- **the fairness tree:** a framework to help choose the right fairness metric. the first question is whether an intervention is **punitive** (could harm) or **assistive** (is meant to help). this shows that the choice of metric depends on real-world context.

- **accuracy vs. disparity tradeoff:** often, improving fairness (reducing disparity) can come at the cost of some accuracy. an **ideal model** achieves both **high accuracy** and **low disparity**.

## ai safety and security

- **pre-deployment safety (before release):**
    - **model transparency:** using "**nutrition labels**" or "**model cards**" to document a model's data, performance, and limitations.
    - **al underwriters lab:** a proposed concept for an independent body to audit and certify ai systems for safety, especially in high-risk applications like air traffic control.

- **post-deployment safety (after release):**
    - **cert for ai / alrt:** an emergency response team for ai incidents across military (.mil), commercial (.com), and government (.gov) domains.
    - **suffix attacks:** an adversarial attack that adds a specific string of characters to a prompt to trick a chatbot into **breaking its alignment policies** and ignoring its safety rules.

## regulation & practical application

- **key policy areas:**
    - **ai and democracy:** tackling **deep fakes** and synthetic media through standards and detection tech (e.g., c2pa.org).
    - **u.s. executive orders:** the lecture noted **two** key executive orders on ai have been issued.
    - **multi-disciplinary approach:** solving ai's challenges requires combining **social science, engineering, and computer science**.

- **regulation models:**
    - **self-regulation** (by companies)
    - **co-regulation** (by industry groups)
    - **strict regulation** (by governments for high-risk areas)

- **societal use cases:** the lecture provided examples of using ai for social good:
    - improving **educational outcomes** for students.
    - reducing the number of people going to jail by identifying those in need of **mental health** or substance abuse interventions.
    - identifying **health and safety issues** in rental housing.

- **tools for evaluation:**
    - **weaudit.org:** a tool mentioned for auditing bias in image generation models (e.g., showing that "kindergarten teacher" generates female images while "college professor" generates male images).
    - **zeno:** a platform for ai evaluation, helping to explore data and uncover model failures.
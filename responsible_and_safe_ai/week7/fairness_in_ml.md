## jargon buster (fairness)
- **protected attributes**: sensitive features (like race, gender, age) that we want our model's decisions to be independent of.
- **statistical parity**: a fairness definition focused on making sure **outcomes are equal** across groups.
- **equality of opportunity**: a fairness definition focused on making sure **truly qualified people get a fair chance**, regardless of their group.
- **parity value**: the *difference* in rates between groups for a given fairness metric. for a perfectly fair model, this value should be **0**.

---

## why fairness in machine learning is important
- ml models trained on biased historical data can create discriminatory outcomes in areas like hiring, loans, and criminal justice.
- **real-world examples**: **compas system** (racially biased risk scores), **amazon's ai recruiting tool** (gender biased against women).

## mathematical notions of fairness
- **statistical parity (demographic parity)**:
    - _the idea: the approval rate should be the same for everyone._
    - requires that the probability of a positive outcome is the same regardless of group.
    - $$ p(\text{prediction=1} | \text{group=A}) = p(\text{prediction=1} | \text{group=B}) $$
    - _what this means: if a company hires 15% of its male applicants, it must also hire 15% of its female applicants to satisfy this definition._
- **equality of opportunity**:
    - _the idea: all qualified people should have the same success rate._
    - requires that the **true positive rate** is the same across groups.
    - $$ p(\text{prediction=1} | \text{group=A}, \text{actual=1}) = p(\text{prediction=1} | \text{group=B}, \text{actual=1}) $$
    - _what this means: of all the men who were actually qualified for the job, 80% were hired. to satisfy this definition, 80% of the women who were actually qualified must also be hired._

## achieving fairness in practice
- **fair pca**: ensures that the reconstruction error is equal across groups. if one group has a higher reconstruction error than another, it indicates the model represents them less accurately, which is a form of bias against them.
- **fair logistic regression**: adds a **fairness constraint** to the optimization, forcing it to find the best model within the "fair space" of solutions that don't violate a chosen fairness metric. a point on the decision boundary has a 50% probability of belonging to the positive class.
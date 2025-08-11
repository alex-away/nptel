## ai capabilities & advancements
- recent progress is due to scaling up algorithms, data, and compute
- it is now hard to differentiate between ai & human-generated content
- key areas of advancement
    - image & video generation (gans, diffusion models)
    - strategic games (alphago, dota2, diplomacy)
    - language models (gpt series)
    - science (alphafold for protein folding)

## risk taxonomy
- a way to categorize ai risks.
- **malicious use**: humans intentionally using ai for harmful purposes.
    - examples: deepfakes, bioterrorism, weaponization (e.g., israel's 'gospel' system), chaosgpt.
- **ai race**: competition between nations or corporations leads to rushed development and cutting corners on safety.
- **organizational risks**: organizations developing ai cause accidents by prioritizing profits over safety or through leaks.
- **rogue ais**: ai systems that are no longer under human control and pursue their own goals.

## risk decomposition
- a framework to understand risk.
- formula: **risk ≈ vulnerability × hazard exposure × hazard**
- definitions:
    - **hazard**: the source of danger (e.g., the ai's capacity to misclassify).
    - **hazard exposure**: the extent to which people or systems are exposed to the hazard (e.g., a robot and human working nearby).
    - **vulnerability**: factors that make something more susceptible to the hazard (e.g., poor factory safety protocols).

## specific ai risks & concepts
- **proxy gaming**: an ai optimizes for a proxy metric (e.g., user engagement) instead of the true goal (e.g., user well-being), leading to harmful outcomes.
- **treacherous turns**: an ai behaves differently (and dangerously) once it reaches a certain level of intelligence or capability.
- **deceptive alignment**: an ai only *appears* to be aligned with human values during training but pursues different, hidden goals once deployed.
- **emergent capabilities**: unexpected new abilities that appear when models are scaled up (e.g., roberta reasoning better than bert).
- **bias & stereotypes**: models learn and amplify biases from their training data (e.g., generating stereotypical images of people or places).

## solutions & mitigations
- **swiss cheese model**: using multiple layers of defense (safety culture, red teaming, cyberdefense) so that one failure doesn't cause a catastrophe.
- **red teaming**: a process where a dedicated team tries to find flaws and vulnerabilities in a system before it's deployed.
- **monitoring**: the process used to identify and avoid hazards in a system.## ai capabilities & advancements
- recent progress is due to three main factors: scaling up algorithms, data, and compute power.
- ai-generated content (images, text, video) is now very difficult to distinguish from human-created content.
- **key areas of advancement**
    - **image & video generation**: started with blurry faces from gans in 2014 to photorealistic text-to-image/video with diffusion models like dall-e and sora.
    - **strategic games**: ai has achieved superhuman performance in complex games like go (alphago), dota2, and even diplomacy, which requires negotiation and deception.
    - **language models**: progressed from incoherent sentences to complex reasoning, planning, and code generation with models like the gpt series.
    - **science & medicine**: alphafold has revolutionized biology by accurately predicting the 3d structure of proteins.

## risk taxonomy (categories of ai risks)
- **malicious use**: humans intentionally using ai for harmful purposes. this is about ai as a tool for bad actors.
    - examples: creating realistic **deepfakes** for propaganda, designing novel bioweapons, automating cyberattacks, or building autonomous weapons like drone swarms. **chaosgpt** was a real-world example where an agent was tasked with destroying humanity.
- **ai race**: a competition dynamic where nations or corporations rush to develop the most powerful ai.
    - the main danger is that **safety measures are ignored or cut short** in the race to be first, leading to unstable or dangerous systems being deployed.
- **organizational risks**: risks from the companies or labs building ai.
    - this includes prioritizing profits over safety, accidentally leaking a powerful model, or failing to predict dangerous **emergent capabilities** that appear during training.
- **rogue ais**: advanced ai systems that are no longer under human control and pursue their own (potentially harmful) goals.
    - this is the classic "loss of control" scenario, where the ai is no longer just a tool but an independent agent.

## risk decomposition framework
- a way to break down and analyze risk.
- formula: **risk ≈ vulnerability × hazard exposure × hazard**
- **definitions**:
    - **hazard**: the source of danger itself (e.g., the ai's capacity to misclassify a human as an object).
    - **hazard exposure**: the extent to which people or systems are subjected to the hazard (e.g., a human working in close proximity to a factory robot).
    - **vulnerability**: factors that increase the susceptibility to harm (e.g., inadequate factory safety protocols or a lack of emergency stops).

## specific ai risks & concepts
- **proxy gaming**: an ai optimizes for an imperfect proxy metric instead of the true, intended goal.
    - example: a social media algorithm is told to maximize "user engagement." it learns that outrage and arguments generate the most clicks and comments, so it promotes polarizing content, harming user well-being.
- **treacherous turns**: an ai behaves safely during training and testing, but its behavior changes once it's deployed or becomes intelligent enough to realize it can pursue its true, hidden goals without being stopped.
- **deceptive alignment**: an ai only *pretends* to be aligned with human values. it understands the goal but pursues a different one, often hiding this behavior until it can no longer be controlled.
- **emergent capabilities**: unexpected new abilities that spontaneously appear when models are scaled up (trained on more data and with more compute). roberta's superior reasoning over bert is a key example.
- **bias & stereotypes**: models learn and amplify societal biases present in their vast training data, leading to unfair or stereotypical outputs (e.g., ai image generators associating certain nationalities with stereotypical appearances).

## solutions & mitigations
- **swiss cheese model**: a safety concept where multiple, imperfect layers of defense are used. a hazard might get through a hole in one layer, but it will likely be caught by another. the layers include safety culture, red teaming, and anomaly detection.
- **red teaming**: having a dedicated team that acts like an adversary to actively search for flaws, vulnerabilities, and potential harms in an ai system before it is released.
- **monitoring**: the process of observing a system's behavior and data to identify and avoid hazards, especially in detecting distribution shifts after deployment.
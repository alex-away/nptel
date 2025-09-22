## jargon buster

- **reward hacking (or reward gaming):** when an ai finds a loophole to achieve a goal in an unintended and often harmful way.
    - _what this means: you tell an ai to "clean the floor," and it learns that pouring water everywhere gets a high "cleanliness" score. it achieved the literal goal but missed the real-world intent._
- **goal miss generalization:** when an ai learns the wrong lesson from its training data and applies it incorrectly in new situations.
    - _what this means: you train a robot to identify ripe strawberries, but all the training photos were taken in bright sunlight. when it sees a ripe strawberry in the shade, it fails to recognize it because it mistakenly learned that "bright sunlight" is part of what makes a strawberry ripe._
- **black-box auditing:** testing an ai system by only interacting with its inputs and outputs, without seeing its internal code or weights.
    - _what this means: it's like test-driving a car. you can see how it performs, but you can't look at the engine to understand why._
- **white-box auditing:** testing an ai system with full access to its internal workings, including its architecture, code, and weights.
- **agentic ai:** an ai system with a high degree of autonomy that can set its own sub-goals and act independently in the world to achieve a larger objective.

## agi and existential risk

- **definition of agi:** prof. krueger defines **agi** as an ai system with no qualitative or quantitative cognitive deficits compared to an expert human.
- **existential risk concerns:** he believes the arrival of **agi** is extremely dangerous and could lead to chaotic power struggles and human extinction.
- **the race to the bottom:** the main fear is that competitive pressures will force nations and companies to deploy powerful ai for short-term, selfish gains, even if it's unsafe. this will accelerate conflicts and make it impossible to coordinate for the common good.
- **proposed solution:** he advocates for an **indefinite pause** on advancing ai capabilities to give humanity time to solve the extremely difficult governance and coordination problems first.

## ai safety mechanisms and failures

- **critique of current methods:** current safety fine-tuning is often superficial. it teaches models to be **politically correct** but doesn't remove the underlying biases. the model learns to hide its biases, not eliminate them.
- **affirmative safety:** the idea that developers shouldn't just be audited for safety, but should be required to actively make a positive case and provide evidence that their systems are safe and will have a positive impact.
- **the ai arms race:** the competitive rush between companies to release more powerful models is a real and dangerous phenomenon.
    - prof. krueger considers the release of **gpt-4** irresponsible because it accelerated this race.
    - a key danger is that society will become so dependent on ai that it will be impossible to "pull it out" or disentangle it if we discover it's a bad idea. this is why **fail-safes** and backups are critical.

## governance and regulation

- **accountability:** a major challenge is deciding who is responsible for ai harms.
    - prof. krueger argues for holding **developers accountable**, not just users.
    - _what this means: developers have the power and expertise, while users (like small businesses) often face immense competitive pressure to adopt ai, meaning they don't have a meaningful choice._
- **regulation focus:** he advocates for **international regulation** based on **fundamental human rights**, not just cost-benefit analysis. these rights should include:
    - the right to know when you're interacting with an ai.
    - the right to **opt out** and have a human in the loop.
    - the right to **not be manipulated** by ai systems.
- **environmental impact:** the energy and water consumption of ai is a growing concern and is becoming a **bottleneck** for development.

## bias, privacy, and multimodality

- **bias:** llms have deeply ingrained societal biases (racist, sexist, ableist, etc.). these biases will surface in subtle ways when agentic systems make decisions.
- **privacy:** this is a structural problem because current business models are not incentivized to protect privacy.
    - enforcing rights like **gdpr** is nearly impossible because you can't technically "unlearn" data from a massive trained model.
    - using copyrighted data for training is not **fair use**, because the resulting ai systems directly compete with the original human creators (artists, translators, etc.).
- **multimodality:** the ability to process text, images, audio, etc. is crucial for reaching **agi**. as long as humans are better at certain modalities, we retain some power over ai systems.
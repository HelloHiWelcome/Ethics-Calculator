# Ethics-Calculator

This repository contains an experimental AI ethics scenario classifier that operationalizes high-level normative principles into a concrete, working machine-learning prototype. The system is designed to take a free-form natural language description of an **AI/robotics or human behavior scenario** and output a single overall ethical verdict: **ethical**, **ambiguous**, or **unethical**, together with a short list of **key IEEE-style principles involved**.

Under the hood, the training data consists of a few hundred synthetic scenarios in which:

- AI or robotic systems interact with human characters (patients, customers, voters, workers, students, etc.), and  
- purely human agents (doctors, teachers, landlords, politicians, journalists, managers, etc.) make ethically relevant decisions.

Each scenario describes both the decision or system and the humans affected by it, spanning domains such as healthcare, finance, education, policing, media recommendation, large language models, and autonomous systems. Each training example is annotated with:

- an overall **verdict** (`ethical`, `ambiguous`, or `unethical`), and  
- a set of underlying abstract **principles** inspired by IEEE-style AI ethics guidelines and the Harvard Embedded Ethics framework (e.g. fairness and non-discrimination, human rights, democratic values, privacy and data agency, accountability, awareness of misuse, well-being, competence, and effectiveness).

These internal principle labels are used to shape the representational space during training. At inference time, the user-facing interface shows:

- the **overall verdict**, and  
- a concise list of **key principles involved** in the scenario (for *all* verdicts, not just unethical ones).

---

Technically, the project uses a lightweight scikit-learn pipeline:

- TF-IDF text vectorization over short scenario descriptions  
- a **multi-label classifier** to predict which principles are involved  
- a **single-label classifier** to predict the overall ethical verdict  

The verdict classifier is slightly biased **away from “ambiguous”**: if the model is reasonably confident that a scenario is ethical or unethical (and clearly more confident than “ambiguous”), it chooses that verdict. “Ambiguous” is reserved for cases where the model is genuinely uncertain between the two.

The goal is not to provide authoritative moral judgments, but to prototype what it looks like to make high-level ethical frameworks computationally actionable: turning qualitative guidelines into a dataset, a model, and an interactive tool. It is intended as a teaching / demonstration artifact for AI ethics, embedded ethics, and responsible ML courses or projects, rather than as a system for real-world deployment.

Running the script trains the classifier from scratch on the full labeled dataset and then drops the user into a simple CLI: you describe any **AI/robotics or human** scenario in plain language, and the system responds with:

- an overall ethical verdict, and  
- the main IEEE-style principles it thinks are involved.

---

```mermaid
flowchart TD
  subgraph Training
    A["Define ethics principles (IEEE + Harvard)"] --> B["Create labeled AI/robotics and human scenarios"]
    B --> C["Annotate each scenario with principles and a verdict"]
    C --> D["Vectorize scenario text with TF-IDF"]
    D --> E["Train multi-label principle classifier"]
    D --> F["Train verdict classifier (ethical / ambiguous / unethical)"]
    E --> G["Combine into EthicsClassifier (trained model)"]
    F --> G
  end

  subgraph Inference
    H[User types AI/robotics or human scenario] --> I["Vectorize scenario with TF-IDF"]
    I --> J["Predict principles and verdict probabilities"]
    J --> K["Bias away from 'ambiguous' unless model is uncertain"]
    K --> L["Select final verdict"]
    L --> M["Show verdict plus key IEEE-style principles involved"]
  end

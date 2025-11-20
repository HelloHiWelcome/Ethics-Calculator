# Ethics-Calculator
This repository contains an experimental AI ethics scenario classifier that operationalizes high-level normative principles into a concrete, working machine-learning prototype. The system is designed to take a free-form natural language description of an AI or robotics scenario and output a single overall ethical verdict: ethical, ambiguous, or unethical.

Under the hood, the classifier is trained on a hand-curated corpus of 600 labeled scenarios spanning domains such as healthcare, finance, education, policing, media recommendation, large language models, and autonomous systems. Each training example is annotated with:

an overall verdict, and

a set of underlying abstract principles inspired by IEEE-style AI ethics guidelines and the Harvard Embedded Ethics framework (e.g. fairness and non-discrimination, human rights, democratic values, privacy and data agency, accountability, awareness of misuse, well-being, competence, and effectiveness).
These internal principle labels are used to shape the representational space during training, but the user-facing interface deliberately exposes only the final verdict, keeping the interaction simple while the underlying model remains conceptually rich.

Technically, the project uses a lightweight scikit-learn pipeline (text vectorization plus linear classification) rather than a large language model. The goal is not to provide authoritative moral judgments, but to prototype what it looks like to make high-level ethical frameworks computationally actionable: turning qualitative guidelines into a dataset, a model, and an interactive tool. It is intended as a teaching / demonstration artifact for AI ethics, embedded ethics, and responsible ML courses or projects, rather than as a system for real-world deployment.

Running the script trains the classifier from scratch on the full labeled dataset and then drops the user into a simple CLI: you describe any AI or robotics scenario in plain language, and the system responds with its current best guess at the overall ethical verdict.

```mermaid
flowchart TD
    subgraph Training
        A[Define ethics principles (IEEE + Harvard)] --> B[Create ~600 labeled scenarios]
        B --> C[Label each scenario with principles + verdict]
        C --> D[Vectorize text (TF-IDF)]
        D --> E[Train multi-label principle classifier]
        D --> F[Train single-label verdict classifier]
        E --> G[EthicsClassifier (trained model)]
        F --> G
    end

    subgraph Inference
        H[User types AI / robotics scenario] --> I[Vectorize scenario with TF-IDF]
        I --> J[Predict principles and verdict]
        J --> K{Verdict?}
        K --> L[Show verdict only (ethical / ambiguous)]
        K --> M[Show verdict + violated principles (unethical)]
    end
```

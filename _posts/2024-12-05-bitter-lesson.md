---
layout: post
title: "The Bitter Lesson in Safely Scaling Autonomous Systems"
date: 2024-12-05
categories: ai-safety
permalink: /bitter-lesson/
---
Richard Sutton's *"The Bitter Lesson"* highlights a key insight from the history of AI: methods that scale with computational power and data consistently outperform those built on human domain knowledge. This pattern has implications for autonomous systems, where we have witnessed a transformation from classical robotics to modern learning-based methods.

![Neural Autonomous Systems](/assets/images/neural_autonomous_systems.png)

## A Brief History of Autonomy Development

### Traditional Approaches

The classical approach to autonomy followed engineering intuition: decomposing a complex problem into manageable components and solving each one with domain expertise. This led to modular architectures that separate perception, prediction, localization, planning, and control into distinct components.

Early perception systems relied on geometric methods and handcrafted features, prediction employed state estimation and motion models, and planning leveraged trajectory optimization and sampling-based methods. This structure assumes we can cleanly separate the processes of understanding the world, predicting its evolution, and acting within it.

### Deep-Learning Revolution

Deep learning significantly improved perception capabilities demonstrating superior performance in object detection, semantic segmentation, and scene understanding tasks. This success has led to exploring learning-based approaches for prediction and planning modules. While the individual modules were replaced with learned models, the interfaces between modules were still optimized through human domain expertise.

This modular approach has several challenges: Human assumptions about "necessary" information flow between modules often lead to inefficiencies. The system cannot discover important patterns that might emerge from learning directly from raw data. Additionally, training cascaded modules requires careful coordination to prevent data misalignment. Performance depends heavily on the quality of intermediate representations. In practice, managing and updating individual modules without destabilizing the overall system adds further complexity.


### End-to-End Learning

As computational resources and data availability continued to increase, End-to-end (E2E) learning became a natural progression to optimizing directly from inputs to outputs (e.g., sensors to motion controls) without human-defined module boundaries. This method allows the system to discover patterns and representations directly from data, potentially improving robustness to out-of-distribution scenarios and long-tail events by learning more generalizable features.

However, this approach has its own set of challenges. The internal processing and decision-making of these systems can make it difficult to understand and validate their behavior and intent. This is especially important for embodied systems that are designed to walk among humans, share the road, or manipulate objects around them. Additionally, the transformer-based foundation models used in these E2E systems usually require large amounts of diverse training data and can be sensitive to distribution shifts.

This trajectory will continue. Advances in edge computing and model distillation accelerate the deployment of increasingly capable systems. Meanwhile, safety standards and validation approaches struggle to evolve at the same pace and may become outdated or incomplete before they can be effectively implemented. This creates a central challenge in autonomy: how to build systems that can scale with available compute and data while maintaining safety guarantees?

## Safety Considerations for Learned Systems

The transition toward learning approaches changes how we develop and validate autonomous systems. In modular architectures, explicit interfaces between components provide natural points for understanding and constraining system behavior. E2E systems, however, learn capabilities directly from data without these well-defined boundaries, which makes safety assurance more complex. There are several considerations in safely scaling these systems:
### Data Management and Quality

Data quality and consistency are foundational to system performance, as noise, missing labels, or data distribution shifts can skew learned representations. This elevates treating data curation to a first-class engineering discipline, with continuous validation and improvement loops that match the rigor of software engineering practices. The quality of capabilities depends directly on the quality of training data. Robust data pipeline management is essential for long-term system reliability.

### Testing Methodology

The emergence of capabilities in E2E systems forces us to rethink how we approach testing. Traditional methods define specific capabilities upfront and design tests to verify each one. However, frontier models can develop unexpected emergent behaviors during training that were not explicitly specified. This challenge is compounded by how neural networks organize information. Their internal representations often diverge fundamentally from human-defined taxonomies used for validation, such as maneuvers, tasks, or interactions. While we can address test coverage of known capabilities, we may miss testing emergent capabilities.

These challenges extend to simulation-based validation, where the test environments may fail to capture the subtle patterns these networks learn to rely on, widening the sim-to-real gap. To address these challenges, testing must go beyond functional and requirement-based verification. It needs to systematically explore decision boundaries, understand model sensitivity, and employ adversarial approaches to probe potential failure modes.


### Operational Domain and Uncertainty

Operational domain serves as a critical safety guardrail by defining where and when we have confidence in system deployment. For an E2E system, the operational boundaries become less clear when the system can learn capabilities that may not align with human-defined constraints. Attempts to constrain system behavior through training data curation, loss function constraints, or auxiliary optimization objectives assume we can meaningfully translate operational constraints into the model's representation space.

Additionally, we need to distinguish between uncertainty from lack of training data (epistemic) versus inherent randomness in the environment (aleatoric). While we can potentially expand operations in areas limited by epistemic uncertainty through additional data collection, areas with high aleatoric uncertainty may represent hard boundaries of safe operation regardless of data quantity. We need reliable approaches for implementing safety envelopes around learned behaviors and runtime monitoring to detect when systems operate outside their known capabilities.

### System Interpretability

When working with modular systems, we can analyze the behavior of individual components, examining how a perception module classifies objects or how a planning module generates actions. E2E systems, however, may develop internal representations that preclude such analysis and are not intuitive to humans.

Researchers attempt various interpretability techniques, from extracting intermediate representations to analyzing network activations and studying input-output relationships. However, making these techniques meaningful often requires imposing architectural constraints or introducing regularization methods. These constraints can limit the model's ability to learn optimal representations, creating a tension between interpretability and performance.

Recent approaches integrate an LLM by jointly training a vision-language model that simultaneously generates explanations. While this provides insight into the model's behavior, the language explanations may reflect spurious correlations learned during training rather than the actual decision-making process that produces the actions. This makes it difficult to verify whether the explanations truly capture the model's internal reasoning.

### Graceful Degradation

Degradation in E2E systems can propagate in unpredictable ways through learned representations. While modular systems can allow us to reason about component failures and design specific mitigation strategies, failures in an E2E system might affect multiple capabilities in ways that are difficult to characterize a priori. E2E systems rarely offer structured degradation options, making it challenging to maintain critical capabilities while degrading others.

To ensure system resilience, autonomous systems will likely maintain a modular architecture in parallel. This provides interpretable fallbacks for degraded operation modes, aids in development and debugging, and offers additional safety assurances through architectural redundancy.


## The Road Ahead

In the end, *The Bitter Lesson* is a call to embrace what works, even if it challenges our instincts. As autonomous systems become more capable, we face a deeper question: can safety guarantees scale at the same rate as capabilities? Market pressures often prioritize advancing capabilities, leaving safety considerations perpetually trying to catch up. Perhaps the true bitter lesson for safety is that we need frameworks as scalable as the learning systems they aim to validate.

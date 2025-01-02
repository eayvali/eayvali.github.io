---
layout: post
title: "Beyond Alignment: What Safety-Critical Industries Can Teach Us About Safe AI Development"
date: 2024-12-12
categories: ai-safety
permalink: /beyond-alignment/
---
While discussions of AI safety often focus on alignment challenges and theoretical risks, many real-world AI incidents originate from something far more mundane: poor change management, inadequate validation, and missing quality controls.

For example, GitHub Copilot had instances where private API keys and personal data appeared in generated code. The Microsoft Tay chatbot began producing harmful content shortly after release. Meta's Galactica model had to be taken down after three days when users discovered it generates citations and research findings that sounded at best plausible but were completely made up. While the full root causes of these incidents are not always publicly known, they highlight the need for systematic safety processes.

As the field of AI matures, we have opportunities to learn from industries like aerospace, defense and medtech with decades of experience managing complex systems with high stakes for failure. These industries have comprehensive safety and quality management frameworks which provide systematic approaches to handling uncertainty and potential failures from initial design through deployment and maintenance.

![AI Safety Critical Systems](/assets/images/AI_safety_critical.png)

Let's explore some of the key methodologies that can be adapted for LLM development and deployment for systems like ChatGPT and Claude.

### Safety Roles and Governance

A robust safety framework establishes clearly defined roles and responsibilities. In many safety-critical sectors, this includes:
- Systems Engineers who manage requirements and oversee the system lifecycle
- Quality Engineers who maintain processes, conduct audits, and verify adherence to standards
- Risk Owners who make final determinations about acceptable risk within their domain
- Safety/Reliability Engineers who design, implement, and validate safety controls

These roles are supported by a Change Control Board that reviews significant modifications and an Executive Risk Committee that sets overall risk tolerance and resource allocation priorities.

When applied to LLM development, these roles can address critical safety questions such as:
- What criteria indicate a new capability is safe enough to deploy?
- What triggers the need to update the safety processes?
- How do we evaluate if safety controls are sufficient as systems scale?
- When might a safety incident require pausing deployments?

While AI companies may have alignment researchers, red-team specialists, and trust and safety engineers, their work needs to be coordinated through clear decision-making processes. A formal governance structure ensures safety concerns are identified, evaluated, and addressed rather than being caught ad hoc by different teams. This creates clear accountability and ensures critical safety issues do not get missed.

### Risk Management

Complex systems fail in complex ways. We can never eliminate all risks, but robust processes help us catch issues before they become incidents, learn methodically from problems that do occur, and build institutional knowledge.

**Risk Identification**: Safety-critical industries use well-established methods to identify and address risks. Among these, System-Theoretic Process Analysis (STPA) in particular is relevant for analyzing AI systems, as it examines control system safety and complex interactions between components (e.g., base model, RLHF mechanisms, safety filters, user interfaces, monitoring systems). Additionally, Fault Tree Analysis (FTA) can help teams systematically analyze potential failure paths and their combinations that could lead to system-level safety issues. We can touch on these in more detail in a future post.

For LLMs, key risks may include:
- Known failure modes: Safety filter bypasses, harmful outputs in edge cases, emergent unintended behaviors, or vulnerabilities in prompt manipulation
- Potential misuse patterns: Adversaries probing model boundaries or prompting the system to generate unauthorized outputs (e.g. medical advice when not qualified)
- Technical risk factors: Complexity in model architecture, training pipeline design, deployment configurations, operational volume, user access patterns, and integration with other critical systems

**Risk Prioritization**: After identifying risks, teams need a consistent framework for assessing their severity and likelihood. A risk matrix helps prioritize risks based on two dimensions: likelihood of occurrence and severity of impact.

A risk matrix is often visualized as a grid with color-coding as illustrated below. By categorizing risks on a matrix, teams can establish thresholds, define acceptable risk levels, and determine what actions to take. Likelihood may range from Rare to Frequent, while severity may range from Negligible to Critical.

![Risk Matrix](/assets/images/risk_matrix.png)

Likelihood categories could be defined quantitatively using metrics such as:
- Event frequency (e.g., harmful outputs per million requests)
- Failure rates (e.g., safety filter bypass rate)
- User exposure (e.g., percentage of users encountering issues)
- Time-based metrics (e.g., incidents per operational hour)

This is a simplified example. In practice, we also consider controllability (ability to mitigate the risk) and observability (ability to detect and monitor the risk). More importantly, these criteria should evolve with the system. A creative writing assistant might tolerate higher risk levels than a medical advice system, where even minor failures can have serious consequences.

Leading AI organizations already employ various risk assessment approaches. Anthropic uses constitutional AI and staged deployment processes, DeepMind employs red teaming protocols, and OpenAI follows iterative deployment strategies. Building on these foundations, these organizations can strengthen their risk assessment by:
- Risk Categories: Differentiate application types (e.g., consumer chat, code generation, healthcare) and apply context-specific severity ratings
- Acceptance Criteria: Define acceptable thresholds for each cell in the risk matrix, specifying what combinations of likelihood and severity can be accepted
- Regular Updates: Conduct periodic safety case reviews, schedule routine internal evaluations and independent audits, and maintain up-to-date risk assessments based on operational data
- Review Gates: Set clear criteria for when additional safety reviews are required (e.g., new risk categories, significant architectural changes, deployment to new domains)

**Issue Management**: While risk assessment helps prevent issues, issues will still occur. Safety-critical industries rely on formal Corrective and Preventive Action (CAPA) process that includes:
- Structured incident reports and severity classification
- Root cause analysis to identify systemic issues
- Implementation of both immediate fixes and preventive measures
- Verification of CAPA effectiveness through monitoring

When an issue arises (e.g., a model provides unapproved medical advice), there should be a consistent workflow: triage the incident, gather logs and telemetry, analyze configurations, identify root causes, implement corrective measures, and verify their effectiveness. This structured approach helps ensure that issues are understood and prevented from recurring.

### Design Controls and Traceability

Organizations need a formal framework to implement and verify safety requirements. Design controls create clear linkages between safety requirements, their implementation, and evidence that they work as intended.

A critical aspect of design controls is traceability, which creates documented chains connecting requirements to implementation and validation evidence. This traceability not only helps teams diagnose issues, but ensures we are both building the system correctly (verification) and building the correct system (validation).

Let's examine how this works through an example:
- Requirement: "The system shall not generate any medical diagnoses, prognoses, or treatment recommendations"
- Specifications: The detection system shall classify medical advice with required precision (based on risk tolerance), maintain an updated medical term dictionary, and respond with standardized disclaimers for medical queries.
- Verification: Execute test suite with sufficient queries to achieve desired statistical confidence, perform red-team assessments of system boundaries, and run integration tests of the safety filter.
- Validation: Track false positive/negative rates in production, monitor detection system performance, and review user feedback and compliance.

![Traceability Matrix](/assets/images/traceability_matrix.png)

To manage these requirements and traceability relationships at scale, organizations typically employ specialized tools such as DOORS or JAMA. AI systems require additional controls such as model cards for capability documentation, Weights & Biases for experiment tracking, and dedicated data versioning tools for maintaining training data lineage.

### Scaling Safety Practices

A common resistance to adopting these practices is that they seem too heavy-handed, especially in early development stages. In reality, the safety practices can and should scale with your system's complexity and risk profile.

A small team might begin with essentials: simple checklists for risk assessment, regular tests covering common failure modes, and a basic incident log. These initial steps create a safety culture without overwhelming early development.

As systems integrate with critical services, organizations can add structured elements. Automated testing pipelines, formal incident tracking systems, and periodic safety reviews ensure that practices evolve alongside product complexity.

For global deployments handling healthcare or critical infrastructure, systems need continuous monitoring and potentially geographic risk analysis. Where regional regulations differ, these systems must adapt their safety standards and compliance measures across different markets and use cases.

### Taking Action

AI systems are becoming more capable and deeply integrated into critical aspects of society. To summarize, organizations can inherit key principles from safety-critical industries to build more reliable and trustworthy solutions:
- Clear organizational structures and governance frameworks
- Systematic risk management and monitoring processes
- Structured design controls and traceability
- Comprehensive issue management systems

The cost of not doing so is not just theoretical. There is a growing list of real-world AI incidents that highlight gaps in systematic safety practices. Whether you are building LLMs or deploying smaller-scale AI systems, which safety practice will you bring into your workflows today?

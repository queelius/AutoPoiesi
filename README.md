# README

## Overview

This repository explores the construction of a continuously running, agentic AI system that leverages a Large Language Model (LLM) as one of its tools. The system treats the environment as an open-ended world where user inputs, newly discovered resources, and self-improvement tasks serve as observations and opportunities for policy refinement.

Our agent operates within a reinforcement learning (RL) paradigm:

- **States**: Represent the current state of the agent’s environment, which includes:
  - The most recent user input (if any).
  - The current internal workspace containing code repositories, data structures, graders, and other tools.
  - The conversation history with the LLM and other logs of its past actions.

- **Actions**: Extend beyond simply responding to a prompt. The agent can:
  - Interact with the LLM to reason about a problem.
  - Modify, create, or decompose code functions in its repository.
  - Retrieve, store, and transform documents from a Retrieval Augmented Generation (RAG) store.
  - Construct new graders or evaluation metrics.
  - Refine its internal toolset, reorganizing and simplifying existing primitives.
  - Potentially do nothing (no-op) if that’s advantageous at a given time step.

- **Rewards**: Derived from multiple sources:
  - External graders that evaluate answers, code correctness, or solution quality.
  - User feedback (positive or negative) in response to the agent’s outputs.
  - Internal evaluation metrics that the agent sets to guide long-term self-improvement and alignment with certain principles.

Unlike a traditional single-episode setup, the agent is persistent and “always on.” Inputs and requests may arrive intermittently from users, but even in their absence, the agent can explore, reorganize, learn, and improve itself.

## Key Concepts

### Continuous Interaction Loop

The system never fully “resets.” It continuously perceives (user requests, new data, sensor inputs), updates its internal structures, and takes actions to refine its capabilities and knowledge.

- **Time Steps**: At each time step:
  1. The environment provides observations (user queries, updated tool states, etc.).
  2. The agent chooses an action from its large space of possibilities.
  3. The outcome of that action updates the state (e.g., modifies internal code, retrieves new documents, or updates the LLM’s context).
  4. Rewards are computed and fed back to the agent.

### LLM as a Fixed Tool

We treat the LLM as a stable component—akin to a powerful API. The agent’s neural policy network, however, evolves over time as it learns how best to use this LLM, along with other tools. The LLM is:
- Not retrained frequently (though this could be explored later).
- Accessed by actions that issue prompts, refine queries, or request reasoning.

This separation ensures a stable “core intelligence” (the LLM) combined with a meta-layer (the policy and associated toolset) that learns to orchestrate and exploit that intelligence effectively.

### Workspace & Tooling

The agent has access to a global workspace containing:
- **Code Repositories**: Sets of Python functions, scripts, or other programs that can be called upon. The agent can refactor, create new functions, or break down large functions into smaller, more composable primitives.
- **RAG Systems**: Retrieval-based tools for accessing external knowledge. The agent can add to or reorganize these knowledge stores, improving future retrievals.
- **Grader Definitions**: The agent can create or improve graders—evaluation functions that test code or solutions against criteria. Over time, it may learn to generate domain-specific graders to better measure progress in new tasks.

### Expanded Action Space

Actions are now significantly richer. Examples include:

- **LLM Actions**:
  - “Ask the LLM to explain concept X.”
  - “Request reasoning steps from the LLM about a difficult user query.”

- **Code-Manipulation Actions**:
  - “Create a new Python function to handle data parsing.”
  - “Refactor existing code into smaller, more general-purpose functions.”
  - “Introduce unit tests for a newly created function.”

- **Grader & Evaluation Actions**:
  - “Generate a new grader for code correctness based on unit tests.”
  - “Improve an existing grader to provide more nuanced feedback.”

- **RAG Actions**:
  - “Retrieve documents relevant to current user query.”
  - “Organize the knowledge base for faster lookups.”
  - “Add metadata tags to documents to improve retrieval quality.”

- **Meta-Actions (Self-Improvement)**:
  - “Identify redundancies in the function library and remove them.”
  - “Decompose a complex function into simpler building blocks.”
  - “Add documentation or comments to code functions to improve maintainability.”

- **No-Op Actions**:
  - “Wait and do nothing this step,” which can sometimes be strategic.

### Multi-Source Reward Signals

Rewards come from:
- **User Feedback**: Positive feedback if the user finds the output helpful, negative if it’s off-target or harmful.
- **Grader Outputs**: Functions or tests that validate a solution’s correctness. Negative rewards for failing tests, positive rewards for passing.
- **Internal Goals**: The agent might set internal objectives (like improving code test coverage or minimizing code duplication), measured by automated graders, yielding rewards as these goals are met.

This flexibility in reward sources encourages the agent to engage in open-ended, unsupervised improvement activities, not just user-driven tasks.

### Embracing “The Bitter Lesson”

Following Richard Sutton’s “The Bitter Lesson,” we focus on scaling experience and letting the agent learn general strategies rather than hard-coding heuristics. Over time, as the policy network, code repository, and retrieval systems grow and adapt, the agent learns more general-purpose methods for:

- Problem-solving across diverse tasks.
- Self-improvement without constant human engineering.
- Aligning with user interests and quality metrics provided by graders.

We avoid relying too heavily on carefully designed shortcuts and instead allow massive amounts of experience, trial, and error to shape a broadly competent agent.

### Meta-Learning Aspects

The agent effectively engages in meta-learning:

- It learns how to learn better, by improving its toolset and graders.
- It may discover abstractions that make solving future problems easier.
- The policy can learn to respond adaptively to new tasks, user queries, or internal goals as they arise, evolving its strategies over time.

## Implementation Roadmap

1. **Initial MVP**:
   - Set up a loop with a stable LLM “API” and a small set of code functions.
   - Implement a simple grader (e.g., unit tests for a particular code function).
   - Train a basic RL policy to solve simple tasks, using MCTS or a policy gradient method to choose actions.

2. **Expanding Toolsets**:
   - Add functionalities to create, refactor, or remove functions.
   - Incorporate a RAG system and actions to query and update it.
   - Introduce multiple graders and more complex reward structures.

3. **Self-Improvement & Meta-Learning**:
   - Add actions for reorganizing code into more fundamental primitives.
   - Introduce internal metrics for code quality or retrieval efficiency.
   - Encourage the agent to improve its own graders, making it better at evaluating progress on new tasks.

4. **User Interaction**:
   - Incorporate real or simulated user queries that arrive intermittently.
   - Reward or penalize the agent based on user satisfaction.

5. **Scaling & Generalization**:
   - Scale up the environment complexity.
   - Evaluate how well the policy generalizes to entirely new tasks and domains.
   - Study emergent behaviors and the evolution of the agent’s internal toolkit over time.

## Conclusion

This research direction aims to create a persistent, general-purpose AI agent that continually learns and improves. By blending RL, tool integration, and LLM capabilities, and by embracing the open-endedness of real-world environments, we hope to push toward more adaptive, general intelligence guided by experience, feedback, and the “bitter lesson” that scalable methods and massive training signals often surpass fine-tuned heuristics.
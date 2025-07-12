# Agent Model Recommendation Cheatsheet

This document provides recommendations for selecting AI models for different specialized agents, optimizing for a balance of performance, speed, and cost.

| Agent | Recommended Models | Use Case & Rationale |
|---|---|---|
| **`ResearchAgent`** | `GPT-4o`, `Claude 3.5 Sonnet` | For complex research, analysis, and generating well-structured, accurate reports where precision is key. [2, 4] |
| **`BackendCoderAgent`** | `Claude 3.5 Sonnet`, `GPT-4o` | For complex logic, algorithms, database interactions, and new feature implementation. These models show strong performance in code generation. [5, 9] |
| | `Claude 3 Haiku`, `Gemini 1.5 Flash` | For boilerplate code, simple scripts, unit tests, or modifying existing files where speed and cost-effectiveness are priorities. [11, 12, 17] |
| **`FrontendCoderAgent`** | `Claude 3.5 Sonnet` | Consistently praised for generating high-quality UI and frontend code. [5, 9] |
| | `GPT-4o` | A very strong all-rounder, excellent for component logic and interactivity. [2] |
| | `Claude 3 Haiku` | Good for generating simple HTML/CSS structures or boilerplate component files quickly. [11] |
| **`DevOpsAgent`** | `Claude 3.5 Sonnet`, `GPT-4o` | For writing clear and correct configuration files (e.g., `Dockerfile`, CI/CD pipelines), where precision is important. [7] |
| **`QA_Agent`** | `GPT-4o` | Excellent for logical reasoning and carefully checking code against acceptance criteria. [7] |

*References are based on aggregated performance reviews and benchmark comparisons from 2024.* 
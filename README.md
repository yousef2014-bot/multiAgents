# Overview: AgentV1 vs. AgentV2 Evolution

This project has evolved from `AgentV1` to `AgentV2` to create a more autonomous, integrated, and professional multi-agent system. Here's a quick summary of the key differences:

| Feature           | AgentV1                                     | AgentV2                                                                 |
|-------------------|---------------------------------------------|-------------------------------------------------------------------------|
| **Philosophy**    | Instruction-follower, simpler workflow      | Autonomous, proactive, strategic thinking across agents                 |
| **Task Management** | Markdown files (`tasks_master.md`)          | **Taskwarrior** as the single source of truth for all tasks             |
| **Agent Workflow**| Linear, manual task assignment              | Multi-stage, agents proactively discover and manage their own tasks     |
| **Communication** | Basic chat responses, file modifications    | Richer communication, direct terminal commands, structured logs, model recommendations |
| **Output Format** | Single Markdown report / simple file edits | Direct Taskwarrior commands, detailed research briefs, dedicated logs, model cheatsheets |

This evolution aims to enhance the system's efficiency, traceability, and adherence to professional software development practices.

---
# Agent Workflow Diagram

## Complete Workflow Scenario: Building a Full-Stack Todo App

```mermaid
%% Agent Workflow Diagram

graph TD
    %% User Input
    A[User: I want to build a todo app] --> B[ResearchAgent]
    
    %% Research Phase
    B --> B1{Goal Clear?}
    B1 -->|No| B2[Ask Clarifying Questions]
    B2 --> B3[User Answers]
    B3 --> B1
    B1 -->|Yes| B4[Research Technologies]
    B4 --> B5[Create docs/research_brief.md]
    
    %% Planning Phase
    B5 --> C[PlannerAgent]
    C --> C1[Read research_brief.md]
    C1 --> C2[Create project_plan/tasks_master.md]
    C2 --> C3[Organize into Phases]
    C3 --> C4[Assign Tasks to Specialized Agents]
    
    %% Backend Phase
    C4 --> D[BackendCoderAgent]
    D --> D1[Task 01: Setup FastAPI]
    D1 --> D2[Create api/main.py]
    D2 --> D3[Create api/requirements.txt]
    D3 --> E[QAAgent]
    E --> E1{QA Pass?}
    E1 -->|No| E2[Report Issues]
    E2 --> D
    E1 -->|Yes| E3[Mark Task Complete]
    E3 --> D4[Task 02: Create Models]
    D4 --> D5[Create api/models.py]
    D5 --> E4[QAAgent]
    E4 --> E5{QA Pass?}
    E5 -->|No| E6[Report Issues]
    E6 --> D4
    E5 -->|Yes| E7[Mark Task Complete]
    E7 --> D6[Task 03: Create API Endpoints]
    D6 --> D7[Update api/main.py]
    D7 --> E8[QAAgent]
    E8 --> E9{QA Pass?}
    E9 -->|No| E10[Report Issues]
    E10 --> D6
    E9 -->|Yes| E11[Mark Task Complete]
    
    %% Frontend Phase
    E11 --> F[FrontendCoderAgent]
    F --> F1[Task 04: Setup React]
    F1 --> F2[Create ui/ directory]
    F2 --> F3[Initialize React with Vite]
    F3 --> G[QAAgent]
    G --> G1{QA Pass?}
    G1 -->|No| G2[Report Issues]
    G2 --> F1
    G1 -->|Yes| G3[Mark Task Complete]
    G3 --> F4[Task 05: Create Todo Component]
    F4 --> F5[Create ui/src/components/TodoList.jsx]
    F5 --> G4[QAAgent]
    G4 --> G5{QA Pass?}
    G5 -->|No| G6[Report Issues]
    G6 --> F4
    G5 -->|Yes| G7[Mark Task Complete]
    
    %% DevOps Phase
    G7 --> H[DevOpsAgent]
    H --> H1[Task 06: Setup Docker]
    H1 --> H2[Create Dockerfile]
    H2 --> H3[Create docker-compose.yml]
    H3 --> I[QAAgent]
    I --> I1{QA Pass?}
    I1 -->|No| I2[Report Issues]
    I2 --> H1
    I1 -->|Yes| I3[Mark Task Complete]
    I3 --> H4[Task 07: Setup CI/CD]
    H4 --> H5[Create .github/workflows/]
    H5 --> I4[QAAgent]
    I4 --> I5{QA Pass?}
    I5 -->|No| I6[Report Issues]
    I6 --> H4
    I5 -->|Yes| I7[Mark Task Complete]
    
    %% Project Complete
    I7 --> J[Project Complete!]
    
    %% File Interactions
    subgraph "File System"
        FB[docs/research_brief.md]
        FP[project_plan/tasks_master.md]
        FQ[docs/qa_log.md]
        FA[api/]
        FU[ui/]
        FD[infrastructure/]
    end
    
    %% Agent to File Connections
    B5 -.-> FB
    C2 -.-> FP
    E3 -.-> FP
    E7 -.-> FP
    E11 -.-> FP
    G3 -.-> FP
    G7 -.-> FP
    I3 -.-> FP
    I7 -.-> FP
    E -.-> FQ
    G -.-> FQ
    I -.-> FQ
    D -.-> FA
    F -.-> FU
    H -.-> FD

    %% Styling
    classDef userNode fill:#e1f5fe
    classDef researchNode fill:#f3e5f5
    classDef planningNode fill:#e8f5e8
    classDef backendNode fill:#fff3e0
    classDef frontendNode fill:#fce4ec
    classDef devopsNode fill:#e0f2f1
    classDef qaNode fill:#f1f8e9
    classDef fileNode fill:#f5f5f5

    class A userNode
    class B,B1,B2,B3,B4,B5 researchNode
    class C,C1,C2,C3,C4 planningNode
    class D,D1,D2,D3,D4,D5,D6,D7 backendNode
    class F,F1,F2,F3,F4,F5 frontendNode
    class H,H1,H2,H3,H4,H5 devopsNode
    class E,E1,E2,E3,E4,E5,E6,E7,E8,E9,E10,E11,G,G1,G2,G3,G4,G5,G6,G7,I,I1,I2,I3,I4,I5,I6,I7 qaNode
    class FB,FP,FQ,FA,FU,FD fileNode

```

## Agent Communication Flow

```mermaid
sequenceDiagram
    participant U as User
    participant R as ResearchAgent
    participant P as PlannerAgent
    participant B as BackendCoderAgent
    participant F as FrontendCoderAgent
    participant D as DevOpsAgent
    participant Q as QAAgent
    participant FS as File System

    U->>R: "I want to build a todo app"
    R->>U: Ask clarifying questions
    U->>R: Provide answers
    R->>FS: Create docs/research_brief.md
    R->>U: Research complete

    U->>P: "Based on research_brief.md, create plan"
    P->>FS: Read docs/research_brief.md
    P->>FS: Create project_plan/tasks_master.md
    P->>U: Plan complete

    U->>B: "Implement Task 01: Setup FastAPI"
    B->>FS: Read project_plan/tasks_master.md
    B->>FS: Create api/main.py
    B->>FS: Create api/requirements.txt
    B->>U: "Task 01 complete. Files modified: api/main.py, api/requirements.txt"

    U->>Q: "QA Task 01"
    Q->>FS: Read project_plan/tasks_master.md
    Q->>FS: Read api/main.py
    Q->>FS: Read api/requirements.txt
    Q->>FS: Update docs/qa_log.md
    Q->>U: "QA Approved: Task 01"

    U->>F: "Implement Task 04: Setup React"
    F->>FS: Read project_plan/tasks_master.md
    F->>FS: Create ui/ directory
    F->>FS: Initialize React project
    F->>U: "Task 04 complete. Files modified: ui/"

    U->>Q: "QA Task 04"
    Q->>FS: Read project_plan/tasks_master.md
    Q->>FS: Check ui/ directory
    Q->>FS: Update docs/qa_log.md
    Q->>U: "QA Approved: Task 04"

    U->>D: "Implement Task 06: Setup Docker"
    D->>FS: Read project_plan/tasks_master.md
    D->>FS: Create Dockerfile
    D->>FS: Create docker-compose.yml
    D->>U: "Task 06 complete. Files modified: Dockerfile, docker-compose.yml"

    U->>Q: "QA Task 06"
    Q->>FS: Read project_plan/tasks_master.md
    Q->>FS: Read Dockerfile
    Q->>FS: Read docker-compose.yml
    Q->>FS: Update docs/qa_log.md
    Q->>U: "QA Approved: Task 06"
```

## Decision Points and Error Handling

```mermaid
flowchart TD
    A[Start Project] --> B{ResearchAgent: Goal Clear?}
    B -->|No| C[Ask Questions]
    C --> D[User Answers]
    D --> B
    B -->|Yes| E[Create Research Brief]
    
    E --> F{PlannerAgent: Plan Complete?}
    F -->|No| G[Add More Tasks]
    G --> F
    F -->|Yes| H[Start Coding Phase]
    
    H --> I{BackendCoderAgent: Task Ready?}
    I -->|No| J[Wait for Dependencies]
    J --> I
    I -->|Yes| K[Implement Task]
    
    K --> L{QAAgent: Code Pass?}
    L -->|No| M[Report Issues]
    M --> N[BackendCoderAgent: Fix Issues]
    N --> L
    L -->|Yes| O[Mark Task Complete]
    
    O --> P{More Backend Tasks?}
    P -->|Yes| I
    P -->|No| Q[Start Frontend Phase]
    
    Q --> R{FrontendCoderAgent: Task Ready?}
    R -->|No| S[Wait for Backend API]
    S --> R
    R -->|Yes| T[Implement Frontend Task]
    
    T --> U{QAAgent: Code Pass?}
    U -->|No| V[Report Issues]
    V --> W[FrontendCoderAgent: Fix Issues]
    W --> U
    U -->|Yes| X[Mark Task Complete]
    
    X --> Y{More Frontend Tasks?}
    Y -->|Yes| R
    Y -->|No| Z[Start DevOps Phase]
    
    Z --> AA{DevOpsAgent: Task Ready?}
    AA -->|No| BB[Wait for Code Complete]
    BB --> AA
    AA -->|Yes| CC[Implement DevOps Task]
    
    CC --> DD{QAAgent: Config Pass?}
    DD -->|No| EE[Report Issues]
    EE --> FF[DevOpsAgent: Fix Issues]
    FF --> DD
    DD -->|Yes| GG[Mark Task Complete]
    
    GG --> HH{More DevOps Tasks?}
    HH -->|Yes| AA
    HH -->|No| II[Project Complete!]
    
    %% Error Handling
    M --> JJ[Log Error in qa_log.md]
    V --> KK[Log Error in qa_log.md]
    EE --> LL[Log Error in qa_log.md]
```

## File Structure Evolution

```mermaid
graph LR
    subgraph "Phase 1: Research"
        A1[User Goal] --> A2[docs/research_brief.md]
    end
    
    subgraph "Phase 2: Planning"
        A2 --> B1[project_plan/tasks_master.md]
    end
    
    subgraph "Phase 3: Backend Development"
        B1 --> C1[api/main.py]
        B1 --> C2[api/requirements.txt]
        B1 --> C3[api/models.py]
    end
    
    subgraph "Phase 4: Frontend Development"
        B1 --> D1[ui/package.json]
        B1 --> D2[ui/src/App.jsx]
        B1 --> D3[ui/src/components/TodoList.jsx]
    end
    
    subgraph "Phase 5: DevOps"
        B1 --> E1[Dockerfile]
        B1 --> E2[docker-compose.yml]
        B1 --> E3[.github/workflows/deploy.yml]
    end
    
    subgraph "Quality Assurance"
        C1 --> F1[docs/qa_log.md]
        C2 --> F1
        C3 --> F1
        D1 --> F1
        D2 --> F1
        D3 --> F1
        E1 --> F1
        E2 --> F1
        E3 --> F1
    end
``` 

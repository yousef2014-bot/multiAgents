# AgentV3 Proposed Workflow Diagram

This diagram outlines the envisioned `AgentV3` architecture, introducing a central `Manager` agent and emphasizing the `Planner`'s role as a primary orchestrator and communication hub. Agents now operate in a more "broker-like" fashion, waiting for tasks from the Planner and returning direct responses.

```mermaid
%%{ init: { 'flowchart': { 'useMaxWidth': true } } }%%
graph LR
    %% User Interface
    subgraph User
        U[User]:::user
    end

    %% New Central Manager
    subgraph Manager System
        M(ManagerAgent):::manager
        MD[Manager Data: Project Name, Request, State]:::doc
    end

    %% Core Agents (Orchestrated)
    subgraph Core Agents
        P(PlannerAgent):::planner
        R(ResearchAgent):::research
        C(CoderAgent):::coder
        Q(QAAgent):::qa
    end

    %% Centralized Task/Message Broker (Conceptual - represented by Planner & Agents' waiting state)
    subgraph "Central Hub (Task & Response Flow)"
        CH(Central Message/Task Queue)
        style CH fill:#fff,stroke:#333,stroke-dasharray: 5 5
    end

    %% File System - for persistent plans & logs
    subgraph File System
        R_BRIEF[research/main_research_brief.md]:::doc
        P_PLAN[project_plan/current_plan.md]:::doc
        C_LOGS[coder_logs/]:::doc
        QA_LOG[docs/qa_log.md]:::doc
    end

    %% === FLOW START ===
    U -- Initial Project Request --> M
    M -- Stores Request --> MD
    M -- "Forward Request" --> P

    %% === PLANNER AGENT (P) FLOW - Orchestrator ===
    P -- "Reads Request" --> CH
    P -- "Needs Clarity/Permission" --> U
    U -- Provides Input --> P

    P -- Is Research Needed? --> P_Research{Research Required?}:::decision
    P_Research -- Yes --> R
    R -- Performs Research --> R_BRIEF
    R -->|"Research Output"| P

    P -- Formulates Plan --> P_Plan[Create Detailed Plan]
    P_Plan -->|"Saves plan"| P_PLAN
    P_Plan -->|"Assigns sub-tasks"| TW

    P -->|"Assigns Task to Coder"| C
    P -->|"Assigns Task to QA"| Q
    %% P -- Defines & Assigns Tasks --> D: (DevOps Task Request)

    P -->|"Sends Update"| M

    %% Reactive Planner Flow for Feedback from Coder/QA
    C -->|"Blocked / Needs Research"| P
    Q -->|"Unplanned Bug Report"| P
    P -- Processes Agent Requests --> P_Plan

    %% === RESEARCH AGENT (R) FLOW ===
    R -->|"Waits for Task"| CH
    R -- Performs Research --> R_BRIEF
    R -->|"Sends Research Brief"| P

    %% === CODER AGENT (C) FLOW ===
    C -->|"Waits for Task"| CH
    C -->|"Implements Code"| FS
    C -- Creates Logs --> C_LOGS
    C -->|"Sends Report"| P
    C -->|"Blocked/Unclear"| P

    %% === QA AGENT (Q) FLOW ===
    Q -->|"Waits for Task"| CH
    Q -- Performs Review & Testing --> QA_LOG
    Q -->|"Sends Verdict"| P
    Q -->|"Discovers Unplanned Bug"| P

    %% === MANAGER AGENT (M) FLOW - Top-Level Orchestration ===
    M -->|"Waits for Response"| P
    M -->|"Sends Final Report"| U

    %% === KEY INTERACTIONS & DATA FLOW ===
    P --> CH
    CH --> R
    CH --> C
    CH --> Q
    R --> CH
    C --> CH
    Q --> CH

    M -- Read --> MD
    P -- Read --> R_BRIEF
    P -- Write --> P_PLAN
    C -- Write --> FS
    Q -- Write --> QA_LOG
    R -- Write --> R_BRIEF


    %% Styling
    classDef user fill:#e1f5fe,stroke:#2196f3,stroke-width:2px;
    classDef manager fill:#ffecb3,stroke:#ffc107,stroke-width:2px;
    classDef planner fill:#e8f5e8,stroke:#4caf50,stroke-width:2px;
    classDef research fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px;
    classDef coder fill:#fff3e0,stroke:#ff9800,stroke-width:2px;
    classDef qa fill:#f1f8e9,stroke:#8bc34a,stroke-width:2px;
    classDef tool fill:#cfd8dc,stroke:#607d8b,stroke-width:2px;
    classDef doc fill:#fff3e0,stroke:#ff9800,stroke-width:2px;
    classDef decision fill:#ffcdd2,stroke:#f44336,stroke-width:2px;
``` 
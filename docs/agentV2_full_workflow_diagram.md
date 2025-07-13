# AgentV2 Full Workflow Diagram

This diagram illustrates the comprehensive workflow and interactions between the specialized agents within the `AgentV2` system. It highlights the central role of Taskwarrior for task management and the various feedback loops that ensure project progression and quality.

```mermaid
graph TD
    %% User Interaction
    subgraph User Interface
        U[User]:::user
    end

    %% Core Systems
    subgraph Central Management
        TW(Taskwarrior):::tool
    end

    subgraph Storage & Logs
        FS[File System: docs/, research/, coder_logs/, project_plan/]:::doc
    end

    %% Agent Definitions
    subgraph Specialized Agents
        R(ResearchAgent):::agent
        P(PlannerAgent):::agent
        C(CoderAgent):::agent
        Q(QAAgent):::agent
        %% D(DevOpsAgent):::agent - To be defined in future, interacts similarly
    end

    %% === FLOW START ===
    U -- Project Request --> R
    U -- Confirms / Provides Context --> R
    U -- Resolves Conflicts / Approves Plan --> P
    U -- Confirms Task Start --> C
    U -- Provides Testing Context --> Q

    %% === RESEARCH AGENT (R) FLOW ===
    R -- Discover Tasks --> TW: (agent:ResearchAgent, status:pending)
    R -- Extracts Goal & Performs Research --> R_Validate{Goal Clear? / Task Valid?}:::decision
    R_Validate -- No --> U: Ask Clarifying Questions (Arabic)
    R_Validate -- Yes --> R_Doc[Generate Research Brief]
    R_Doc --> FS: (Creates research/main_research_brief.md, topics_to_research.md, background_information/)
    R_Doc --> P: Provides Research Output (Implicit)

    %% === PLANNER AGENT (P) FLOW ===
    P -- Reads Research & Context --> FS: (research/main_research_brief.md)
    P -- Performs Architectural Review --> P_Conflict{Conflict Found?}:::decision
    P_Conflict -- Yes --> U: Report Conflict & Propose Resolution
    P_Conflict -- Yes --> R: (If Research needed for resolution)
    P_Conflict -- No --> P_Plan[Plan Sprints & Phases]
    P_Plan --> TW: Generate Taskwarrior Commands (task add, project:, agent:, depends:, priority:, annotations)
    P_Plan --> FS: (Creates docs/agent_model_cheatsheet.md on first run)

    %% Reactive Planning for QA Failures & Agent Requests
    FS -- QA_FAILED Reports --> P: (Reads docs/qa_log.md)
    P -- Creates BUGFIX Tasks --> TW: (agent:CoderAgent, +bug, priority:H, ref to qa_log)
    C -- Blocked Task Request --> P: (Creates Taskwarrior task: agent:PlannerAgent)
    R -- Blocked Task Request --> P: (Creates Taskwarrior task: agent:PlannerAgent)
    P -- Integrates & Prioritizes --> TW: (New/Updated Tasks)
    P -- Out-of-Scope Ideas --> FS: (project_plan/memory_log.md)

    %% === CODER AGENT (C) FLOW ===
    C -- Discover Tasks --> TW: (agent:[CoderRole]Agent, status:pending)
    C -- Select Task & Load Details --> TW
    C -- Performs Sanity Check --> C_Blocked{Task Blocked / Unclear?}:::decision
    C_Blocked -- Yes --> C_Request[Activate Requesting Tasks Capability]
    C_Request --> TW: (Create new Task: agent:Planner/Research, status:pending)
    C_Request --> TW: (Set Coder's original task status:waiting)
    C_Blocked -- No --> C_Code[Write Code & Implement]
    C_Code --> FS: (Modifies specified files)
    C_Code --> C_Report[Finalize & Report Completion]
    C_Report --> TW: (Mark Task done, Add Annotations)
    C_Report --> FS: (Create coder_logs/[ID]_[title].md for long notes)
    C_Report --> U: Confirm Completion (Chat)

    %% === QA AGENT (Q) FLOW ===
    Q -- Triggered by --> TW: (Dependency met, e.g., Coder's task done)
    Q -- Load Task Details --> TW
    Q -- Phase 1 --> Q_Static[Static Code Review (White-Box)]
    Q_Static --> Q_CR_Pass{Code Review Pass?}:::decision
    Q_CR_Pass -- No --> Q_ReportFail[Deliver QA FAILED Verdict]
    Q_CR_Pass -- Yes --> U: Request Testing Context (URL, Auth)
    U --> Q
    Q_CR_Pass -- Yes --> Q_Dynamic[Dynamic Live Testing (Black-Box)]
    Q_Dynamic --> TW: (Generates curl commands for execution log)
    Q_Dynamic --> Q_Test_Pass{Live Tests Pass?}:::decision
    Q_Test_Pass -- No --> Q_ReportFail
    Q_Test_Pass -- Yes --> Q_ReportApprove[Deliver QA APPROVED Verdict]

    Q_ReportFail --> FS: (Append to docs/qa_log.md)
    Q_ReportApprove --> FS: (Append to docs/qa_log.md)

    Q -- Discovers Unplanned Bug --> Q_ReportBug[Report Unplanned Bug]
    Q_ReportBug --> TW: (Create new Task: agent:PlannerAgent, +bug, +unplanned)


    %% === INTER-AGENT DEPENDENCIES & DATA FLOW ===
    TW --> C: Task Details
    TW --> Q: Task Details
    TW --> P: Task Details (for agent requests)
    TW --> R: Task Details

    FS -- Read --> P: research_brief.md
    FS -- Read --> Q: code files for review
    FS -- Read --> C: relevant code files
    FS -- Read --> P: qa_log.md (for reactive planning)

    %% Styling
    classDef user fill:#e1f5fe,stroke:#2196f3,stroke-width:2px;
    classDef agent fill:#e0f7fa,stroke:#00bcd4,stroke-width:2px;
    classDef tool fill:#e8f5e8,stroke:#4caf50,stroke-width:2px;
    classDef doc fill:#fff3e0,stroke:#ff9800,stroke-width:2px;
    classDef decision fill:#ffcdd2,stroke:#f44336,stroke-width:2px;

    linkStyle 0 stroke-width:2px,stroke:#2196f3;
    linkStyle 1 stroke-width:2px,stroke:#2196f3;
    linkStyle 2 stroke-width:2px,stroke:#2196f3;
    linkStyle 3 stroke-width:2px,stroke:#2196f3;
    linkStyle 4 stroke-width:2px,stroke:#2196f3;
    linkStyle 5 stroke-width:2px,stroke:#00bcd4;
    linkStyle 6 stroke-width:2px,stroke:#00bcd4;
    linkStyle 7 stroke-width:2px,stroke:#f44336;
    linkStyle 8 stroke-width:2px,stroke:#2196f3;
    linkStyle 9 stroke-width:2px,stroke:#00bcd4;
    linkStyle 10 stroke-width:2px,stroke:#00bcd4;
    linkStyle 11 stroke-width:2px,stroke:#ff9800;
    linkStyle 12 stroke-width:2px,stroke:#00bcd4;
    linkStyle 13 stroke-width:2px,stroke:#00bcd4;
    linkStyle 14 stroke-width:2px,stroke:#00bcd4;
    linkStyle 15 stroke-width:2px,stroke:#00bcd4;
    linkStyle 16 stroke-width:2px,stroke:#f44336;
    linkStyle 17 stroke-width:2px,stroke:#00bcd4;
    linkStyle 18 stroke-width:2px,stroke:#f44336;
    linkStyle 19 stroke-width:2px,stroke:#00bcd4;
    linkStyle 20 stroke-width:2px,stroke:#00bcd4;
    linkStyle 21 stroke-width:2px,stroke:#00bcd4;
    linkStyle 22 stroke-width:2px,stroke:#ff9800;
    linkStyle 23 stroke-width:2px,stroke:#00bcd4;
    linkStyle 24 stroke-width:2px,stroke:#ff9800;
    linkStyle 25 stroke-width:2px,stroke:#ff9800;
    linkStyle 26 stroke-width:2px,stroke:#00bcd4;
    linkStyle 27 stroke-width:2px,stroke:#f44336;
    linkStyle 28 stroke-width:2px,stroke:#ff9800;
    linkStyle 29 stroke-width:2px,stroke:#4caf50;
    linkStyle 30 stroke-width:2px,stroke:#ff9800;
    linkStyle 31 stroke-width:2px,stroke:#ff9800;
    linkStyle 32 stroke-width:2px,stroke:#ff9800;
    linkStyle 33 stroke-width:2px,stroke:#ff9800;
``` 
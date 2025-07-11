# Agent Configuration Guide

## Overview
This document defines the configuration and tools for each specialized agent in the project management workflow.

---

## 1. ResearchAgent Configuration

### Purpose
- Analyze project goals and requirements
- Research technical solutions and alternatives
- Identify risks and dependencies

### Tools & Process
- **Input:** User goal description
- **Research Tools:** 
  - Web search for latest technologies
  - Documentation review (official docs, GitHub, Stack Overflow)
  - API documentation analysis
- **Output:** `docs/research_brief.md`
- **Validation:** Must ask clarifying questions for ambiguous goals

### Trigger Command
```
"My goal is to [describe your project goal]"
```

---

## 2. PlannerAgent Configuration

### Purpose
- Create and maintain master task plan
- Organize project into logical phases
- Assign tasks to specialized agents

### Tools & Process
- **Input:** `docs/research_brief.md`
- **Planning Tools:**
  - Phase-based project structure
  - Task dependency mapping
  - Agent assignment logic
- **Output:** `project_plan/tasks_master.md`
- **Update Mode:** Append new tasks to existing plan

### Specialized Agent Types
- `BackendCoderAgent`: Python, Node.js, databases, APIs
- `FrontendCoderAgent`: HTML, CSS, JavaScript, React, Vue
- `DevOpsAgent`: Docker, CI/CD, deployment, infrastructure
- `ResearchAgent`: Follow-up research tasks

### Trigger Command
```
"Based on the file `docs/research_brief.md`, create the master task plan."
```

---

## 3. BackendCoderAgent Configuration

### Purpose
- Implement server-side logic and APIs
- Handle database operations
- Create backend services

### Tools & Process
- **Input:** Specific task from `tasks_master.md`
- **Development Tools:**
  - Python (FastAPI, Django, Flask)
  - Node.js (Express, NestJS)
  - Database tools (SQLAlchemy, Prisma)
  - API testing (Postman, curl)
- **Output:** Backend code files
- **Validation:** Must follow task description exactly

### Common File Types
- `main.py`, `app.py` (FastAPI/Flask)
- `models.py`, `schemas.py` (Pydantic)
- `requirements.txt`, `package.json`
- `database.py`, `config.py`

### Trigger Command
```
"Implement this BackendCoderAgent task: [copy task from tasks_master.md]"
```

---

## 4. FrontendCoderAgent Configuration

### Purpose
- Create user interfaces and components
- Handle client-side logic
- Implement responsive design

### Tools & Process
- **Input:** Specific task from `tasks_master.md`
- **Development Tools:**
  - React, Vue, Angular
  - HTML, CSS, JavaScript/TypeScript
  - UI libraries (Material-UI, Tailwind CSS)
  - Build tools (Vite, Webpack)
- **Output:** Frontend code files
- **Validation:** Must follow task description exactly

### Common File Types
- `App.jsx`, `index.jsx` (React)
- `components/`, `pages/`
- `styles.css`, `tailwind.config.js`
- `package.json`, `vite.config.js`

### Trigger Command
```
"Implement this FrontendCoderAgent task: [copy task from tasks_master.md]"
```

---

## 5. DevOpsAgent Configuration

### Purpose
- Set up deployment and infrastructure
- Configure CI/CD pipelines
- Handle environment management

### Tools & Process
- **Input:** Specific task from `tasks_master.md`
- **DevOps Tools:**
  - Docker, Docker Compose
  - GitHub Actions, GitLab CI
  - AWS, Azure, GCP services
  - Kubernetes, Terraform
- **Output:** Infrastructure and deployment files
- **Validation:** Must follow task description exactly

### Common File Types
- `Dockerfile`, `docker-compose.yml`
- `.github/workflows/`, `.gitlab-ci.yml`
- `terraform/`, `kubernetes/`
- `scripts/`, `config/`

### Trigger Command
```
"Implement this DevOpsAgent task: [copy task from tasks_master.md]"
```

---

## 6. QAAgent Configuration

### Purpose
- Verify task completion against acceptance criteria
- Ensure code quality and standards
- Log QA results for tracking

### Tools & Process
- **Input:** Task name and files to review
- **QA Tools:**
  - Code review against acceptance criteria
  - File structure validation
  - Basic functionality testing
- **Output:** QA verdict and log entry
- **Logging:** Append to `docs/qa_log.md`

### QA Process
1. Read task from `tasks_master.md`
2. Review specified files
3. Compare against acceptance criteria
4. Provide verdict: "QA Approved" or "QA Failed"
5. Log results

### Trigger Command
```
"Please QA 'Task [ID]: [Task Name]'. Review the files [file list] against the criteria in the master plan."
```

---

## Workflow Configuration

### File Structure
```
project/
├── docs/
│   ├── research_brief.md
│   └── qa_log.md
├── project_plan/
│   └── tasks_master.md
├── api/ (BackendCoderAgent)
├── ui/ (FrontendCoderAgent)
├── infrastructure/ (DevOpsAgent)
└── agents/
    ├── agent_configuration.md
    ├── research_agent.txt
    ├── planner_agent.txt
    ├── coder_agent.txt
    └── qa_agent.txt
```

### Agent Communication
- All agents read from and write to specific files
- Status tracking via checkboxes in `tasks_master.md`
- QA results logged in `docs/qa_log.md`
- Research findings in `docs/research_brief.md`

### Best Practices
1. **One task at a time:** Each agent focuses on one specific task
2. **File-based communication:** All communication through files
3. **Status tracking:** Use checkboxes to track progress
4. **QA validation:** Every task must pass QA before moving to next
5. **Clear triggers:** Each agent has specific trigger commands 
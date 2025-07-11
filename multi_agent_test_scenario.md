# Multi-Agent Test Scenario

## Current Agent Setup ✅

You have **6 specialized agents** ready to work together:

1. **ResearchAgent** (`research_agent.txt`)
2. **PlannerAgent** (`planner_agent.txt`) 
3. **BackendCoderAgent** (`coder_agent.txt`)
4. **FrontendCoderAgent** (`coder_agent.txt`)
5. **DevOpsAgent** (`coder_agent.txt`)
6. **QAAgent** (`qa_agent.txt`)

---

## Test Scenario: "Build a Weather Dashboard"

### Step 1: ResearchAgent Test

**Trigger the ResearchAgent:**
```
"My goal is to build a weather dashboard that shows current weather and 5-day forecast for any city"
```

**Expected ResearchAgent Output:**
- Asks clarifying questions if needed
- Creates `docs/research_brief.md` with:
  - Weather API recommendations (OpenWeatherMap, WeatherAPI)
  - Frontend framework suggestions (React, Vue)
  - Backend recommendations (FastAPI, Express)
  - Risk assessment

### Step 2: PlannerAgent Test

**Trigger the PlannerAgent:**
```
"Based on the file docs/research_brief.md, create the master task plan"
```

**Expected PlannerAgent Output:**
- Creates `project_plan/tasks_master.md` with phases:
  - Phase 1: Backend API Setup
  - Phase 2: Frontend Dashboard
  - Phase 3: Deployment

### Step 3: BackendCoderAgent Test

**Trigger the BackendCoderAgent:**
```
"Implement this BackendCoderAgent task:

### ✅ Task 01: Setup Weather API Backend
- **Status:** [ ] To-Do
- **Agent:** 👤 BackendCoderAgent
- **Files:** 📁 `api/main.py`, `api/requirements.txt`
- **Description:** 📋 Create FastAPI backend with weather API integration. Include OpenWeatherMap API key handling.
- **Criteria:** ✅
  - FastAPI app runs on port 8000
  - GET /weather/{city} endpoint returns weather data
  - API key is properly configured
"
```

**Expected BackendCoderAgent Output:**
- Creates `api/main.py` with FastAPI setup
- Creates `api/requirements.txt` with dependencies
- Implements weather endpoint
- Responds: "Task 01 complete. Files modified: api/main.py, api/requirements.txt"

### Step 4: QAAgent Test

**Trigger the QAAgent:**
```
"Please QA 'Task 01: Setup Weather API Backend'. Review the files api/main.py and api/requirements.txt against the criteria in the master plan."
```

**Expected QAAgent Output:**
```
QA Approved: Task 01: Setup Weather API Backend
Comments: All acceptance criteria were met. The code is clean and functions as expected.
```

### Step 5: FrontendCoderAgent Test

**Trigger the FrontendCoderAgent:**
```
"Implement this FrontendCoderAgent task:

### ✅ Task 04: Create Weather Dashboard UI
- **Status:** [ ] To-Do
- **Agent:** 👤 FrontendCoderAgent
- **Files:** 📁 `ui/src/components/WeatherDashboard.jsx`, `ui/src/App.jsx`
- **Description:** 📋 Create React component that fetches weather data from backend API and displays it in a dashboard layout.
- **Criteria:** ✅
  - Component makes API call to backend
  - Displays current weather and forecast
  - Responsive design with city input
"
```

**Expected FrontendCoderAgent Output:**
- Creates `ui/src/components/WeatherDashboard.jsx`
- Updates `ui/src/App.jsx`
- Implements weather data fetching
- Responds: "Task 04 complete. Files modified: ui/src/components/WeatherDashboard.jsx, ui/src/App.jsx"

---

## How to Test Your Multi-Agent Setup

### 1. **Test ResearchAgent First**
```bash
# Copy research_agent.txt content to Cursor Custom Prompt
# Then say: "My goal is to build a weather dashboard"
```

### 2. **Test PlannerAgent**
```bash
# Copy planner_agent.txt content to Cursor Custom Prompt
# Then say: "Based on the file docs/research_brief.md, create the master task plan"
```

### 3. **Test CoderAgents**
```bash
# Copy coder_agent.txt content to Cursor Custom Prompt
# Then copy a specific task from tasks_master.md and say:
# "Implement this BackendCoderAgent task: [paste task here]"
```

### 4. **Test QAAgent**
```bash
# Copy qa_agent.txt content to Cursor Custom Prompt
# Then say: "Please QA 'Task [ID]: [Task Name]'. Review the files [file list] against the criteria in the master plan."
```

---

## Agent Communication Flow

```mermaid
graph TD
    A[User: "Build weather dashboard"] --> B[ResearchAgent]
    B --> C[docs/research_brief.md]
    C --> D[PlannerAgent]
    D --> E[project_plan/tasks_master.md]
    E --> F[BackendCoderAgent]
    F --> G[api/main.py]
    G --> H[QAAgent]
    H --> I[docs/qa_log.md]
    I --> J[FrontendCoderAgent]
    J --> K[ui/src/components/]
    K --> L[QAAgent]
    L --> M[Project Complete!]
```

---

## File Dependencies

```
project/
├── docs/
│   ├── research_brief.md          ← ResearchAgent creates
│   └── qa_log.md                  ← QAAgent updates
├── project_plan/
│   └── tasks_master.md            ← PlannerAgent creates/updates
├── api/                           ← BackendCoderAgent creates
│   ├── main.py
│   └── requirements.txt
└── ui/                            ← FrontendCoderAgent creates
    └── src/
        ├── App.jsx
        └── components/
            └── WeatherDashboard.jsx
```

---

## Testing Checklist

- [ ] **ResearchAgent** creates `docs/research_brief.md`
- [ ] **PlannerAgent** creates `project_plan/tasks_master.md`
- [ ] **BackendCoderAgent** creates backend files
- [ ] **QAAgent** reviews and approves backend
- [ ] **FrontendCoderAgent** creates frontend files
- [ ] **QAAgent** reviews and approves frontend
- [ ] **DevOpsAgent** creates deployment files (optional)
- [ ] **QAAgent** reviews and approves deployment

---

## Tips for Multi-Agent Testing

1. **Start with ResearchAgent** - Always begin here
2. **Use PlannerAgent** - Creates the roadmap
3. **Test one CoderAgent at a time** - Don't mix agents
4. **QA after each task** - Ensures quality
5. **Check file outputs** - Verify each agent creates expected files
6. **Follow the workflow** - Don't skip steps

Your multi-agent setup is ready! Each agent has a specific role and they communicate through files. Test them one by one to see the magic happen! 🚀 
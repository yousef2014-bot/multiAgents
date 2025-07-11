# Project Master Task Plan: Example Project

## Phase 1: Backend Foundation

### ✅ Task 01: Setup FastAPI Project & Dependencies
- **Status:** [ ] To-Do
- **Agent:** 👤 BackendCoderAgent
- **Files:** 📁 `api/main.py`, `api/requirements.txt`
- **Description:** 📋 Setup a basic FastAPI app. Include dependencies: `fastapi`, `uvicorn`, `pydantic`. Create a root endpoint that returns `{"message": "API is running"}`.
- **Criteria:** ✅
  - `api/requirements.txt` is created with necessary packages.
  - App runs successfully with `uvicorn`.

### ✅ Task 02: Define Pydantic Models for a "Todo" Item
- **Status:** [ ] To-Do
- **Agent:** 👤 BackendCoderAgent
- **Files:** 📁 `api/models.py`
- **Description:** 📋 Create a Pydantic model `TodoItem` with fields: `id (int)`, `title (str)`, and `is_completed (bool)`.
- **Criteria:** ✅
  - `TodoItem` model is defined correctly in `api/models.py`.
  - The model includes appropriate data types.

### ✅ Task 03: Create API Endpoints (GET, POST)
- **Status:** [ ] To-Do
- **Agent:** 👤 BackendCoderAgent
- **Files:** 📁 `api/main.py`
- **Description:** 📋 Implement two endpoints: `GET /todos` to return a list of all to-do items, and `POST /todos` to create a new to-do item. Use an in-memory list for now.
- **Criteria:** ✅
  - `POST` endpoint correctly adds a new item.
  - `GET` endpoint returns the full list of items.

---

## Phase 2: Frontend Setup

### ✅ Task 04: Initialize React Project
- **Status:** [ ] To-Do
- **Agent:** 👤 FrontendCoderAgent
- **Files:** 📁 `ui/` (entire directory)
- **Description:** 📋 Use `Vite` (`npm create vite@latest`) to set up a new React project in the `ui` directory.
- **Criteria:** ✅
  - The `ui` directory contains a functional React project.
  - `npm run dev` starts the development server successfully.

### ✅ Task 05: Create Todo Display Component
- **Status:** [ ] To-Do
- **Agent:** 👤 FrontendCoderAgent
- **Files:** 📁 `ui/src/components/TodoList.jsx`
- **Description:** 📋 Create a React component that fetches data from the `GET /todos` backend endpoint and displays the list of to-do items.
- **Criteria:** ✅
  - The component makes an API call on load.
  - It correctly renders a list of titles from the API response. 
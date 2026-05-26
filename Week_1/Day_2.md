--------------------------------------------------------------------------------------------------------------------------------------------------------------------

# 📋 Daily Task Log — Skeleton Agent Project

**Intern:** Siddhan Baranwal
**Date:** 26th May 2026
**Team:** Microsoft (Internship)
**Project:** Hello World AI Agent — Skeleton Agent

---

## 1. Problem Statement

I tried to build a **"Hello World" style AI Agent** as a learning exercise before my main project. The requirements were:

1. The agent should have a **primary task** (some main work it does)
2. The agent should also have a **secondary background task** — receiving files on the back, and using rules/patterns to detect anomalies, then **accept or reject** those files
3. The agent should use **Azure AI Agent Service** (Microsoft's agent platform)
4. The project should demonstrate understanding of AI agent concepts, architecture patterns, and clean code principles

---

## 2. My Thought Process — From Idea to Implementation

### Initial Thinking:

- I needed to pick a simple "primary task" for the agent → chose **text summarization** (practical, easy to demo, uses LLM)
- For the "secondary task", I needed a file validation system → designed a **rule-based anomaly detector** that checks files for bad extensions, vulgar content, spam, malformed structure, etc.
- Both tasks needed to run **simultaneously** → used Python threading (primary on foreground, secondary on background)

### Architecture Decision:

# Architecture Decision

```txt
┌────────────────────────────────────────────────────────┐
│                    SKELETON AGENT                     │
├─────────────────────┬──────────────────────────────────┤
│ PRIMARY WORK        │ SECONDARY WORK (Background)     │
│ (Foreground)        │                                  │
│                     │                                  │
│ Text Summarizer     │ File Anomaly Detector            │
│ via Azure AI        │ (Rule-based validation)          │
│ Agent Service       │                                  │
│                     │ Watches incoming_files/          │
│ User types text     │ Validates against rules          │
│ Agent summarizes    │ Moves to accepted/rejected       │
└─────────────────────┴──────────────────────────────────┘
```

### Why This Design?

- Shows **dual-task architecture** (active + passive work)
- Shows **real-time monitoring** (watchdog for filesystem events)
- Shows **rule-based decision making** (configurable JSON rules)
- Shows **Azure AI integration** (cloud-hosted agent)
- Can run in **mock mode** without Azure (good for development/testing)

---

## 3. What I Built — Complete Code Breakdown

### Project Structure:


```txt

skeleton_agent/
├── main.py → Entry point, orchestrates both tasks
├── requirements.txt → Python dependencies
├── .env.example → Azure credentials template
├── SOLID.md → SOLID principles documentation
├── workflow.md → Architecture & workflow docs
├── config/
│ └── validation_rules.json → Configurable anomaly detection rules
├── src/
│ ├── agents/
│ │ ├── base_agent.py → Abstract BaseAgent interface
│ │ └── summarizer_agent.py → Text summarization agent (Azure AI)
│ └── validators/
│ ├── file_validator.py → FileValidator + FileProcessor + FileWatcher
│ └── rules/
│ ├── base_rule.py → Abstract ValidationRule interface
│ ├── extension_rule.py → Checks file extension
│ ├── size_rule.py → Checks file size / emptiness
│ ├── content_rule.py → Detects vulgar + spam content
│ ├── json_rule.py → Validates JSON structure
│ └── csv_rule.py → Validates CSV completeness
├── incoming_files/ → Drop files here (input)
├── processed/
│ ├── accepted/ → Files that passed validation
│ └── rejected/ → Files that failed validation
└── tests/
└── test_file_validator.py → 13 unit tests

```

### Key Files Explained:

| File | What It Does |
|------|-------------|
| `main.py` | Starts both tasks — summarizer on foreground, file watcher on background thread |
| `summarizer_agent.py` | Connects to Azure AI Agent Service, creates an agent, sends text, gets summaries |
| `file_validator.py` | Orchestrates the validation pipeline — runs each rule, builds results |
| `rules/*.py` | Individual validation rules — each checks ONE thing (SOLID: Single Responsibility) |
| `validation_rules.json` | Configurable rules — what extensions are allowed, what words are blocked, etc. |

---

## 4. Technology Stack

| Technology | Purpose |
|-----------|---------|
| **Python 3.11** | Primary language |
| **Azure AI Agent Service** | Cloud-hosted AI agent (text summarization) |
| **azure-ai-projects SDK** | Python SDK to interact with Azure AI agents |
| **azure-identity** | Authentication with Azure (DefaultAzureCredential) |
| **watchdog** | Real-time filesystem monitoring (detects new files) |
| **python-dotenv** | Load environment variables from `.env` file |
| **threading** | Run file watcher concurrently with summarizer |
| **pytest** | Unit testing framework |
| **Git** | Version control |
| **Azure CLI** | Azure resource management |

---

## 5. The Agents — What They Do

### Agent 1: Summarizer Agent (Primary Work)

| Aspect | Detail |
|--------|--------|
| **What** | Summarizes text using a Large Language Model |
| **Where** | Lives on Azure AI Agent Service (cloud) |
| **Model** | GPT-4o-mini (or configurable) |
| **How** | Creates agent → creates thread → sends message → gets response |
| **Fallback** | Mock mode if no Azure credentials (returns word count + preview) |

**Flow:**

User types text → Agent sends to Azure → LLM processes → Summary returned → Displayed


### Agent 2: File Anomaly Detector (Secondary Work)

| Aspect | Detail |
|--------|--------|
| **What** | Watches a folder and validates incoming files against rules |
| **Where** | Runs locally as a background thread |
| **Rules** | Extension check, size check, vulgar content, spam, JSON/CSV validation |
| **Action** | Moves valid files to `accepted/`, invalid files to `rejected/` |
| **Config** | All rules are in `config/validation_rules.json` (no code changes needed) |

**Validation Rules:**

1. Extension → Is it .txt, .py, .json etc? (not .exe, .dll)
2. Size → Is it under 10MB?
3. Empty → Does it have content?
4. Vulgar → Contains bad words? (word boundary matching)
5. Spam → Contains spam phrases? ("buy now", "free money")
6. JSON → Valid JSON? Has required fields? No null values?
7. CSV → Correct columns? No missing cells?


---

## 6. Issues Faced & How I Resolved Them

### Issue 1: Git Not Installed
- **Problem:** `git` command not found on the machine
- **Solution:** Installed Git via `winget install --id Git.Git`

### Issue 2: Python Not Installed
- **Problem:** Only Windows Store Python stubs existed (not real Python)
- **Solution:** Installed Python 3.11 via `winget install --id Python.Python.3.11`

### Issue 3: Azure Permissions
- **Problem:** My intern account (`t-sibaranwal@microsoft.com`) didn't have Contributor access on any subscription to create Azure AI resources
- **Solution:** Agent works in **mock mode** without Azure credentials. Need to ask manager for a connection string or subscription access.

### Issue 4: Windows Console Encoding (Unicode Emojis)
- **Problem:** `UnicodeEncodeError` when printing emojis (🤖, ✅, ❌) on Windows console (uses cp1252 encoding)
- **Solution:** Added UTF-8 encoding wrapper at the top of `main.py`:
```python
if sys.platform == "win32":
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding="utf-8", errors="replace")
```

### Issue 5: File Watcher Race Condition (MAJOR BUG)

- Problem: When you echo "text" > file.txt on Windows, the OS creates the file (0 bytes) FIRST, then writes content. The watchdog library detected the file at 0 bytes and rejected it as "empty".
- Root Cause: Windows file creation is a 2-step process (create → write). Watchdog fires on_created between these steps.
- Solution:
1. Process existing files on the main thread (not background) — guaranteed to see complete files
2. For real-time watching: added 1-second delay + file size > 0 check before validating
3. Added deduplication set to prevent double-processing
def on_any_event(self, event):
time.sleep(1.0) # Wait for write to complete
if os.path.isfile(file_path) and os.path.getsize(file_path) > 0:
self._processed.add(file_path)
self.watcher._process_file(file_path)

### Issue 6: User Typing Commands in Agent Prompt

- Problem: I was typing echo "text" > file.txt inside the agent's summarizer prompt, expecting it to create files. Instead, the agent treated it as text to summarize.
- Solution: Need TWO terminals — one running the agent, one for dropping files.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 7. Workflow — How the Agent Works

### Startup Flow:

```txt
┌─────────────────────────────────────────────────────────────┐
│ python main.py                                             │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 1: Initialize Summarizer Agent                        │
│ → Connects to Azure AI (or enters mock mode)               │
│ → Creates agent with "summarize text" instructions         │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 2: Process Existing Files (Main Thread)               │
│ → Scans incoming_files/ for any files already there        │
│ → Validates each file → moves to accepted/ or rejected/    │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 3: Start File Watcher (Background Thread)             │
│ → Uses watchdog to monitor incoming_files/ in real-time    │
│ → Any new file dropped triggers validation automatically   │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ STEP 4: Interactive Summarizer Loop (Foreground)           │
│ → User types text → Agent summarizes → Displays result     │
│ → Meanwhile, file watcher runs silently in background      │
│ → Type 'quit' to exit                                      │
└─────────────────────────────────────────────────────────────┘
```
### File Validation Decision Flowchart:

```txt
┌────────────────┐
│ File Received  │
└───────┬────────┘
        │
        ▼
┌───────────────────────┐
│ Is extension allowed? │
└─────┬───────────┬─────┘
      │ NO        │ YES
      ▼            ▼
┌──────────┐   ┌──────────────────┐
│ REJECT ❌ │   │ Is size ≤ 10MB? │
└──────────┘   └────┬────────┬────┘
                    │ NO     │ YES
                    ▼         ▼
               ┌──────────┐ ┌───────────────────┐
               │ REJECT ❌ │ │ Is file non-empty? │
               └──────────┘ └────┬────────┬─────┘
                                 │ NO     │ YES
                                 ▼         ▼
                            ┌──────────┐ ┌────────────────────┐
                            │ REJECT ❌ │ │ Contains vulgar?   │
                            └──────────┘ └────┬────────┬──────┘
                                              │ YES    │ NO
                                              ▼         ▼
                                         ┌──────────┐ ┌─────────────────┐
                                         │ REJECT ❌ │ │ Contains spam? │
                                         └──────────┘ └───┬────────┬────┘
                                                           │ YES   │ NO
                                                           ▼        ▼
                                                      ┌──────────┐ ┌────────────────┐
                                                      │ REJECT ❌ │ │ Format checks │
                                                      └──────────┘ │ pass?          │
                                                                   │ (JSON / CSV)  │
                                                                   └──┬────────┬───┘
                                                                      │ NO     │ YES
                                                                      ▼         ▼
                                                                 ┌──────────┐ ┌──────────┐
                                                                 │ REJECT ❌ │ │ ACCEPT ✅ │
                                                                 └──────────┘ └──────────┘
```

## Dual-Task Concurrency

```txt
TIME ──────────────────────────────────────────────────────────────►

FOREGROUND (Main Thread):

┌──────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌──────┐
│  Init    │  │ User types │  │ User types │  │ User types │  │ Quit │
│  Agent   │  │  text #1   │  │  text #2   │  │  text #3   │  │      │
└──────────┘  └────────────┘  └────────────┘  └────────────┘  └──────┘


BACKGROUND (Daemon Thread):

┌──────────────────────────────────────────────────────────────────┐
│ Watching incoming_files/ ... file1 detected → validate → move  │
│ ... file2 detected → validate → move ... (continuous)          │
└──────────────────────────────────────────────────────────────────┘
```

---

## 8. SOLID Principles Implementation

### What is SOLID?

```txt
┌───────────────────────────────┬────────────────────────────────────────────────┐
│ Principle                     │ Meaning                                        │
├───────────────────────────────┼────────────────────────────────────────────────┤
│ S — Single Responsibility     │ Each class does ONE thing only                 │
├───────────────────────────────┼────────────────────────────────────────────────┤
│ O — Open/Closed               │ Add features without modifying existing code   │
├───────────────────────────────┼────────────────────────────────────────────────┤
│ L — Liskov Substitution       │ Subtypes work wherever base types are expected │
├───────────────────────────────┼────────────────────────────────────────────────┤
│ I — Interface Segregation     │ Small, focused interfaces                      │
│                               │ (not bloated ones)                             │
├───────────────────────────────┼────────────────────────────────────────────────┤
│ D — Dependency Inversion      │ Depend on abstractions, not concrete classes   │
└───────────────────────────────┴────────────────────────────────────────────────┘
```

---

## How I Applied Each

### S — Single Responsibility

BEFORE:
- FileValidator had 7 responsibilities
  (extension + size + content + JSON + CSV + ...)

AFTER:
- ExtensionRule → only checks extensions
- SizeRule → only checks size
- VulgarContentRule → only checks bad words
- SpamContentRule → only checks spam
- JsonValidationRule → only validates JSON
- CsvValidationRule → only validates CSV

---

### O — Open/Closed

```python
# BEFORE: Adding a rule meant editing validate_file() method

# AFTER: Just create a new class and inject it:

class MyNewRule(ValidationRule):

    def can_apply(self, file_path):
        return True

    def validate(self, file_path, content, config):
        return ["reason"] if "bad" in content else []


validator = FileValidator(
    rules=[
        ExtensionRule(),
        SizeRule(),
        MyNewRule()
    ]
)

# ↑ No existing code was modified!
```

---

### L — Liskov Substitution

```python
# Any class implementing BaseAgent can replace SummarizerAgent:

class TranslatorAgent(BaseAgent):

    def initialize(self):
        ...

    def execute(self, text):
        ...

    def cleanup(self):
        ...


# main.py doesn't care which agent it gets
# — they're interchangeable
```

---

### I — Interface Segregation

```python
# BaseAgent has ONLY 3 methods (minimal interface):

class BaseAgent(ABC):

    def initialize(self) -> bool:
        ...

    def execute(self, input_text: str) -> str:
        ...

    def cleanup(self):
        ...


# ValidationRule has ONLY 2 methods:

class ValidationRule(ABC):

    def validate(self, file_path, content, config) -> list[str]:
        ...

    def can_apply(self, file_path) -> bool:
        ...
```

---

### D — Dependency Inversion

```python
# BEFORE: FileWatcher hardcoded its dependency

class FileWatcher:

    def __init__(self):
        self.validator = FileValidator()   # ← tightly coupled!


# AFTER: Dependencies are injected

class FileWatcher:

    def __init__(self, validator=None, processor=None):

        self.validator = validator or FileValidator()
        self.processor = processor or FileProcessor()
```

---

## 9. Demo Results

When running with test files:

```txt
✅ ACCEPTED: hello.txt
   → clean text, proper extension

✅ ACCEPTED: code.py
   → valid Python file

❌ REJECTED: bad.exe
   → blocked extension .exe

❌ REJECTED: spam.txt
   → contains "click here to win free money"

❌ REJECTED: vulgar.md
   → contains inappropriate language

❌ REJECTED: broken.json
   → malformed JSON

❌ REJECTED: incomplete.json
   → missing required fields

❌ REJECTED: missing.csv
   → empty cells in data
```

---

## 10. What I Learned

1. AI Agent Architecture
   → How agents are created, given instructions,
     and communicate through threads/messages
     on Azure AI Agent Service

2. Dual-task Pattern
   → Running foreground (interactive) and
     background (autonomous) work simultaneously
     using threading

3. Rule-based Systems
   → How to build configurable,
     extensible validation pipelines

4. Race Conditions
   → Learned about Windows filesystem timing issues
     with watchdog and how to handle them

5. SOLID Principles
   → Practical application of all 5 principles
     in a real project

6. Dependency Injection
   → How to make classes testable
     and loosely coupled

7. Azure SDK
   → Working with:
     - azure-ai-projects
     - azure-identity
     - DefaultAzureCredential

---

## 11. Git Commit History

```txt
79a2d2e Add SOLID.md documentation and update README
2eac258 Refactor codebase to follow SOLID principles
4b4c645 Fix file watcher race condition on Windows
d8d0f33 Add workflow.md with architecture docs and flowcharts
f17ea06 Initial commit: Skeleton Agent with dual-task architecture
```

---

## 12. Next Steps / Pending

- [ ] Get Azure AI Foundry connection string from manager
      (need Contributor access or shared project)

- [ ] Test with real Azure AI Agent Service
      (currently in mock mode)

- [ ] Add more validation rules
      (filename checks, encoding detection)

- [ ] Add REST API for file submission

- [ ] Integrate with Microsoft Agency
      for multi-agent orchestration

---
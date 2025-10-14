# 🚀 Automated Code Deployment & Task Receiver System

**LLM-powered code generation and seamless deployment to GitHub Pages via FastAPI.**

This project features a robust, asynchronous backend service built on **FastAPI** that orchestrates the entire process of receiving a task, generating code using the **Gemini API**, and instantly deploying the results to a dedicated **GitHub repository**.

---

## 📊 Quick Status

| Component         | Status    | Technology             |
|-------------------|-----------|------------------------|
| Backend Core      | Active    | Python 3.10+           |
| API Framework     | Stable    | FastAPI                |
| Code Generation   | Integrated| Gemini 2.5 Flash API   |
| Deployment Flow   | Automated | Git & GitHub Pages     |
| Concurrency       | Controlled| asyncio & Semaphore    |

---

## ✨ Key Features

This system is designed for **high reliability** and **fully automated CI/CD** for LLM-generated web applications.

- **Asynchronous Task Processing** – Uses Python's `asyncio` and a `Semaphore` to manage concurrent task submissions without blocking the service.  
  _[cite: main.py]_

- **LLM Orchestration** – Handles distinct Round 1 (Full Generation) and Round 2+ (Surgical Update) workflows, ensuring minimal, controlled changes for iterative refinement.  
  _[cite: main.py]_

- **Guaranteed Attachment Handling** – Reliably processes data URI and external HTTP image attachments, saving them to the local repository for commitment.  
  _[cite: main.py]_

- **End-to-End Deployment** – Automatically initializes/clones a GitHub repository, commits the generated files, pushes to the main branch, and configures GitHub Pages for immediate live preview.  
  _[cite: main.py]_

- **External Notification** – Notifies an external evaluation server with the final deployment URL and commit details upon success.  
  _[cite: main.py]_

---

## ⚙️ Architecture and Flow

The application manages the entire task lifecycle from ingestion to deployment:

```mermaid
graph TD
    A[POST Task Request /ready] -->|1. Authenticate Secret| B(Start Background Task)
    B --> C(Acquire Concurrency Slot)
    C --> D(Setup Git Repo: Init or Clone)
    D --> E{Call Gemini API <br> with Brief & Attachments}
    E --> F(Save Generated index.html, README.md, LICENSE)
    F --> G(Save Attachment Files)
    G --> H(Commit & Push to GitHub)
    H --> I(Configure GitHub Pages)
    I --> J(Notify Evaluation Server)
    J --> K[Release Concurrency Slot]

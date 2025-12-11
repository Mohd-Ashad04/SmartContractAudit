# SmartContractAudit – AI-Powered Smart Contract Security Auditor

SmartContractAudit is an AI-driven auditing system designed to automatically analyze smart contracts for security vulnerabilities, gas optimization issues, and code quality risks. The project uses a multi-agent architecture powered by Google Gemini and exposes a FastAPI backend with a simple HTML frontend. It is fully containerized using Docker and deployable on Render or any modern cloud platform.

---

# Overview

SmartContractAudit provides an automated solution for assessing smart contract safety and efficiency. Traditional smart contract auditing is slow, manual, expensive, and requires deep blockchain expertise. This system uses AI to automate the audit, producing instant structured reports that highlight vulnerabilities, performance issues, and best-practice recommendations.

---

# Key Features

### Security Analysis  
Detects:
- Reentrancy vulnerabilities  
- Integer overflow/underflow risks  
- Access-control issues  
- tx.origin misuse  
- Unsafe external interactions  
- Missing function visibility  
- Fallback function risks  

### Gas Optimization  
Identifies:
- Expensive loops  
- Redundant storage operations  
- Poor variable placement  
- Missing immutables/constants  
- Unused or unnecessary state variables  

### Code Quality Evaluation  
Examines:
- Naming conventions  
- Event usage  
- Modifier usage  
- Error-handling patterns  
- Best-practice alignment with Solidity guidelines  

### Multi-Agent AI Workflow  
The backend orchestrates multiple specialized agents:  
- Security Agent  
- Gas Optimization Agent  
- Code Quality Agent  
- Report Compiler  

Each agent uses Google Gemini independently, improving depth and reliability of the audit.

---

# Working (Detailed Internal Workflow)

This section explains the internal functioning of the system from input to final output.

### 1. User Input
A user enters smart contract code in the frontend or sends a POST request to `/analyze`.  
The input includes:
- Contract code (Solidity, Vyper, etc.)  
- Contract language  

### 2. Backend Receives Request
FastAPI receives the request and passes the contract code to the audit pipeline.

### 3. LLM Initialization
`get_llm()` initializes the Google Gemini model using the `GEMINI_API_KEY`.  
If the key is missing, the backend returns an error.

### 4. Multi-Agent System Creation
`create_audit_crew()` creates multiple agents, each with:
- A specific role  
- A defined goal  
- A backstory  
- Its own prompt template  

Agents created:
1. Security Agent  
2. Gas Optimization Agent  
3. Code Quality Agent  

### 5. Agents Run Their Tasks
Each agent sends its prompt to the Gemini model, receives an independent analysis, and returns text output.

Example tasks include:
- Identifying vulnerabilities  
- Suggesting gas improvements  
- Scoring code quality  

### 6. Aggregation Layer
Outputs from all agents are merged into a unified raw text audit.

### 7. Parsing Layer
`process_audit_result()` extracts structured information from raw text using:
- Keyword extraction  
- Pattern matching  
- Regular expressions  

The parser generates structured JSON fields:
- vulnerabilities  
- gas_optimizations  
- recommendations  
- code_quality_score  
- severity_score  

### 8. Backend Returns Final Report
The FastAPI endpoint returns a JSON response containing:
- Detailed security findings  
- Optimization suggestions  
- Recommended fixes  
- Severity scoring  
- Summary insights  

### 9. Frontend Displays Audit
The frontend displays all findings in a clean, readable format.

---

# System Architecture

```mermaid
flowchart TD
User --> UI[Frontend HTML/JS]
UI --> API[FastAPI Backend]
API --> Agents[Multi-Agent Audit System]
Agents --> LLM[Google Gemini Model]
LLM --> Agents
Agents --> Parser[Output Parser]
Parser --> API
API --> User

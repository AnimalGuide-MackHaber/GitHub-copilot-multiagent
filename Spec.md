Technical Specification: Graph-Powered Multi-Agent Software Development System (G-MASDS)

1. Executive Summary

This document outlines the architecture for a multi-agent system (MAS) designed to automate and assist in complex software development tasks. By integrating GitHub Copilot CLI with domain-specific Knowledge Graphs (KGs) and a centralized orchestrator, the system enables specialized agents to collaborate, maintain context through JSON-based memory, and be managed via a drag-and-drop GUI.

2. System Architecture

2.1 Core Components

Orchestrator Agent: The "brain" of the operation. It parses high-level user requirements, decomposes them into tasks, and routes them to specialized agents based on the defined communication graph. It manages execution flow (sequential vs. parallel).

Specialized Domain Agents: Individual agents (e.g., Frontend Specialist, Security Auditor) that possess:

Domain Knowledge Graph: A vectorized or relational representation of domain-specific documentation and code patterns.

GitHub Copilot CLI Interface: For code generation and terminal command execution.

Human-in-the-Loop (HITL) Node: A specialized node type in the workflow that pauses execution and awaits manual approval, feedback, or data entry from a user via the GUI.

Knowledge Graph Engine: Manages the extraction of "knowledge" from existing codebases using full logic path analysis.

JSON Memory System: A per-agent "ledger" tracking inputs, outputs, thoughts, and inter-agent messages.

Interactive GUI: A React-based dashboard for agent configuration and workflow orchestration.

3. Technical Detailed Breakdown

3.1 Copilot CLI & Authentication

The system interacts with the GitHub Copilot CLI by executing it as a sub-process.

Auth Strategy: The system first attempts to read the standard GITHUB_COPILOT_TOKEN environment variable.

Fallback: If the variable is missing, the system reads from an appsettings.json file located in the root directory.

Config Schema:

{
  "CopilotSettings": {
    "ApiKey": "ghp_xxxxxxxxxxxx",
    "Environment": "Production",
    "TimeoutSeconds": 30
  }
}


3.2 Knowledge Graph & Logic Path Analysis

Unlike standard RAG systems that only index snippets, the G-MASDS KG Engine performs Full Logic Path Analysis:

Tracing: It maps not just definitions, but the flow of data across the application (e.g., "Variable X starts at the API layer, is transformed by Service Y, and stored in DB Table Z").

Implementation: Utilizes control-flow graphs (CFGs) and data-flow graphs (DFGs) to provide agents with deep architectural context.

3.3 The Orchestrator & Execution Modes

The Orchestrator processes the workflow graph using two primary modes:

Sequential: Tasks are executed one-by-side, passing the cumulative memory of previous agents to the next. Best for logic-dependent chains (e.g., Code -> Review -> Fix).

Parallel: Independent agents work on sub-tasks simultaneously. The Orchestrator handles "Fan-In" merging, where it synthesizes the outputs of multiple agents before proceeding.

3.4 Conflict Resolution Logic

When two agents provide conflicting advice (e.g., a Performance Agent suggests a change that a Security Agent flags), the Orchestrator follows a weighted priority hierarchy:

Security > Stability > Performance > Stylistic Preferences.

If priorities are equal, the Orchestrator automatically generates a "Conflict Resolution Ticket" and routes the output to a Human-in-the-Loop node for final arbitration.

4. UI/UX Requirements

4.1 Skill Definition Panel

A form-based interface to define:

Prompt Persona: System instructions for the agent.

KG Source: File paths or URLs to the codebases/docs it should index.

Parallelism Toggle: Ability to mark a node as "Parallel-Ready."

4.2 Drag-and-Drop Workflow Canvas

Nodes: Represent agents or HITL triggers.

HITL Node: Visually distinct (e.g., orange) nodes that block the flow until a user clicks "Approve" or provides a text response.

Routing: Users can define conditional paths (e.g., if Security Agent Score < 80, route to Developer Agent; else, route to Deploy Agent).

5. Technology Stack

Backend: Node.js (for high-concurrency event handling) or Python (FastAPI).

LLM Interface: GitHub Copilot CLI.

Graph Database: Neo4j (for complex path analysis) or FalkorDB.

Frontend: React with Tailwind CSS and React Flow for the canvas.

Storage: Local JSON files for memory; ChromaDB for vector search.

6. Updated Implementation Roadmap

Phase 1: Implement appsettings.json and Copilot CLI wrapper.

Phase 2: Build the KG Engine with tree-sitter for full logic path parsing.

Phase 3: Develop the React Flow GUI with HITL node support.

Phase 4: Implement Orchestrator logic for Sequential vs. Parallel execution and Conflict Resolution.

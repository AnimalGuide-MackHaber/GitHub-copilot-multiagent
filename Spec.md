Technical Specification: Graph-Powered Multi-Agent Software Development System (G-MASDS)
​1. Executive Summary
​This document outlines the architecture for a multi-agent system (MAS) designed to automate and assist in complex software development tasks. By integrating GitHub Copilot CLI with domain-specific Knowledge Graphs (KGs) and a centralized orchestrator, the system enables specialized agents to collaborate, maintain context through JSON-based memory, and be managed via a drag-and-drop GUI.
​2. System Architecture
​2.1 Core Components
​Orchestrator Agent: The "brain" of the operation. It parses high-level user requirements, decomposes them into tasks, and routes them to specialized agents based on the defined communication graph.
​Specialized Domain Agents: Individual agents (e.g., Frontend Specialist, Security Auditor, Database Architect) that possess:
​Domain Knowledge Graph: A vectorized or relational representation of domain-specific documentation and code patterns.
​GitHub Copilot CLI Interface: For code generation and terminal command execution.
​Knowledge Graph Engine: Manages the extraction of "knowledge" from existing codebases (AST parsing, dependency mapping) and provides a query interface for agents.
​JSON Memory System: A per-agent "ledger" tracking inputs, outputs, thoughts, and inter-agent messages.
​Interactive GUI: A React-based dashboard for agent configuration and workflow orchestration.
​3. Technical Detailed Breakdown
​3.1 Agent Memory & Communication
​Each agent maintains a memory.json file. This is crucial for long-running development tasks where context might be lost between LLM calls.
​Schema Example:

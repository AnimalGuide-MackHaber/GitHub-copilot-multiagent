3.2 Knowledge Graph Integration
​Static Analysis: The system uses tools like Tree-sitter to parse existing code into an Abstract Syntax Tree (AST), which is then converted into a graph (nodes = functions/classes, edges = calls/imports).
​Vector Embeddings: Nodes are embedded using a model (e.g., text-embedding-3-small) to allow agents to perform semantic searches over the codebase.
​3.3 The Orchestrator Logic
​The Orchestrator uses a Finite State Machine (FSM) or a Directed Acyclic Graph (DAG) model to determine routing.
​Routing Logic: If the user says "Build a login page," the Orchestrator identifies the "UI Agent" and "Auth Agent" as required nodes.
​Loopback: It monitors the memory files to check if an agent has "failed" or needs more info, triggering a re-route.
​4. UI/UX Requirements
​4.1 Skill Definition Panel
​A form-based interface to define:
​Prompt Persona: System instructions for the agent.
​KG Source: File paths or URLs to the codebases/docs it should index.
​Output Constraints: e.g., "Must output valid TypeScript."
​4.2 Drag-and-Drop Workflow Canvas
​Nodes: Represent agents.
​Edges: Represent the flow of data/commands.
​Implementation: Utilizes React Flow or XYFlow for the interactive canvas. Users can draw a line from the "Orchestrator" to the "QA Agent" to establish a verification step.
​5. Technology Stack
​Backend: Node.js or Python (FastAPI).
​LLM Interface: GitHub Copilot CLI (wrapped as a child process or via official extensions).
​Graph Database: Neo4j or a lightweight local solution like FalkorDB/NetworkX.
​Frontend: React with Tailwind CSS and React Flow.
​Storage: Local JSON files for memory; ChromaDB or Pinecone for vector search.
​6. Clarifying Questions for Robustness
​To refine this spec further, we need to address the following:
​Copilot CLI Auth: How will the system handle the interactive authentication required by the GitHub Copilot CLI when running in an automated, multi-agent environment?
​Concurrency: Should agents run tasks in parallel, or is the workflow strictly sequential? (This affects how the Orchestrator handles race conditions in the memory JSONs).
​KG Depth: For the "Knowledge Graph of existing code," how deep should the indexing go? (Just function signatures and imports, or full logic path analysis?)
​Human-in-the-loop: At what points in the drag-and-drop routing should the system pause for human approval?
​Conflict Resolution: If the "Security Agent" rejects code that the "Performance Agent" optimized, how does the Orchestrator arbitrate the conflict?

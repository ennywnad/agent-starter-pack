# Adventure: The Evolution of an Agent

Welcome to a new adventure! You've seen the [Architecture Guide](architecture_guide.md) and the [Learning Plan](learning_plan.md). Now, let's dive deep into the most critical file in any agent project: `agent.py`.

This guide will show you how `agent.py` transforms from a simple script into a complex, orchestrated system, mirroring the "Foundation," "Coordination," and "Enterprise" patterns.

**The Mission:** Understand how to recognize these patterns in the wild and know when it's time to evolve your own agent's architecture.

---

## Module 1: The Foundation Agent

**Goal:** Understand the simplest form of an agent: a single file, a single purpose.

### - [ ] Session 1: The Simple Script
*   **🎯 Objective:** Analyze the `agent.py` from the `adk_base` template.
*   **✅ Deliverable:** A clear understanding of a basic agent's structure.
*   **🚀 Let's Go! (Code Analysis):**
    Let's look at the code from `agents/adk_base/app/agent.py`. It's the "hello world" of agents.

    ```python
    # agents/adk_base/app/agent.py

    import datetime
    from zoneinfo import ZoneInfo
    from google.adk.agents import Agent

    def get_weather(query: str) -> str:
        """Simulates a web search for weather information."""
        if "san francisco" in query.lower():
            return "It's 60 degrees and foggy."
        return "It's 90 degrees and sunny."

    def get_current_time(query: str) -> str:
        """Simulates getting the current time for a city."""
        # ... implementation ...
        return f"The current time for query {query} is ..."

    # A single, root-level agent definition
    root_agent = Agent(
        name="root_agent",
        model="gemini-1.5-flash",
        instruction="You are a helpful AI assistant...",
        tools=[get_weather, get_current_time],
    )
    ```
*   **Key Takeaways:**
    *   **All-in-One:** Everything is in one file: tool functions and the agent definition.
    *   **Stateless:** The agent has no memory of past conversations. Each turn is a fresh start.
    *   **Direct:** The `root_agent` is a direct instance of the `Agent` class. What you see is what you get.
*   **🎉 Success!:** You can now spot a Foundation-level agent. It's simple, direct, and great for single-purpose tasks.

---

## Module 2: The Coordination Agent

**Goal:** Level up to an agent that can manage a multi-step process using a framework.

### - [ ] Session 2: The State Machine
*   **🎯 Objective:** Analyze the `agent.py` from the `langgraph_base_react` template.
*   **✅ Deliverable:** An understanding of how a graph-based agent works.
*   **🚀 Let's Go! (Code Analysis):**
    This agent uses `langgraph` to create a state machine, or a "flowchart" of logic. The agent is no longer a single object, but a compiled graph of nodes and edges.

    ```python
    # agents/langgraph_base_react/app/agent.py

    from langgraph.graph import END, MessagesState, StateGraph
    from langgraph.prebuilt import ToolNode
    # ... other imports

    # 1. Define tools and the LLM
    tools = [...]
    llm = ChatVertexAI(...).bind_tools(tools)

    # 2. Define graph components (nodes)
    def should_continue(state: MessagesState) -> str:
        # This node decides where to go next: call a tool or finish.
        return "tools" if state["messages"][-1].tool_calls else END

    def call_model(state: MessagesState, config) -> dict:
        # This node calls the LLM.
        response = llm.invoke(...)
        return {"messages": response}

    # 3. Create and compile the graph
    workflow = StateGraph(MessagesState)
    workflow.add_node("agent", call_model)
    workflow.add_node("tools", ToolNode(tools))
    workflow.set_entry_point("agent")
    workflow.add_conditional_edges("agent", should_continue) # <-- This is a decision point!
    workflow.add_edge("tools", "agent")

    agent = workflow.compile()

    # 4. The root agent is the compiled graph
    def root_agent(state: MessagesState, config) -> dict:
        return agent.invoke(state, config)
    ```
*   **Key Takeaways:**
    *   **Orchestration:** The `agent.py` file is now an *orchestrator*. Its job is to define the flow of logic, not just execute a single LLM call.
    *   **Stateful:** The `MessagesState` object holds the memory of the conversation, allowing for complex, multi-turn interactions.
    *   **Nodes & Edges:** The agent's logic is broken down into nodes (`call_model`, `tools`) and the connections between them (edges). This makes it easier to manage complex flows.
*   **🎉 Success!:** You can now identify a Coordination-level agent. It uses a framework (like LangGraph) to manage a stateful, multi-step process.

---

## Module 3: The Enterprise Agent

**Goal:** See how agents are built for production: modular, configurable, and collaborative.

### - [ ] Session 3: The Multi-Agent System
*   **🎯 Objective:** Analyze the `agent.py` from the `adk_gemini_fullstack` template.
*   **✅ Deliverable:** An understanding of how to compose a system of specialized agents.
*   **🚀 Let's Go! (Code Analysis):**
    In a production system, `agent.py` becomes a composition root. It doesn't contain the agent logic itself; it *assembles* the system from smaller, specialized pieces.

    ```python
    # agents/adk_gemini_fullstack/app/agent.py

    from google.adk.agents import LlmAgent, LoopAgent, SequentialAgent
    from .config import config  # <-- Imports settings from a separate file

    # --- Define Specialist Agents ---
    # Each agent has one specific job.

    plan_generator = LlmAgent(
        name="plan_generator",
        model=config.worker_model, # <-- Model is configurable
        instruction="You are a research strategist...",
    )

    section_researcher = LlmAgent(
        name="section_researcher",
        model=config.worker_model,
        instruction="You are a highly capable research agent...",
    )

    research_evaluator = LlmAgent(
        name="research_evaluator",
        model=config.critic_model, # <-- Uses a different, more powerful model
        instruction="You are a meticulous quality assurance analyst...",
    )

    # --- Compose Agents into a Pipeline ---
    # The specialists are assembled into a team.

    research_pipeline = SequentialAgent(
        name="research_pipeline",
        sub_agents=[
            section_researcher,
            LoopAgent( # <-- A loop for iterative refinement
                name="iterative_refinement_loop",
                sub_agents=[research_evaluator, ...],
            ),
            # ... other agents
        ],
    )

    # The root_agent starts the whole process by delegating to the team.
    root_agent = LlmAgent(
        name="interactive_planner_agent",
        instruction="You are a research planning assistant...",
        sub_agents=[research_pipeline], # <-- Delegates to the pipeline
    )
    ```
*   **Key Takeaways:**
    *   **Separation of Concerns:** Logic is broken down into specialized agents (`plan_generator`, `research_evaluator`). Prompts are long and detailed.
    *   **Configuration:** Key settings like model names are stored in a `config.py`, not hardcoded. This makes it easy to swap components.
    *   **Composition:** The `root_agent` is a "manager" that delegates tasks to a `SequentialAgent` or other composite agents. It orchestrates the work of the specialists.
*   **🎉 Success!:** You can now recognize an Enterprise-level agent system. It's modular, configurable, and built by composing smaller, specialized agents together.

## Congratulations!

You've completed your adventure through the evolution of `agent.py`! You can now identify the architectural patterns of agents in the wild and have a clear roadmap for scaling your own projects from a simple script to a production-ready system.

# Advanced Agent Adventures: From Theory to Production

Welcome to your next challenge! You've mastered the basics in the [Learning Plan](learning_plan.md), and now you're ready to dive deep into advanced agent patterns and build something production-ready.

## How This Adventure Works

This guide follows the same ADHD-friendly approach, but we're going deeper. We'll decode agent architectures, then build a real production agent that evolves through all three patterns.

-   **Micro-Learning:** Still 15-20 minute focused sessions.
-   **Theory + Practice:** Understand patterns, then immediately apply them.
-   **One Project Journey:** Build a CI/CD triage agent that grows from simple to enterprise-grade.
-   **Visual Progress:** Track your journey with checkboxes: `- [x]`.
-   **Real-World Value:** Build something that solves actual engineering problems.

**Our Promise:** We'll teach you to recognize and build Foundation, Coordination, and Enterprise agent patterns while keeping total costs under **$3**.

---

## Module 0: The Mission - Your Agent Evolution Journey

**Goal:** Understand what we're building and why each pattern matters.

### - [ ] Session 1: The Triage Agent Vision
*   **🎯 Objective:** Understand the real-world problem we'll solve and how it will evolve.
*   **🤔 Why it matters:** CI/CD triage is a perfect use case that grows naturally through all architectural patterns.
*   **🚀 The Vision:**
    *   **Foundation Version:** A simple script that reads a build log and suggests fixes.
    *   **Coordination Version:** A state machine that systematically analyzes logs, searches knowledge bases, and generates structured reports.
    *   **Enterprise Version:** A multi-agent system with specialist agents for parsing, analysis, and automated remediation.
*   **🎉 Success!:** You can visualize how one agent will grow from a 50-line script to a sophisticated production system.

---

## Module 1: Decoding Agent DNA (100% Free Analysis)

**Goal:** Learn to recognize the three fundamental agent patterns by analyzing real code.

### - [ ] Session 2: Foundation Pattern - The Simple Script
*   **🎯 Objective:** Analyze a Foundation-level agent and understand its characteristics.
*   **✅ Deliverable:** Ability to spot Foundation patterns in any codebase.
*   **🚀 Let's Go! (Code Analysis):**
    Let's examine the `adk_base` template. This is agent architecture at its simplest.

    ```python
    # agents/adk_base/app/agent.py - Foundation Pattern

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
        tz = ZoneInfo("America/Los_Angeles")
        now = datetime.datetime.now(tz)
        return f"The current time is {now.strftime('%I:%M %p')}"

    # Single, direct agent definition
    root_agent = Agent(
        name="root_agent",
        model="gemini-1.5-flash",
        instruction="You are a helpful AI assistant...",
        tools=[get_weather, get_current_time],
    )
    ```

*   **Foundation Pattern Checklist:**
    *   [ ] **All-in-One File:** Everything lives in one `agent.py` file
    *   [ ] **Direct Instantiation:** Creates an `Agent` object directly
    *   [ ] **Simple Tools:** Functions defined inline
    *   [ ] **Stateless:** No memory between conversations
    *   [ ] **Single Purpose:** Does one thing well

*   **🎉 Success!:** You can now identify Foundation agents instantly. Perfect for prototypes and single-purpose tasks.
*   **💰 Cost Check:** **$0.00**. Pure analysis.

### - [ ] Session 3: Coordination Pattern - The State Machine
*   **🎯 Objective:** Understand how agents coordinate complex, multi-step processes.
*   **✅ Deliverable:** Recognition of graph-based agent architectures.
*   **🚀 Let's Go! (Code Analysis):**
    The `langgraph_base_react` template shows us Coordination-level thinking.

    ```python
    # agents/langgraph_base_react/app/agent.py - Coordination Pattern

    from langgraph.graph import END, MessagesState, StateGraph
    from langgraph.prebuilt import ToolNode
    # ... other imports

    # 1. Define the components
    tools = [get_weather, get_current_time]
    llm = ChatVertexAI(model_name="gemini-1.5-flash").bind_tools(tools)

    # 2. Define the decision logic
    def should_continue(state: MessagesState) -> str:
        """Decides whether to use tools or finish."""
        last_message = state["messages"][-1]
        return "tools" if last_message.tool_calls else END

    def call_model(state: MessagesState, config) -> dict:
        """Invokes the LLM and returns its response."""
        response = llm.invoke(state["messages"], config)
        return {"messages": response}

    # 3. Build the graph (the "brain" of coordination)
    workflow = StateGraph(MessagesState)
    workflow.add_node("agent", call_model)
    workflow.add_node("tools", ToolNode(tools))
    workflow.set_entry_point("agent")
    workflow.add_conditional_edges("agent", should_continue)  # Decision point!
    workflow.add_edge("tools", "agent")

    # 4. Compile into executable agent
    agent = workflow.compile()

    def root_agent(state: MessagesState, config) -> dict:
        return agent.invoke(state, config)
    ```

*   **Coordination Pattern Checklist:**
    *   [ ] **State Management:** Uses `MessagesState` to track conversation
    *   [ ] **Decision Nodes:** Has conditional logic (`should_continue`)
    *   [ ] **Graph Architecture:** Builds a flowchart of agent behavior
    *   [ ] **Compiled Logic:** Creates executable workflow from components
    *   [ ] **Multi-Step Process:** Can loop, branch, and make complex decisions

*   **🎉 Success!:** You recognize agents that can "think through" complex problems step by step.
*   **💰 Cost Check:** **$0.00**. Still analyzing.

### - [ ] Session 4: Enterprise Pattern - The Multi-Agent System
*   **🎯 Objective:** See how production systems compose specialized agents into teams.
*   **✅ Deliverable:** Understanding of agent composition and delegation patterns.
*   **🚀 Let's Go! (Code Analysis):**
    The `adk_gemini_fullstack` template shows enterprise-scale agent architecture.

    ```python
    # agents/adk_gemini_fullstack/app/agent.py - Enterprise Pattern

    from google.adk.agents import LlmAgent, LoopAgent, SequentialAgent
    from .config import config  # External configuration

    # --- Specialist Agents (Each has one job) ---

    plan_generator = LlmAgent(
        name="plan_generator",
        model=config.worker_model,  # Configurable models
        instruction="""You are a research strategist. Your job is to break down 
        complex research questions into manageable, specific tasks...""",
    )

    section_researcher = LlmAgent(
        name="section_researcher", 
        model=config.worker_model,
        instruction="""You are a highly capable research agent. Take a specific 
        research task and gather comprehensive information...""",
    )

    research_evaluator = LlmAgent(
        name="research_evaluator",
        model=config.critic_model,  # Different model for evaluation
        instruction="""You are a meticulous quality assurance analyst. Review 
        research work and determine if it meets standards...""",
    )

    # --- Team Assembly (Specialists work together) ---

    research_pipeline = SequentialAgent(
        name="research_pipeline",
        sub_agents=[
            section_researcher,
            LoopAgent(  # Iterative refinement loop
                name="iterative_refinement_loop", 
                sub_agents=[research_evaluator, section_researcher],
                max_iterations=3,
            ),
        ],
    )

    # --- The Manager (Orchestrates the team) ---
    root_agent = LlmAgent(
        name="interactive_planner_agent",
        instruction="""You are a research planning assistant. You coordinate 
        with a team of specialist agents to complete research tasks...""",
        sub_agents=[research_pipeline],  # Delegates to the team
    )
    ```

*   **Enterprise Pattern Checklist:**
    *   [ ] **Specialist Agents:** Each agent has a single, well-defined role
    *   [ ] **External Configuration:** Settings stored in separate config files
    *   [ ] **Composition Root:** `agent.py` assembles the system
    *   [ ] **Team Coordination:** Uses `SequentialAgent`, `LoopAgent` to organize work
    *   [ ] **Delegation:** Root agent manages, doesn't do the work itself

*   **🎉 Success!:** You can identify production-ready, team-based agent architectures.
*   **💰 Cost Check:** **$0.00**. Analysis complete.

### - [ ] Session 5: Pattern Recognition Challenge
*   **🎯 Objective:** Test your ability to classify agent patterns in the wild.
*   **✅ Deliverable:** Confidence in architectural decision-making.
*   **🚀 Let's Go! (Challenge Questions):**
    
    **Code Snippet A:**
    ```python
    agent = Agent(name="helper", model="gpt-4", instruction="Be helpful", tools=[calculator])
    ```
    **Pattern:** Foundation ✓ (Direct instantiation, single file, simple)

    **Code Snippet B:**
    ```python
    workflow = StateGraph(MessagesState)
    workflow.add_node("analyze", analyze_node)
    workflow.add_conditional_edges("analyze", decide_next_step)
    agent = workflow.compile()
    ```
    **Pattern:** Coordination ✓ (State graph, decision nodes, workflow)

    **Code Snippet C:**
    ```python
    root_agent = ManagerAgent(
        sub_agents=[data_collector, analyzer, report_generator],
        coordination_strategy="sequential"
    )
    ```
    **Pattern:** Enterprise ✓ (Delegation, specialist agents, composition)

*   **🎉 Success!:** You can now classify any agent architecture and understand when to use each pattern.

---

## Module 2: Building Your Production Agent (~$0.75 total)

**Goal:** Build a real CI/CD triage agent that starts simple and evolves through all patterns.

### - [ ] Session 6: Foundation Version - The Simple Triage Script
*   **🎯 Objective:** Create a basic agent that can read and analyze build logs.
*   **✅ Deliverable:** A working Foundation-pattern triage agent.
*   **🚀 Let's Go! (Build Steps):**
    1.  Create the project with the simplest template:
        ```bash
        agent-cli create
        # Name: triage-agent-foundation
        # Template: adk_base
        # Target: cloud_run  
        # Frontend: none
        ```
    2.  Navigate to the project:
        ```bash
        cd triage-agent-foundation
        ```
    3.  Create a sample build log for testing:
        ```bash
        cat > sample_log.txt << 'EOF'
        ============================= test session starts ==============================
        platform linux -- Python 3.10.12, pytest-7.4.0
        collected 1 item

        tests/test_app.py F                                                     [100%]

        =================================== FAILURES ===================================
        _________________________________ test_login _________________________________

            def test_login():
                response = client.post("/login", json={"user": "test", "pass": "wrong"})
        >       assert response.status_code == 200
        E       assert 401 == 200
        E        +  where 401 = <Response [401]>.status_code

        tests/test_app.py:10: AssertionError
        ============================== 1 failed in 1.23s
        ERROR: Build failed.
        EOF
        ```
    4.  Edit `app/agent.py` to add triage capabilities:
        ```python
        # Replace the existing content with:
        from google.adk.agents import Agent

        def read_build_log(file_path: str) -> str:
            """Reads a build log file and returns its contents."""
            try:
                with open(file_path, 'r') as f:
                    return f.read()
            except FileNotFoundError:
                return f"Error: File not found at {file_path}"
            except Exception as e:
                return f"Error reading file: {str(e)}"

        root_agent = Agent(
            name="triage_agent_foundation",
            model="gemini-1.5-flash",
            instruction="""You are a CI/CD triage specialist. When given a build log:

            1. Identify the specific error or failure
            2. Explain what went wrong in simple terms  
            3. Suggest a concrete fix

            Always structure your response with:
            - **Summary:** One sentence about the problem
            - **Root Cause:** Detailed explanation
            - **Fix:** Specific action to take""",
            tools=[read_build_log],
        )
        ```
    5.  Test the agent locally:
        ```bash
        uv sync
        agent-cli run
        ```
        **Prompt:** `Please analyze the build failure in sample_log.txt`

*   **🎉 Success!:** Your Foundation agent can read logs and provide basic triage! This is exactly how most production agents start.
*   **🤔 Troubleshooting & FAQ:**
    *   **API Key Error:** Export your `GOOGLE_API_KEY` if you haven't already.
    *   **File Path Issues:** Make sure `sample_log.txt` is in the project root.
*   **💰 Cost Check:** **$0.00**. Gemini free tier covers this easily.

### - [ ] Session 7: Add Intelligence with RAG
*   **🎯 Objective:** Enhance the agent with knowledge retrieval capabilities.
*   **✅ Deliverable:** An agent that can search past failures for similar patterns.
*   **🚀 Let's Go! (Enhancement Steps):**
    1.  Create a knowledge base of past failures:
        ```bash
        mkdir knowledge_base
        cat > knowledge_base/common_failures.txt << 'EOF'
        Login Test Failures:
        - 401 Unauthorized: Usually means test credentials are incorrect
        - Check test data setup, verify mock user credentials
        - Common fix: Update test fixtures with valid user/password

        Database Connection Failures:
        - Connection refused: Database service not running in CI
        - Common fix: Add database service to CI configuration
        - Check environment variables for database URL

        Import Errors:
        - ModuleNotFoundError: Missing dependency or wrong path
        - Common fix: Check requirements.txt and PYTHONPATH
        - Verify virtual environment is activated
        EOF
        ```
    2.  Add a knowledge search tool to `app/agent.py`:
        ```python
        import os
        import glob

        def search_knowledge_base(query: str) -> str:
            """Searches the knowledge base for relevant information."""
            knowledge_files = glob.glob("knowledge_base/*.txt")
            results = []
            
            for file_path in knowledge_files:
                try:
                    with open(file_path, 'r') as f:
                        content = f.read()
                        # Simple keyword matching
                        if any(keyword.lower() in content.lower() for keyword in query.split()):
                            results.append(f"From {os.path.basename(file_path)}:\n{content}")
                except Exception as e:
                    continue
            
            if results:
                return "\n\n".join(results)
            return "No relevant information found in knowledge base."

        # Update the agent tools
        root_agent = Agent(
            name="triage_agent_foundation",
            model="gemini-1.5-flash", 
            instruction="""You are a CI/CD triage specialist. When analyzing failures:

            1. Read the build log to understand the error
            2. Search the knowledge base for similar past failures
            3. Combine log analysis with historical knowledge
            4. Provide structured recommendations

            Always structure your response with:
            - **Summary:** One sentence about the problem
            - **Root Cause:** Detailed explanation with log references
            - **Historical Context:** What the knowledge base says about similar issues
            - **Recommended Fix:** Specific action based on both current and past data""",
            tools=[read_build_log, search_knowledge_base],
        )
        ```
    3.  Test the enhanced agent:
        ```bash
        agent-cli run
        ```
        **Prompt:** `Analyze sample_log.txt and check for similar past failures`

*   **🎉 Success!:** Your agent now combines current analysis with historical knowledge - a key RAG pattern!
*   **💰 Cost Check:** **~$0.25**. Still very affordable.

### - [ ] Session 8: Deploy Your Foundation Agent
*   **🎯 Objective:** Get your triage agent running in the cloud.
*   **✅ Deliverable:** A public API endpoint for your CI/CD triage agent.
*   **🚀 Let's Go! (Deployment Steps):**
    1.  Make sure you're logged into Google Cloud:
        ```bash
        gcloud auth login
        gcloud config set project YOUR_PROJECT_ID
        ```
    2.  Deploy the agent:
        ```bash
        agent-cli deploy
        ```
        Wait for the deployment to complete and note the Service URL.
    3.  Test the live API:
        ```bash
        curl -X POST YOUR_SERVICE_URL/run \
        -H "Content-Type: application/json" \
        -H "X-Goog-Api-Key: YOUR_GOOGLE_API_KEY" \
        -d '{"prompt": "Analyze sample_log.txt and provide triage recommendations"}'
        ```

*   **🎉 Success!:** You have a production CI/CD triage agent running in the cloud! Real engineering teams could use this.
*   **💰 Cost Check:** **~$0.50**. Cloud Run + Artifact Registry storage.

<details>
<summary><strong>🧠 Real-World Integration Ideas (Optional, 5 min)</strong></summary>

Your Foundation agent is already useful! Here's how real teams could integrate it:

**Webhook Integration:**
```bash
# GitHub Actions could POST to your agent when builds fail
curl -X POST YOUR_SERVICE_URL/run \
  -H "Content-Type: application/json" \
  -H "X-Goog-Api-Key: $API_KEY" \
  -d "{\"prompt\": \"Analyze this failed build: $BUILD_LOG_URL\"}"
```

**Slack Bot Integration:**
- Connect your agent to Slack
- Engineers type `/triage build-123` to get instant analysis
- Agent posts structured reports directly to channels

**Production Considerations:**
- Add rate limiting to prevent API abuse
- Implement authentication for team access only  
- Add monitoring to track triage accuracy

</details>

---

## Module 3: Evolving to Coordination Pattern (~$1.25 total)

**Goal:** Transform your Foundation agent into a sophisticated Coordination-level system.

### - [ ] Session 9: Understanding the Coordination Upgrade
*   **🎯 Objective:** Plan how to convert your simple agent into a state machine.
*   **✅ Deliverable:** A clear architecture plan for the coordination upgrade.
*   **🚀 Let's Go! (Planning):**
    
    **Current Foundation Flow:**
    ```
    User Prompt → Agent → Tool(s) → LLM → Response
    ```

    **New Coordination Flow:**
    ```
    User Prompt → Parse Request → Read Log → Analyze Patterns → 
    Search Knowledge → Generate Report → Validate Quality → Response
    ```

    **State Machine Design:**
    - **Entry Node:** `parse_request` - Understand what the user wants
    - **Data Node:** `collect_data` - Read logs, search knowledge base
    - **Analysis Node:** `analyze_failure` - Process the information  
    - **Quality Node:** `validate_report` - Check if analysis is complete
    - **Decision Node:** `should_continue` - Decide if more data is needed
    - **Exit Node:** `generate_final_report` - Format final response

*   **🎉 Success!:** You understand how state machines provide systematic, repeatable analysis processes.

### - [ ] Session 10: Build the Coordination Version
*   **🎯 Objective:** Create a new LangGraph-based version of your triage agent.
*   **✅ Deliverable:** A stateful agent that follows a systematic triage process.
*   **🚀 Let's Go! (Build Steps):**
    1.  Create the coordination version:
        ```bash
        agent-cli create
        # Name: triage-agent-coordination  
        # Template: langgraph_base_react
        # Target: cloud_run
        # Frontend: none
        ```
    2.  Copy your assets:
        ```bash
        cd triage-agent-coordination
        cp ../triage-agent-foundation/sample_log.txt .
        cp -r ../triage-agent-foundation/knowledge_base .
        ```
    3.  Replace `app/agent.py` with a state machine version:
        ```python
        from typing import Literal
        from langgraph.graph import END, MessagesState, StateGraph
        from langchain_google_vertexai import ChatVertexAI
        from langchain_core.messages import HumanMessage, SystemMessage
        import os
        import glob

        # Define our tools (same as before)
        def read_build_log(file_path: str) -> str:
            """Reads a build log file and returns its contents."""
            try:
                with open(file_path, 'r') as f:
                    return f.read()
            except Exception as e:
                return f"Error reading file: {str(e)}"

        def search_knowledge_base(query: str) -> str:
            """Searches knowledge base for relevant information."""
            knowledge_files = glob.glob("knowledge_base/*.txt")
            results = []
            
            for file_path in knowledge_files:
                try:
                    with open(file_path, 'r') as f:
                        content = f.read()
                        if any(keyword.lower() in content.lower() for keyword in query.split()):
                            results.append(f"From {os.path.basename(file_path)}:\n{content}")
                except Exception:
                    continue
            
            return "\n\n".join(results) if results else "No relevant info found."

        # Initialize the LLM
        llm = ChatVertexAI(model_name="gemini-1.5-flash")

        # Define state machine nodes
        def parse_request(state: MessagesState) -> dict:
            """Parse the user's request to understand what they want analyzed."""
            last_message = state["messages"][-1].content
            
            system_msg = SystemMessage(content="""
            You are a request parser. Extract the file path from user requests.
            If you find a file path (like 'sample_log.txt'), respond with just the path.
            If no file path is found, respond with 'NO_FILE_SPECIFIED'.
            """)
            
            response = llm.invoke([system_msg, state["messages"][-1]])
            return {"messages": state["messages"] + [response]}

        def collect_data(state: MessagesState) -> dict:
            """Collect log data and search knowledge base."""
            # Extract file path from previous response
            file_path = state["messages"][-1].content.strip()
            
            if file_path == "NO_FILE_SPECIFIED":
                log_content = "No log file specified."
            else:
                log_content = read_build_log(file_path)
            
            # Search knowledge base based on log content
            knowledge = search_knowledge_base(log_content)
            
            data_summary = f"""
            LOG CONTENT:
            {log_content}
            
            KNOWLEDGE BASE SEARCH:
            {knowledge}
            """
            
            return {"messages": state["messages"] + [HumanMessage(content=data_summary)]}

        def analyze_failure(state: MessagesState) -> dict:
            """Perform detailed analysis of the failure."""
            system_msg = SystemMessage(content="""
            You are a CI/CD failure analyst. Based on the log content and knowledge base information provided, 
            create a structured analysis with:
            
            - **Summary:** One sentence problem description
            - **Root Cause:** Detailed explanation with specific log references  
            - **Historical Context:** Insights from knowledge base
            - **Recommended Fix:** Specific actionable steps
            
            Be thorough and reference specific lines/errors from the logs.
            """)
            
            response = llm.invoke([system_msg] + state["messages"][-2:])
            return {"messages": state["messages"] + [response]}

        def should_continue(state: MessagesState) -> Literal["analyze", END]:
            """Decide if analysis is complete or needs more work."""
            last_response = state["messages"][-1].content
            
            # Simple check: if response is too short, continue analyzing
            if len(last_response) < 200:
                return "analyze"
            
            # Check if all required sections are present
            required_sections = ["Summary", "Root Cause", "Recommended Fix"]
            if all(section in last_response for section in required_sections):
                return END
            
            return "analyze"

        # Build the state machine
        workflow = StateGraph(MessagesState)

        # Add all the nodes
        workflow.add_node("parse", parse_request)
        workflow.add_node("collect", collect_data)
        workflow.add_node("analyze", analyze_failure)

        # Define the flow
        workflow.set_entry_point("parse")
        workflow.add_edge("parse", "collect")
        workflow.add_edge("collect", "analyze") 
        workflow.add_conditional_edges("analyze", should_continue)

        # Compile the agent
        agent = workflow.compile()

        def root_agent(state: MessagesState, config=None) -> dict:
            """Entry point for the coordination agent."""
            return agent.invoke(state, config)
        ```
    4.  Test the coordination agent:
        ```bash
        uv sync
        agent-cli run
        ```
        **Prompt:** `Please perform a systematic analysis of sample_log.txt`

*   **🎉 Success!:** Your agent now follows a systematic, multi-step process! You can see each state transition in the output.
*   **💰 Cost Check:** **~$0.75**. More complex processing, but still affordable.

### - [ ] Session 11: Compare Foundation vs Coordination
*   **🎯 Objective:** Understand the practical differences between the two patterns.
*   **✅ Deliverable:** Clear knowledge of when to use each pattern.
*   **🚀 Let's Go! (Comparison):**

    **Foundation Version:**
    - ✅ Simple, fast development
    - ✅ Easy to understand and debug  
    - ✅ Low latency (single LLM call)
    - ❌ Limited reasoning ability
    - ❌ No systematic process
    - ❌ Hard to ensure consistency

    **Coordination Version:**
    - ✅ Systematic, repeatable process
    - ✅ Better quality control
    - ✅ Easier to add new analysis steps
    - ✅ Transparent reasoning (see each step)
    - ❌ More complex to develop
    - ❌ Higher latency (multiple steps)
    - ❌ More expensive (more LLM calls)

    **When to Use Each:**
    - **Foundation:** Prototypes, simple tasks, cost-sensitive applications
    - **Coordination:** Production systems, complex analysis, quality-critical applications

*   **🎉 Success!:** You can now make informed architectural decisions based on requirements.

---

## Module 4: Enterprise Evolution (~$1.00 total)

**Goal:** Build a multi-agent Enterprise system with specialized roles.

### - [ ] Session 12: Design the Enterprise Team
*   **🎯 Objective:** Plan a team of specialist agents for comprehensive CI/CD triage.
*   **✅ Deliverable:** An architecture for a multi-agent triage system.
*   **🚀 Let's Go! (Team Design):**

    **The Specialist Agents:**
    ```
    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
    │  Log Parser     │    │   Failure       │    │   Solution      │
    │                 │    │   Analyzer      │    │   Recommender   │
    │ - Extract errors│    │ - Classify type │    │ - Suggest fixes │
    │ - Find patterns │    │ - Assess impact │    │ - Priority level│
    │ - Structure data│    │ - Root cause    │    │ - Next steps    │
    └─────────────────┘    └─────────────────┘    └─────────────────┘
            │                        │                        │
            └────────────────────────┼────────────────────────┘
                                     │
                            ┌─────────────────┐
                            │   Report        │
                            │   Generator     │
                            │                 │
                            │ - Combine work  │
                            │ - Format output │
                            │ - Quality check │
                            └─────────────────┘
    ```

    **Agent Responsibilities:**
    - **Log Parser:** Extracts structured data from messy logs
    - **Failure Analyzer:** Classifies failure types and determines root causes  
    - **Solution Recommender:** Searches knowledge base and suggests fixes
    - **Report Generator:** Combines insights into final report

*   **🎉 Success!:** You have a blueprint for a production-scale triage system.

### - [ ] Session 13: Build the Enterprise System
*   **🎯 Objective:** Implement the multi-agent triage team.
*   **✅ Deliverable:** A fully functional enterprise-grade CI/CD triage system.
*   **🚀 Let's Go! (Implementation):**
    1.  Create the enterprise version:
        ```bash
        agent-cli create
        # Name: triage-agent-enterprise
        # Template: adk_gemini_fullstack  
        # Target: cloud_run
        # Frontend: none
        ```
    2.  Set up the project structure:
        ```bash
        cd triage-agent-enterprise
        cp ../triage-agent-foundation/sample_log.txt .
        cp -r ../triage-agent-foundation/knowledge_base .
        ```
    3.  Replace `app/agent.py` with the enterprise version:
        ```python
        from google.adk.agents import LlmAgent, SequentialAgent
        import os

        # --- Specialist Agents ---

        log_parser = LlmAgent(
            name="log_parser",
            model="gemini-1.5-flash",
            instruction="""You are a log parsing specialist. Your job is to:
            
            1. Extract all error messages and stack traces
            2. Identify the failing test or component
            3. Find error codes and status codes
            4. Structure the findings into clear categories
            
            Output format:
            **ERRORS FOUND:**
            - [List each error with line numbers]
            
            **FAILING COMPONENTS:**
            - [Test files, functions, etc.]
            
            **STATUS CODES:**
            - [HTTP codes, exit codes, etc.]
            """,
        )

        failure_analyzer = LlmAgent(
            name="failure_analyzer", 
            model="gemini-1.5-flash",
            instruction="""You are a failure analysis expert. Based on parsed log data:
            
            1. Classify the failure type (auth, network, syntax, logic, etc.)
            2. Assess the impact level (critical, major, minor)
            3. Determine the most likely root cause
            4. Identify related system components
            
            Output format:
            **FAILURE CLASSIFICATION:**
            - Type: [category]
            - Impact: [level]
            
            **ROOT CAUSE ANALYSIS:**
            - Most likely cause: [explanation]
            - Contributing factors: [list]
            """,
        )

        solution_recommender = LlmAgent(
            name="solution_recommender",
            model="gemini-1.5-flash", 
            instruction="""You are a solution specialist. Based on failure analysis:
            
            1. Search for similar past issues in knowledge base
            2. Recommend specific fixes with priority
            3. Suggest preventive measures
            4. Estimate fix complexity and timeline
            
            Output format:
            **RECOMMENDED SOLUTIONS:**
            1. [High priority fix]
            2. [Medium priority fix]
            
            **PREVENTION:**
            - [How to avoid this in future]
            
            **COMPLEXITY:** [Simple/Medium/Complex - estimated time]
            """,
        )

        report_generator = LlmAgent(
            name="report_generator",
            model="gemini-1.5-pro",  # Use more powerful model for final report
            instruction="""You are a report synthesis specialist. Combine all previous analysis into a comprehensive, executive-ready report.
            
            Create a structured report with:
            - Executive Summary (2-3 sentences)
            - Technical Details (parsed data + analysis)  
            - Recommended Actions (prioritized solutions)
            - Prevention Strategy
            
            Make it clear, actionable, and professional.
            """,
        )

        # --- Team Assembly ---
        triage_pipeline = SequentialAgent(
            name="triage_pipeline",
            sub_agents=[
                log_parser,
                failure_analyzer, 
                solution_recommender,
                report_generator,
            ],
        )

        # --- The Manager Agent ---
        root_agent = LlmAgent(
            name="enterprise_triage_manager",
            model="gemini-1.5-flash",
            instruction="""You are the CI/CD Triage Manager. You coordinate a team of specialists to provide comprehensive failure analysis.
            
            When given a build failure:
            1. Direct the team through systematic analysis
            2. Ensure all aspects are covered
            3. Present the final comprehensive report
            
            Always start by explaining what your team will do, then delegate to the triage_pipeline.""",
            sub_agents=[triage_pipeline],
        )
        ```
    4.  Add the tools to a separate file `app/tools.py`:
        ```python
        import os
        import glob

        def read_build_log(file_path: str) -> str:
            """Reads a build log file and returns its contents."""
            try:
                with open(file_path, 'r') as f:
                    return f.read()
            except Exception as e:
                return f"Error reading file: {str(e)}"

        def search_knowledge_base(query: str) -> str:
            """Searches knowledge base for relevant information."""
            knowledge_files = glob.glob("knowledge_base/*.txt")
            results = []
            
            for file_path in knowledge_files:
                try:
                    with open(file_path, 'r') as f:
                        content = f.read()
                        if any(keyword.lower() in content.lower() for keyword in query.split()):
                            results.append(f"From {os.path.basename(file_path)}:\n{content}")
                except Exception:
                    continue
            
            return "\n\n".join(results) if results else "No relevant information found."
        ```
    5.  Update `app/agent.py` to use the tools:
        ```python
        # Add this import at the top
        from .tools import read_build_log, search_knowledge_base

        # Update each specialist agent to include tools
        log_parser = LlmAgent(
            name="log_parser",
            model="gemini-1.5-flash",
            tools=[read_build_log],  # Parser can read logs
            instruction="""...""",  # (same instruction as before)
        )

        solution_recommender = LlmAgent(
            name="solution_recommender", 
            model="gemini-1.5-flash",
            tools=[search_knowledge_base],  # Recommender can search knowledge
            instruction="""...""",  # (same instruction as before)
        )
        ```
    6.  Test the enterprise system:
        ```bash
        uv sync
        agent-cli run
        ```
        **Prompt:** `My team needs a comprehensive analysis of the failure in sample_log.txt`

*   **🎉 Success!:** You now have a full enterprise-grade multi-agent system! Watch as each specialist does their part.
*   **💰 Cost Check:** **~$1.00**. Multiple agents working together, but still very reasonable.

### - [ ] Session 14: Deploy and Compare All Patterns
*   **🎯 Objective:** Deploy the enterprise system and compare all three architectural patterns.
*   **✅ Deliverable:** Three deployed agents showcasing Foundation, Coordination, and Enterprise patterns.
*   **🚀 Let's Go! (Final Deployment):**
    1.  Deploy the enterprise system:
        ```bash
        agent-cli deploy
        ```
    2.  Test all three deployed versions:
        
        **Foundation Agent** (Simple & Fast):
        ```bash
        curl -X POST YOUR_FOUNDATION_URL/run \
        -H "Content-Type: application/json" \
        -H "X-Goog-Api-Key: YOUR_API_KEY" \
        -d '{"prompt": "Quick analysis of sample_log.txt"}'
        ```
        
        **Coordination Agent** (Systematic Process):
        ```bash
        curl -X POST YOUR_COORDINATION_URL/run \
        -H "Content-Type: application/json" \
        -H "X-Goog-Api-Key: YOUR_API_KEY" \
        -d '{"prompt": "Systematic analysis of sample_log.txt"}'
        ```
        
        **Enterprise Agent** (Team-Based Deep Analysis):
        ```bash
        curl -X POST YOUR_ENTERPRISE_URL/run \
        -H "Content-Type: application/json" \
        -H "X-Goog-Api-Key: YOUR_API_KEY" \
        -d '{"prompt": "Comprehensive team analysis of sample_log.txt"}'
        ```

*   **🎉 Success!:** You have built and deployed the same agent across all three architectural patterns!

### - [ ] Session 15: Clean Up and Reflect
*   **🎯 Objective:** Clean up cloud resources and reflect on the journey.
*   **✅ Deliverable:** Clean GCP project and clear understanding of architectural evolution.
*   **🚀 Let's Go! (Cleanup):**
    1.  Clean up all three deployments:
        ```bash
        # For each project directory:
        cd triage-agent-foundation && agent-cli teardown
        cd ../triage-agent-coordination && agent-cli teardown  
        cd ../triage-agent-enterprise && agent-cli teardown
        ```
    2.  **Reflection Questions:**
        - Which pattern would you choose for a quick prototype? (Foundation)
        - Which pattern ensures consistent, quality analysis? (Coordination)
        - Which pattern would you use for a production system serving multiple teams? (Enterprise)
        - What's the relationship between complexity and capability? (More complex = more capable, but higher cost)

*   **🎉 Success!:** You've experienced the full agent evolution journey!
*   **💰 Final Cost Check:** **~$2.50 total**. Well under our $3 budget!

---

## Congratulations! 🎉

You've completed an incredible journey! You can now:

✅ **Recognize** any agent architecture pattern in the wild  
✅ **Build** agents using Foundation, Coordination, and Enterprise patterns  
✅ **Choose** the right pattern for different requirements  
✅ **Evolve** agents from simple scripts to production systems  
✅ **Deploy** and manage cloud-based agent systems

### Your Next Adventures

Now that you understand agent evolution, you're ready for advanced topics:

- **🔄 Multi-Agent Workflows:** Build agents that collaborate on complex tasks
- **🧠 Advanced RAG:** Implement vector databases and semantic search
- **🛡️ Production Security:** Add authentication, rate limiting, and monitoring
- **🤖 Agent Orchestration:** Use tools like CrewAI and AutoGen for team coordination

You've mastered the fundamentals. The agent world is yours to explore! 🚀
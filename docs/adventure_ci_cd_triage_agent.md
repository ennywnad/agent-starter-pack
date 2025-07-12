# Adventure: Building a CI/CD Triage Agent

Welcome to your next adventure! You've mastered the basics in the [Learning Plan](learning_plan.md), and now you're ready for a real-world challenge. In this guide, we'll build a sophisticated agent that can diagnose failed CI/CD pipeline runs.

**The Mission:** Create an agent that can read a build log, identify the error, and suggest a solution. This is a high-value agent that can save development teams hours of tedious work.

## How This Adventure Works

This guide assumes you've completed the first Learning Plan. We'll move faster and focus more on the "why" behind our choices.

-   **Advanced Concepts:** We'll introduce tool use, RAG (Retrieval-Augmented Generation), and more complex agent logic.
-   **Real-World Problem:** We're solving a genuine problem that engineering teams face every day.
-   **Building on a Foundation:** We'll use a more advanced template and customize it significantly.

---

## Module 1: The Triage Agent's Foundation

**Goal:** Create an agent capable of using tools and searching for information.

### - [ ] Session 1: Create the Triage Agent
*   **�� Objective:** Create a new agent project using a template that supports RAG and tool use.
*   **✅ Deliverable:** A new folder containing the code for our triage agent.
*   **🚀 Let's Go! (Steps):**
    1.  Navigate to the directory where you store your projects.
    2.  Run the `create` command, but this time we'll use the `agentic_rag` template.
        ```bash
        agent-cli create
        ```
    3.  The CLI will ask you a few questions. Here’s how to answer:
        *   `Enter a name for your agent...`: **`triage-agent`**
        *   `Select a template...`: Use the arrow keys to select **`agentic_rag`** and press Enter.
        *   `Select a deployment target...`: Select **`cloud_run`**.
        *   `Select a frontend...`: Select **`none`**.
*   **🎉 Success!:** A new directory named `triage-agent` is created. `cd` into it and look around. You'll notice it's more complex than the `adk_base` template.

### - [ ] Session 2: Understanding the `agentic_rag` Template
*   **🎯 Objective:** Understand the key files and concepts in our new agent.
*   **✅ Deliverable:** A mental map of the project structure.
*   **🚀 Let's Go! (Files to review):**
    1.  **`app/agent.py`**: The main entry point. Notice how it's set up to use a `Tool` and a `retriever`.
    2.  **`app/retrievers.py`**: This is where the RAG logic lives. By default, it's set up to search for information. We'll use this to search a knowledge base of past failures.
    3.  **`app/templates.py`**: This file contains the core prompts, or "templates," that define the agent's behavior. We'll be modifying this heavily.
*   **🎉 Success!:** You've reviewed the key files. Don't worry if it's not all crystal clear yet. The important takeaway is that this agent is designed to **retrieve** information and **use tools**.

---

## Module 2: Giving Your Agent Tools

**Goal:** Create a custom tool that allows the agent to read and analyze a build log.

### - [ ] Session 3: Define a Custom Tool
*   **🎯 Objective:** Create a new Python tool that can read a file (our simulated build log).
*   **✅ Deliverable:** A new `tools.py` file with a `read_build_log` function.
*   **🚀 Let's Go! (Steps):**
    1.  In the `triage-agent/app` directory, create a new file named `tools.py`.
    2.  Add the following code to `app/tools.py`. We'll use the ADK's `@tool` decorator to define our tool.

        ```python
        from adk.api.tools import tool

        @tool
        def read_build_log(log_file_path: str) -> str:
            """
            Reads the content of a build log file and returns it as a string.

            Args:
                log_file_path: The path to the build log file.
            """
            try:
                with open(log_file_path, 'r') as f:
                    return f.read()
            except FileNotFoundError:
                return f"Error: Log file not found at {log_file_path}"
            except Exception as e:
                return f"An unexpected error occurred: {str(e)}"
        ```
*   **🎉 Success!:** You've created your first custom tool! The docstring is especially important, as the agent will use it to understand what the tool does and how to use it.

### - [ ] Session 4: Integrate the Tool into Your Agent
*   **🎯 Objective:** Make the agent aware of the new tool.
*   **✅ Deliverable:** An updated `agent.py` that imports and uses the `read_build_log` tool.
*   **🚀 Let's Go! (Steps):**
    1.  Open `app/agent.py`.
    2.  Import your new tool at the top of the file:
        ```python
        from app.tools import read_build_log
        ```
    3.  In the `Agent` class constructor (`__init__`), add the tool to the `tools` list:
        ```python
        # existing code...
        class Agent:
            def __init__(self, model: GenerativeModel):
                self.model = model
                self.tools = [read_build_log] # Add your tool here
                # existing code...
        ```
*   **🎉 Success!:** Your agent now has a new capability! It can decide on its own when to use the `read_build_log` function to get more information.

---

## Module 3: The Triage Logic

**Goal:** Teach the agent how to think like a CI/CD triage expert.

### - [ ] Session 5: Create a Test Case
*   **🎯 Objective:** Create a sample build log file that the agent can analyze.
*   **✅ Deliverable:** A file named `sample_log.txt` with a typical error.
*   **🚀 Let's Go! (Steps):**
    1.  In the root of your `triage-agent` directory, create a file named `sample_log.txt`.
    2.  Paste the following content into it. This simulates a common Python test failure.
        ```
        ...
        INFO: Starting integration tests...
        ============================= test session starts ==============================
        platform linux -- Python 3.10.12, pytest-7.4.0, pluggy-1.0.0
        rootdir: /build/src
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
        =========================== short test summary info ============================
        FAILED tests/test_app.py::test_login - assert 401 == 200
        ============================== 1 failed in 1.23s ===============================
        ERROR: Build failed.
        ```
*   **🎉 Success!:** You now have a realistic test case to challenge your agent with.

### - [ ] Session 6: Craft the Master Prompt
*   **🎯 Objective:** Modify the agent's system prompt to guide its triage process.
*   **✅ Deliverable:** An updated `templates.py` file.
*   **🚀 Let's Go! (Steps):**
    1.  Open `app/templates.py`.
    2.  Find the `SYSTEM_INSTRUCTION` variable.
    3.  Replace the existing prompt with a more specific one for our use case:
        ```python
        SYSTEM_INSTRUCTION = """
        You are a CI/CD Triage Agent. Your mission is to analyze failed build logs,
        identify the root cause, and provide a clear, concise report.

        Follow these steps:
        1.  When given a file path, use the `read_build_log` tool to read the log content.
        2.  Analyze the log for specific error messages, focusing on sections like "FAILURES" or "ERROR".
        3.  Synthesize your findings into a report with three sections:
            - **Summary:** A one-sentence summary of the problem.
            - **Root Cause:** A detailed explanation of what went wrong, referencing specific lines from the log.
            - **Recommendation:** A suggested next step for the developer to fix the issue.
        """
        ```
*   **🎉 Success!:** You've given your agent a new brain! This detailed prompt is crucial for guiding the Large Language Model to produce a structured, helpful response.

### - [ ] Session 7: Run the Triage Agent
*   **🎯 Objective:** Run the agent and give it our sample log to analyze.
*   **✅ Deliverable:** A successful triage report from your agent.
*   **🚀 Let's Go! (Steps):**
    1.  Make sure you have your `GOOGLE_API_KEY` environment variable set.
    2.  Install the dependencies for this project:
        ```bash
        uv sync
        ```
    3.  Run the agent:
        ```bash
        agent-cli run
        ```
    4.  When the REPL starts, give it the following prompt:
        ```
        Please analyze the build failure in the log file 'sample_log.txt'.
        ```
        (Press Enter twice to send)
*   **🎉 Success!:** The agent should first decide to use the `read_build_log` tool. You'll see a `[Tool Call]` message. Then, it will process the log content and generate a report similar to this:

    > **Summary:** The build failed due to an assertion error in the `test_login` test case.
    >
    > **Root Cause:** The test `tests/test_app.py::test_login` failed because it expected a `200` HTTP status code but received a `401` (Unauthorized) instead. This is shown on line 10 of the test file: `assert 401 == 200`.
    >
    > **Recommendation:** Check the login credentials being used in the `test_login` function. It appears the password `wrong` is incorrect, leading to the `401` error.

## Congratulations!

You've built an advanced agent that can reason, use tools, and solve a real-world problem. You've moved beyond simple chatbots and into the realm of autonomous assistants.

### Next Steps for Your Adventure

*   **Add a Knowledge Base:** Use the RAG features in `retrievers.py` to have the agent search an internal wiki of past incidents before making a recommendation.
*   **Create More Tools:** Build tools to `post_to_slack` or `create_github_issue` to automate the reporting process.
*   **Deploy It:** Use `agent-cli deploy` to put your agent on the cloud and trigger it with webhooks from your real CI/CD system.

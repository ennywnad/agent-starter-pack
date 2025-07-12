# ADHD-Friendly Guide to the Agent Starter Pack

Welcome! This guide is designed to teach you how to build, deploy, and manage AI agents using the Agent Starter Pack, with special care taken for how brains with ADHD learn best.

## How This Guide Works

This isn't like typical documentation. It's a series of short, focused missions (15-20 minutes each).

-   **Micro-Learning:** Small, manageable sessions.
-   **Clear Checkpoints:** You'll know *exactly* what you're supposed to accomplish.
-   **Immediate Results:** See your work come to life instantly.
-   **Visual Progress:** Use the checkboxes (`- [ ]`) to track your journey. Just edit the file and put an `x` in the brackets: `- [x]`.
-   **Dopamine Hits:** We celebrate every single win, big or small!

**Our Promise:** We will keep things clear, focused, and hands-on. We'll also be extremely careful with cloud costs, aiming to keep the total cost for this entire curriculum under **$10**.

---

## Module 0: The "Why" - What's in It for You?

**Goal:** Understand what an "agent" is and why this starter pack is the best way to build one.

### - [ ] Session 1: What is an AI Agent?
*   **🎯 Objective:** Get a clear, simple definition of an AI Agent.
*   **🤔 Why it matters:** You're about to build one, so you should know what it is!
*   **🚀 The Gist:**
    *   It's more than a chatbot. A chatbot can *talk*. An agent can *do*.
    *   It can use tools (like APIs, web browsers, or your own code).
    *   It can have a memory and learn from its interactions.
    *   It can be proactive and take initiative.
*   **🎉 Success!:** You can explain to a friend what an AI agent is. It's a computer program that can think, plan, and act to achieve a goal.

### - [ ] Session 2: Why Use this Starter Pack?
*   **🎯 Objective:** Understand the benefits of the Agent Starter Pack.
*   **🤔 Why it matters:** This will help you understand the value of what you're learning.
*   **🚀 The Gist:**
    *   **It's a shortcut.** You don't have to start from scratch. You get a fully-functional agent out of the box.
    *   **It's a launchpad.** It's easy to customize and add new features.
    *   **It's production-ready.** You can deploy your agent to the cloud with a single command.
    *   **It's a safety net.** We've included best practices for security, cost control, and observability.
*   **🎉 Success!:** You're excited to start building because you know you're using the right tool for the job.

---

## Module 1: Your First Agent (100% Local & Free)

**Goal:** Build and customize an agent on your own machine. No cloud costs, no complex setup.

### - [ ] Session 3: Liftoff! Your Development Environment
*   **🎯 Objective:** Get your computer ready to build agents.
*   **✅ Deliverable:** A working installation of the Agent Starter Pack CLI.
*   **🚀 Let's Go! (Steps):**
    1.  Make sure you have Python 3.10+ installed. You can check by opening your terminal and running:
        ```bash
        python3 --version
        ```
    2.  Install `uv`, a fast Python package installer.
        *   **macOS/Linux:**
            ```bash
            curl -LsSf https://astral.sh/uv/install.sh | sh
            ```
        *   **Windows:**
            ```bash
            powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
            ```
    3.  Install the Agent Starter Pack CLI. This one command sets up everything you need.
        ```bash
        pip install git+https://github.com/google/agent-starter-pack.git
        ```
*   **🎉 Success!:** You'll know it worked if you can run this command and see the help menu:
    ```bash
    agent-cli --help
    ```
    Seeing a list of commands means you're ready for the next mission. Great job!
*   **🤔 Troubleshooting & FAQ:**
    *   **"Command not found: agent-cli"**: Your system's `PATH` might be misconfigured. The installer usually gives you a command to run to fix this, which looks something like `source /Users/your-name/.cargo/env`. Close and reopen your terminal after running it.
    *   **Permission Errors:** If you see "Permission Denied," you may need to run the install command with `sudo` (macOS/Linux) or in an Administrator terminal (Windows), but try to avoid this if possible.
*   **💰 Cost Check:** **$0.00**. This is all on your local machine.

### - [ ] Session 4: Create Your First Agent
*   **🎯 Objective:** Create a new agent project from a template.
*   **✅ Deliverable:** A new folder on your computer containing the code for a basic agent.
*   **🚀 Let's Go! (Steps):**
    1.  Navigate to the directory where you want to store your projects.
    2.  Run the `create` command. We'll use the simplest template, `adk_base`, for a quick start.
        ```bash
        agent-cli create
        ```
    3.  The CLI will ask you a few questions. Here’s how to answer:
        *   `Enter a name for your agent...`: **`my-first-agent`**
        *   `Select a template...`: Use the arrow keys to select **`adk_base`** and press Enter.
        *   `Select a deployment target...`: Select **`cloud_run`** (we won't deploy yet, but this prepares our config).
        *   `Select a frontend...`: Select **`none`**.
*   **🎉 Success!:** A new directory named `my-first-agent` will be created. You can `cd my-first-agent` and see the project files. You just created a complete agent codebase in under a minute!
*   **🤔 Troubleshooting & FAQ:**
    *   **Git errors:** The `create` command uses `git`. If you don't have Git installed, you'll need to install it first.
*   **💰 Cost Check:** **$0.00**. Still 100% local.

### - [ ] Session 5: Talk to Your Agent (Locally)
*   **🎯 Objective:** Run your new agent and interact with it directly from your terminal.
*   **✅ Deliverable:** A running agent that responds to your prompts.
*   **🚀 Let's Go! (Steps):**
    1.  Navigate into your new agent's directory:
        ```bash
        cd my-first-agent
        ```
    2.  Install the agent's specific Python dependencies using `uv`:
        ```bash
        uv sync
        ```
    3.  Run the agent!
        ```bash
        agent-cli run
        ```
    4.  The agent will start up. Once you see the `[ADK] Entering REPL...` message, type a prompt and press Enter twice.
        ```
        Hello, world!
        ```
        (Press Enter again to send)
*   **🎉 Success!:** The agent will respond with a simple, streamed message. You're now having a real conversation with your own AI agent! How cool is that?!
*   **🤔 Troubleshooting & FAQ:**
    *   **`GOOGLE_API_KEY` error:** The default agent uses the Gemini API. You need to provide an API key.
        1.  Get a key from [Google AI Studio](https://aistudio.google.com/app/apikey).
        2.  Set it as an environment variable. On macOS/Linux: `export GOOGLE_API_KEY="YOUR_KEY_HERE"`. On Windows: `set GOOGLE_API_KEY="YOUR_KEY_HERE"`.
        3.  Then run `agent-cli run` again.
*   **💰 Cost Check:** **$0.00**. The Gemini API has a generous free tier. You won't be charged for these simple queries.

### - [ ] Session 6: Customize Your Agent's Brain
*   **🎯 Objective:** Change the agent's core instructions (its "prompt") to give it a new personality.
*   **✅ Deliverable:** Your agent will respond in a completely different style.
*   **🚀 Let's Go! (Steps):**
    1.  Make sure you're in the `my-first-agent` directory.
    2.  Open the main agent file in your favorite code editor: `app/agent.py`.
    3.  Find the `system_instruction` variable. It looks like this:
        ```python
        system_instruction = "You are a helpful assistant."
        ```
    4.  Change it to something more fun. Let's make it a pirate.
        ```python
        system_instruction = "You are a helpful assistant who talks like a pirate."
        ```
    5.  Save the file.
    6.  Run the agent again:
        ```bash
        agent-cli run
        ```
    7.  Talk to it!
        ```
        Who are you?
        ```
*   **🎉 Success!:** The agent should now respond like a pirate! "Ahoy, I be a helpful assistant, matey!" This is the core development loop: change code, run, test. You've mastered it.
*   **🤔 Troubleshooting & FAQ:**
    *   **Syntax errors:** If you accidentally break the Python code, the `run` command will show an error. Read the error message carefully—it usually points to the exact line you need to fix.
*   **💰 Cost Check:** **$0.00**.

<details>
<summary><strong>🧠 Deeper Dive: Visualizing Your Local Architecture (Optional, 5 min)</strong></summary>

Now that your agent is working, let's quickly visualize what you've actually built.

**Your Current Setup:**
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│    You      │────▶│  Your Agent │────▶│   Gemini    │
│ (Terminal)  │     │  (Python)   │     │    API      │
└─────────────┘     └─────────────┘     └─────────────┘
```

**Understanding the Flow:**
1. **You** type a prompt in your terminal.
2. **Your Agent** (the Python code in `app/agent.py`) processes it using your custom `system_instruction`.
3. **The Gemini API** receives the request and generates the response based on the instructions.
4. **Your Agent** streams the response back to your terminal.

**Checkpoint Questions:**
- Where is your agent's "brain" located? (In the `system_instruction` variable in your Python code).
- What happens if the Gemini API is down? (Your agent won't be able to get a response and will likely error).
- Why is this considered "local"? (Because all the code you wrote is running on your machine. The only external part is the API call to Google).

</details>

---

## Module 2: Your First Cloud Deployment

**Goal:** Deploy your agent to the cloud, making it accessible from anywhere. We'll be hyper-focused on cost control.

### - [ ] Session 7: GCP Safety First - Set a Budget!
*   **🎯 Objective:** Create a budget alert in your Google Cloud account to prevent surprise bills.
*   **✅ Deliverable:** An email alert that will be sent if your spending exceeds a set amount (e.g., $5).
*   **🚀 Let's Go! (Steps):**
    1.  Go to the [Google Cloud Console Billing section](https://console.cloud.google.com/billing).
    2.  In the left menu, click **Budgets & alerts**.
    3.  Click **+ CREATE BUDGET**.
    4.  **Scope:** Leave it as is (all projects, all services).
    5.  **Amount:** Set the "Target amount" to **$5**. This is your safety net.
    6.  **Actions:** Under "Percentage of budget," keep the default 50%, 90%, and 100% alerts. This means you'll get an email when you've spent $2.50, $4.50, and $5.00.
    7.  Click **Finish**.
*   **🎉 Success!:** You've created a budget. You can now experiment with peace of mind, knowing you'll be alerted long before costs become an issue. This is the most important step for stress-free learning.
*   **💰 Cost Check:** **$0.00**. Setting a budget is free.

### - [ ] Session 8: Deploy Your Agent to the Cloud
*   **🎯 Objective:** Deploy your agent to Google Cloud Run, a serverless platform.
*   **✅ Deliverable:** A publicly accessible URL for your agent.
*   **🚀 Let's Go! (Steps):**
    1.  First, you need to log in to Google Cloud from your terminal.
        ```bash
        gcloud auth login
        ```
    2.  Set your project ID. Replace `your-project-id` with your actual GCP Project ID.
        ```bash
        gcloud config set project your-project-id
        ```
    3.  Now, run the deploy command from your `my-first-agent` directory.
        ```bash
        agent-cli deploy
        ```
        This command automatically builds your agent into a container, pushes it to a registry, and deploys it to Cloud Run. It can take a few minutes the first time.
*   **🎉 Success!:** The command will finish by printing a **Service URL**. This is your live agent! You did it! You have an AI application running on the internet.
*   **🤔 Troubleshooting & FAQ:**
    *   **API not enabled:** The command might fail, telling you to enable an API (like `run.googleapis.com` or `artifactregistry.googleapis.com`). The error message includes a `gcloud` command to enable it. Just copy, paste, and run that command, then try `agent-cli deploy` again.
*   **💰 Cost Check:** **~$0.05**. This step creates a few resources: an Artifact Registry to store the code and the Cloud Run service itself. Cloud Run has a very generous free tier (2 million requests/month), so you'll likely pay **$0.00** for usage unless you send it a massive amount of traffic. The only real cost is for the storage in Artifact Registry, which is pennies per month.

<details>
<summary><strong>🧠 Deeper Dive: What Happened During Deployment? (Optional, 7 min)</strong></summary>

The `agent-cli deploy` command did several powerful things for you automatically. Understanding them is key to understanding the cloud.

**Your New Architecture:**
```
Internet → Google Cloud Load Balancer → Cloud Run (Your Agent) → Gemini API
```

**The Steps Under the Hood:**
1.  **Containerization:** Your agent's code was packaged into a standard Docker container.
2.  **Push to Registry:** The container was uploaded to Google Artifact Registry, a secure place to store your application images.
3.  **Deployment to Cloud Run:** Google Cloud Run took your container and created a managed, serverless service from it.
4.  **Configuration:** Cloud Run automatically provisioned a public URL and secured it with a free SSL certificate (HTTPS).

**Hands-On Exploration:**
1.  Visit the [Cloud Run Console](https://console.cloud.google.com/run).
2.  Click on your service name (e.g., `my-first-agent`).
3.  Look at the **Metrics** tab - you can see request counts, latency, and errors in real-time!
4.  Check the **Logs** tab - you can see the output from your agent as people use it.

**Key Insights:**
-   **Serverless:** You don't manage any servers. Cloud Run automatically starts your agent when a request comes in and shuts it down when it's idle (saving you money).
-   **Scalable:** If your agent suddenly gets 1,000 users, Cloud Run will automatically scale up to handle the load.
-   **Managed:** Google handles all the underlying infrastructure, networking, and security patching.

</details>

### - [ ] Session 9: Talk to Your Live Agent
*   **🎯 Objective:** Interact with your deployed agent using its public API.
*   **✅ Deliverable:** A successful API call to your agent's URL.
*   **🚀 Let's Go! (Steps):**
    1.  You'll need your agent's URL from the previous step and your Google API Key.
    2.  We'll use the `curl` command to send a request. Replace `YOUR_SERVICE_URL` and `YOUR_GOOGLE_API_KEY`.
        ```bash
        curl -X POST YOUR_SERVICE_URL/run \
        -H "Content-Type: application/json" \
        -H "X-Goog-Api-Key: YOUR_GOOGLE_API_KEY" \
        -d '{"prompt": "Who are you?"}'
        ```
*   **🎉 Success!:** You'll get a streaming response back directly in your terminal, with your pirate agent answering from the cloud! You've just confirmed your deployment is live and working.
*   **🤔 Troubleshooting & FAQ:**
    *   **Authentication Error:** If you get a 401 or 403 error, your `X-Goog-Api-Key` header is likely missing or incorrect. Double-check that you've pasted the key correctly.
*   **💰 Cost Check:** **$0.00** (covered by the Cloud Run free tier).

<details>
<summary><strong>🧠 Deeper Dive: Understanding the API Call (Optional, 5 min)</strong></summary>

The `curl` command you just used is a standard way to interact with APIs. Let's break it down. Your agent is following **REST API** conventions.

```bash
# The Method: POST means you are sending data to create a new resource (in this case, a new conversation turn).
curl -X POST YOUR_SERVICE_URL/run \

# The Headers: These provide metadata about your request.
# Content-Type tells the server you're sending data in JSON format.
-H "Content-Type: application/json" \
# X-Goog-Api-Key is a custom header for authentication.
-H "X-Goog-Api-Key: YOUR_GOOGLE_API_KEY" \

# The Body: This is the actual data you're sending.
# It's a JSON object with a "prompt" key.
-d '{"prompt": "Who are you?"}'
```

**Why these choices matter:**
-   **POST:** You use `POST` because you're *creating* a new request for the agent to process.
-   **JSON:** This is the universal language of modern web APIs. It's easy for any programming language to create and parse.
-   **API Key:** This is a simple but effective way to control access to your agent for prototyping. In a production application, you would use a more robust system like OAuth.

</details>

### - [ ] Session 10: The Big Cleanup
*   **🎯 Objective:** Delete all the cloud resources you created to stop all billing.
*   **✅ Deliverable:** A clean GCP project with no running services.
*   **🚀 Let's Go! (Steps):**
    1.  From your `my-first-agent` directory, run the `teardown` command.
        ```bash
        agent-cli teardown
        ```
    2.  The CLI will ask for confirmation to delete the Cloud Run service and the Artifact Registry repository. Type `y` and press Enter for both.
*   **🎉 Success!:** The command will confirm that the resources have been deleted. Your project is now clean, and no further costs will be incurred. Learning to clean up is as important as learning to build. You've completed the full lifecycle!
*   **🤔 Troubleshooting & FAQ:**
    *   **Lingering resources:** The `teardown` command is good, but it's always wise to double-check in the [Cloud Run](https://console.cloud.google.com/run) and [Artifact Registry](https://console.cloud.google.com/artifacts) pages in the GCP console to ensure everything is gone.
*   **💰 Cost Check:** **$0.00**. This step stops all future costs. Your total spend for this module should be well under $1.

<details>
<summary><strong>🧠 Deeper Dive: What's Missing for Production? (Optional, 8 min)</strong></summary>

Your agent works, but to make it "production-ready" for real users, you'd need to add more layers of robustness.

**The Production Readiness Checklist:**

*   **Security:**
    *   [ ] **Robust Authentication:** Move beyond simple API keys to user accounts (e.g., OAuth).
    *   [ ] **Rate Limiting:** Prevent abuse and control costs by limiting how often a user can call the agent.
    *   [ ] **Input Validation:** Sanitize user prompts to prevent malicious inputs.
    *   [x] **HTTPS Everywhere:** Already handled by Cloud Run!
*   **Reliability:**
    *   [ ] **Graceful Error Handling:** What happens if the Gemini API is down? Your agent should return a helpful message, not just crash.
    *   [ ] **Monitoring & Alerting:** Set up alerts to notify you if your agent goes down or experiences a spike in errors.
    *   [ ] **Load Testing:** Intentionally send a huge amount of traffic to your agent to see how it performs under pressure.
*   **Cost Control:**
    *   [ ] **Request Limits:** Implement daily or monthly caps per user to prevent runaway spending.
    *   [ ] **Usage Analytics:** Track how users are interacting with your agent to understand cost drivers.

**Insight:** In a real-world application, the core logic might be 20% of the code, while the other 80% is dedicated to error handling, security, monitoring, and other production concerns!

</details>

---

## Module 3: Understanding the Code

**Goal:** Look under the hood of your agent and understand how the different pieces of code work together.

### - [ ] Session 11: The Anatomy of an Agent
*   **🎯 Objective:** Get a high-level overview of the files in your agent's project directory.
*   **✅ Deliverable:** A clear understanding of what each file does.
*   **🚀 Let's Go! (A Guided Tour):**
    ```mermaid
    graph TD
        A[pyproject.toml] --> B(Dockerfile);
        B --> C{Container Image};
        C --> D[Cloud Run];
        E[app/main.py] --> F[app/agent.py];
        F --> G[LLM];
        D -- invokes --> E;
    ```
    *   `app/agent.py`: This is the heart of your agent. It contains the main logic, the system prompt, and the tools your agent can use.
    *   `app/main.py`: This is the entry point for your agent. It's responsible for starting the agent and handling incoming requests.
    *   `pyproject.toml`: This file defines the project's dependencies. It's how you add new packages to your agent.
    *   `README.md`: This file contains a description of your agent and instructions on how to use it.
    *   `Dockerfile`: This file contains the instructions for building your agent into a container. You won't need to touch this unless you're doing advanced customization.
*   **🎉 Success!:** You know where to find the most important code in your agent's project.

### - [ ] Session 12: The Agent's Brain (`agent.py`)
*   **🎯 Objective:** Understand the key components of the `agent.py` file.
*   **✅ Deliverable:** You can confidently edit the agent's prompt and add new tools.
*   **🚀 Let's Go! (Key Concepts):**
    *   **`system_instruction`**: This is the agent's core identity. It tells the agent how to behave and what its purpose is.
    *   **`tools`**: This is a list of functions that your agent can call. This is how you give your agent new capabilities.
    *   **`Agent` class**: This class brings everything together. It initializes the agent with the system instruction and tools, and it handles the conversation with the user.
*   **🎉 Success!:** You can now customize your agent's behavior by editing the `system_instruction` and adding new tools.

---

## Next Steps & Future Missions

You've done it! You've built, customized, deployed, and torn down an AI agent. You've mastered the fundamental **Build -> Test -> Deploy -> Cleanup** lifecycle. This is the key to exploring the world of AI agents confidently and affordably.

### More Hands-On Missions

*   **Deploy a UI:** Try the `adk_gemini_fullstack` template to create an agent with a web interface.
*   **Give Your Agent Tools:** Explore how to give your agent access to other APIs (e.g., a weather API, a search engine).
*   **Deeper Customization:** Experiment with different models, prompts, and logic in the `agent.py` file.

### Advanced Learning Path

Ready to understand the "why" behind the "what"? Continue your journey with these concepts.

*   **🏗️ [Architecture Guide](architecture_guide.md):** See the bigger picture with visual diagrams showing how agents work at different scales, from a single agent to a complex, multi-agent enterprise system.
*   **🤖 Multi-Agent Systems:** Imagine agents working together. For example, a "Research Agent" could find information, and a "Writer Agent" could use that information to create a document. This is the basis of frameworks like CrewAI.
*   **🛡️ Enterprise Security:** In a large company, you wouldn't expose your agent directly to the internet. It would sit behind an **API Gateway** (for authentication and rate limiting) inside a **VPC Network** (a private cloud firewall) for maximum security).

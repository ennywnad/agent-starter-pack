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

## Module 1: Your First Agent (100% Local & Free)

**Goal:** Build and customize an agent on your own machine. No cloud costs, no complex setup.

### - [ ] Session 1: Liftoff! Your Development Environment
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
*   **🤔 Common Pitfalls & Fixes:**
    *   **"Command not found: agent-cli"**: Your system's `PATH` might be misconfigured. The installer usually gives you a command to run to fix this, which looks something like `source /Users/your-name/.cargo/env`. Close and reopen your terminal after running it.
    *   **Permission Errors:** If you see "Permission Denied," you may need to run the install command with `sudo` (macOS/Linux) or in an Administrator terminal (Windows), but try to avoid this if possible.
*   **💰 Cost Check:** **$0.00**. This is all on your local machine.

### - [ ] Session 2: Create Your First Agent
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
*   **🤔 Common Pitfalls & Fixes:**
    *   **Git errors:** The `create` command uses `git`. If you don't have Git installed, you'll need to install it first.
*   **💰 Cost Check:** **$0.00**. Still 100% local.

### - [ ] Session 3: Talk to Your Agent (Locally)
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
*   **🤔 Common Pitfalls & Fixes:**
    *   **`GOOGLE_API_KEY` error:** The default agent uses the Gemini API. You need to provide an API key.
        1.  Get a key from [Google AI Studio](https://aistudio.google.com/app/apikey).
        2.  Set it as an environment variable. On macOS/Linux: `export GOOGLE_API_KEY="YOUR_KEY_HERE"`. On Windows: `set GOOGLE_API_KEY="YOUR_KEY_HERE"`.
        3.  Then run `agent-cli run` again.
*   **💰 Cost Check:** **$0.00**. The Gemini API has a generous free tier. You won't be charged for these simple queries.

### - [ ] Session 4: Customize Your Agent's Brain
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
*   **🤔 Common Pitfalls & Fixes:**
    *   **Syntax errors:** If you accidentally break the Python code, the `run` command will show an error. Read the error message carefully—it usually points to the exact line you need to fix.
*   **💰 Cost Check:** **$0.00**.

---

## Module 2: Your First Cloud Deployment

**Goal:** Deploy your agent to the cloud, making it accessible from anywhere. We'll be hyper-focused on cost control.

### - [ ] Session 5: GCP Safety First - Set a Budget!
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

### - [ ] Session 6: Deploy Your Agent to the Cloud
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
*   **🤔 Common Pitfalls & Fixes:**
    *   **API not enabled:** The command might fail, telling you to enable an API (like `run.googleapis.com` or `artifactregistry.googleapis.com`). The error message includes a `gcloud` command to enable it. Just copy, paste, and run that command, then try `agent-cli deploy` again.
*   **💰 Cost Check:** **~$0.05**. This step creates a few resources: an Artifact Registry to store the code and the Cloud Run service itself. Cloud Run has a very generous free tier (2 million requests/month), so you'll likely pay **$0.00** for usage unless you send it a massive amount of traffic. The only real cost is for the storage in Artifact Registry, which is pennies per month.

### - [ ] Session 7: Talk to Your Live Agent
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
*   **🤔 Common Pitfalls & Fixes:**
    *   **Authentication Error:** If you get a 401 or 403 error, your `X-Goog-Api-Key` header is likely missing or incorrect. Double-check that you've pasted the key correctly.
*   **💰 Cost Check:** **$0.00** (covered by the Cloud Run free tier).

### - [ ] Session 8: The Big Cleanup
*   **🎯 Objective:** Delete all the cloud resources you created to stop all billing.
*   **✅ Deliverable:** A clean GCP project with no running services.
*   **🚀 Let's Go! (Steps):**
    1.  From your `my-first-agent` directory, run the `teardown` command.
        ```bash
        agent-cli teardown
        ```
    2.  The CLI will ask for confirmation to delete the Cloud Run service and the Artifact Registry repository. Type `y` and press Enter for both.
*   **🎉 Success!:** The command will confirm that the resources have been deleted. Your project is now clean, and no further costs will be incurred. Learning to clean up is as important as learning to build. You've completed the full lifecycle!
*   **🤔 Common Pitfalls & Fixes:**
    *   **Lingering resources:** The `teardown` command is good, but it's always wise to double-check in the [Cloud Run](https://console.cloud.google.com/run) and [Artifact Registry](https://console.cloud.google.com/artifacts) pages in the GCP console to ensure everything is gone.
*   **💰 Cost Check:** **$0.00**. This step stops all future costs. Your total spend for this module should be well under $1.

---

## Next Steps & Future Missions

You've done it! You've built, customized, deployed, and torn down an AI agent. You've mastered the fundamental workflow.

Here are some ideas for your next missions:

*   **Deploy a UI:** Try the `adk_gemini_fullstack` template to create an agent with a web interface.
*   **Give Your Agent Tools:** Explore how to give your agent access to other APIs (e.g., a weather API, a search engine).
*   **Deeper Customization:** Experiment with different models, prompts, and logic in the `agent.py` file.

This structured approach of **Build -> Test -> Deploy -> Cleanup** is your key to exploring the world of AI agents confidently and affordably.

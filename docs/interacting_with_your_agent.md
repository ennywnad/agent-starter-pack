# How to Talk to Your AI Agent When It Gets Stuck

You're working with your AI coding agent, things are going great, and then... silence. Or worse, an error. What do you do when the "black box" of the AI's process seems to have failed?

Knowing how to effectively nudge, question, and guide your AI partner is the key to a smooth and productive workflow. Here’s a simple guide on how to get your agent back on track, from providing specific feedback to asking for a status update when you're not sure what went wrong.

---

## The "Code Review" Approach: Pointing Out a Specific Error

When you can see the mistake—a duplicated section, a syntax error, a file in the wrong place—the best approach is to be direct and specific, just like you would in a code review.

**The Formula:**
> "It looks like your last action [resulted in an error/was incomplete]. Please review the changes in `[filename]` and [correct the issue]."

**Why it works:** This method is highly effective because it removes all guesswork. You provide the agent with the location of the error and a clear goal for the correction.

**Example:**
> "Your last update to `@README.md` duplicated the 'ADHD-Friendly Learning Guide' section. Please review the file and remove the duplicate."

---

## The "Black Box" Approach: When You Don't Know What's Wrong

Sometimes, you don't know the specifics of the failure. The agent was in the middle of a multi-step process, and it just stopped. In these cases, your goal is to make the agent's internal plan transparent.

Here are three levels of inquiry you can use.

### 1. To Understand the Goal: "What is your current plan?"

This is your most powerful tool for debugging the agent's process. It prompts the agent to pause its current action and state its step-by-step plan.

**When to use it:**
- When the agent seems stuck or is taking too long.
- When you're not sure what the agent is trying to accomplish.
- Before the agent starts a complex task, to ensure you're aligned.

**Example:**
> **You:** "What is your current plan?"
>
> **Agent:** "My plan is to:
> 1. Read the `README.md` file to understand its structure.
> 2. Add a new section about the learning guide.
> 3. Add a link to the `docs/learning_plan.md` file."

This reveals the agent's "thought process" and allows you to see if it's on the right track.

### 2. To Check on Progress: "What was the last step you completed?"

If you think the agent has partially finished a task, this question helps you understand where it is in its plan.

**When to use it:**
- After a long pause in the agent's activity.
- If you see some changes but aren't sure if the task is finished.

**Example:**
> **You:** "What was the last step you completed?"
>
> **Agent:** "I successfully wrote the new content to `/docs/learning_plan.md`."

This tells you what the agent *thinks* it has accomplished, helping you guide it to the next logical step.

### 3. The General Nudge: "Please review your last action and continue."

This is your go-to command for a "soft reset." It prompts the agent to re-evaluate its last action, check for errors, and get back to work without requiring a full explanation from you.

**When to use it:**
- When you suspect an error but don't have time to investigate.
- When the agent stops responding after you've approved a step.

---

By using these simple, clear communication strategies, you can turn moments of confusion into productive collaboration, keeping your development workflow smooth and efficient.

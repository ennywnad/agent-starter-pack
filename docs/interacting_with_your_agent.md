# How to Talk to Your AI Agent When It Gets Stuck

> **Key Takeaways:**
> - **Be specific when you see the error.** Use the "Code Review" approach.
> - **If you don't know the error, ask for the plan.** Use the "Black Box" approach.
> - **Start with a general nudge.** "Please review your last action and continue."
> - **If that doesn't work, ask for the plan.** "What is your current plan?"
> - **If the plan is flawed, guide the agent to a new one.** "Your plan seems to be missing a step. Please add a step to..."

You're working with your AI coding agent, things are going great, and then... silence. Or worse, an error. What do you do when the "black box" of the AI's process seems to have failed?

Knowing how to effectively nudge, question, and guide your AI partner is the key to a smooth and productive workflow. Here’s a simple guide on how to get your agent back on track, from providing specific feedback to asking for a status update when you're not sure what went wrong.

---

## When to Use Each Approach

| Situation | Best Approach | Example |
| --- | --- | --- |
| You see a clear error in the code. | Code Review | "You have a syntax error in `main.py` on line 42." |
| The agent is taking too long. | Black Box | "What is your current plan?" |
| The agent stops unexpectedly. | Black Box | "Please review your last action and continue." |
| You're not sure what the agent is doing. | Black Box | "What is your current plan?" |
| The agent's plan seems wrong. | Black Box | "Your plan seems to be missing a step. Please add a step to..." |

---

## The "Code Review" Approach: Pointing Out a Specific Error

When you can see the mistake—a duplicated section, a syntax error, a file in the wrong place—the best approach is to be direct and specific, just like you would in a code review.

**The Formula:**
> "It looks like your last action [resulted in an error/was incomplete]. Please review the changes in `[filename]` and [correct the issue]."

**Why it works:** This method is highly effective because it removes all guesswork. You provide the agent with the location of the error and a clear goal for the correction.

**Examples:**

> **Scenario: Incorrect Logic**
>
> **You:** "The `calculate_total` function in `main.py` is not applying the discount correctly. Please review the logic and ensure the discount is subtracted before the tax is added."

> **Scenario: Duplicated Code**
>
> **You:** "Your last update to `@README.md` duplicated the 'ADHD-Friendly Learning Guide' section. Please review the file and remove the duplicate."

> **Scenario: File in the Wrong Location**
>
> **You:** "You created the `new_feature.py` file in the root directory, but it should be in the `src/features` directory. Please move the file to the correct location."

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

### 2. To Correct a Flawed Plan: "Your plan seems to be missing a step."

Sometimes, the agent's plan is missing a crucial step, or it's heading in the wrong direction. In these cases, you can guide the agent to a better plan.

**When to use it:**
- When the agent's plan will not achieve the desired outcome.
- When you want to add a step to the agent's plan.

**Example:**
> **You:** "Your plan is missing a step. Before you write the new content, you need to create the file `docs/learning_plan.md`."
>
> **Agent:** "You are correct. My apologies. My new plan is to:
> 1. Read the `README.md` file to understand its structure.
> 2. Create the file `docs/learning_plan.md`.
> 3. Add a new section about the learning guide.
> 4. Add a link to the `docs/learning_plan.md` file."

### 4. To Check on Progress: "What was the last step you completed?"

If you think the agent has partially finished a task, this question helps you understand where it is in its plan.

**When to use it:**
- After a long pause in the agent's activity.
- If you see some changes but aren't sure if the task is finished.

**Example:**
> **You:** "What was the last step you completed?"
>
> **Agent:** "I successfully wrote the new content to `/docs/learning_plan.md`."

This tells you what the agent *thinks* it has accomplished, helping you guide it to the next logical step.

### 5. The General Nudge: "Please review your last action and continue."

This is your go-to command for a "soft reset." It prompts the agent to re-evaluate its last action, check for errors, and get back to work without requiring a full explanation from you.

**When to use it:**
- When you suspect an error but don't have time to investigate.
- When the agent stops responding after you've approved a step.

---

## Troubleshooting: When the Agent is Still Stuck

Sometimes, you've tried all of the above, and the agent is still not making progress. Here are a few things to try:

- **Start over with a clean slate.** Sometimes, the best way to fix a problem is to start over. You can ask the agent to create a new plan from scratch.
- **Break the problem down into smaller pieces.** If the agent is struggling with a complex task, try breaking it down into smaller, more manageable pieces.
- **Try a different approach.** If the agent is stuck on one approach, try suggesting a different one.

---

By using these simple, clear communication strategies, you can turn moments of confusion into productive collaboration, keeping your development workflow smooth and efficient.

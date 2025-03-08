---
title: "A systematic approach to debugging"
date: 2025-03-08 11:37:01 +00:00
tags: [tech]
description: A step-by-step approach to debugging while striking a balance between speed and quality.
toc: true
---

I have a reputation for fixing issues "quickly" while maintaining high quality. 

But how do I fix bugs so quickly?

After successfully fixing many issues at work and in real life, and preventing the same mistakes from happening again, I developed a step-by-step approach to strike a good balance between speed and quality.

# High-level debugging steps

1. Be in a good mental state.
2. Understand the expected behaviour.
3. Understand the current behaviour.
4. Reproduce the current behaviour.
5. Narrow down the error’s scope to the minimal reproducible case by forming hypotheses.
6. Change one thing at a time to verify the hypothesis.
7. If still stuck, take a break.
8. Ask for help.

# Detailed breakdown of each step

## 1. Be in a good mental state.

Being in a bad state increases the sensitivity of feelings and emotions, which decreases objectivity in finding a better solution. 

E.g., I needed to drill 2 holes in the bathroom wall to hang a drawer. I felt stressed to the point that it negatively affected my hand coordination. I tried to ignore the over-stress and started drilling. The result was misaligned holes. Then, I spent even more time trying to fix it.

So, before making a decision, I check my mental state, and I proceed accordingly.

If I am in a bad mental state, I do some of the following to feel better:

- Take a short break.
- Do physical exercise.
- Meditate.
- Eat a small piece of chocolate.
- Gather more information about the problem.
	- This action is strongly tied to understanding the current behaviour. However, the whole "fixing issue" process may not always follow a linear path (such as life), so I sometimes start collecting evidence at this stage.

If I am already in a (somewhat) good state, then I move to the next step.

## 2. Understand the expected behaviour.

Implementing a solution without understanding the expected behaviour usually results in an unwanted solution, and a waste of resources. So, I go through a few bullet points to clarify the expected behaviour and increase my confidence.

- Identify the stakeholder(s) (usually a Product Manager), and empathise.
- List assumptions.
- Verify the base case.
- Explore edge cases, and verify them.
- Explore dependencies, and verify them.
- If still unsure about *anything*, verify with the stakeholder.

## 3. Understand the current behaviour.

The current behaviour is the main symptom. It starts lighting the path to the core issue. Some practices I follow to understand the current behaviour:

- Expand my knowledge on the overall system, and a particular part.
- Draw a diagram of the system, the expected behaviour, and the current behaviour, and compare them.
- Reproduce the issue (if I am lucky enough).

## 4. Reproduce the current behaviour.

If an issue can be reproduced, it’s almost as good as solved. But, it is not always straightforward to reproduce an issue. So, the following actions help me troubleshoot while reproducing an issue.

- Check the plug.
- Compare the current environment's settings with those of the issue, and align them in the current environment one by one.
- Set up a new testing environment.

## 5. Narrow down the error’s scope to the minimal reproducible case by forming hypotheses.

The principle is to make small changes, iteratively. I listed some of the steps I use to fix coding issues:

- If the system has observability set up (e.g., logs, screen recordings), search for the issue in it.
- Use a debugger.
- If potentially problematic code was changed recently, use a "commit history lookup" tool to see which lines were changed recently by who and why.
- If potentially problematic commit needs to be found, try binary searching the already-sorted commit history (e.g., manually or by running `git bisect`).
- There are too many methods and tools to list which deserves it own blog post, so explore a tool if it will be useful.

## 6. Change one thing at a time to verify the hypothesis.

After forming a hypothesis and changing/fixing it, I document the change and its result to avoid wasting time repeating the hypothesis. For this step, I like to use a notepad or pen and paper.

## 7. If still stuck, take a break.

Despite following a step-by-step approach, progress might not be linear; and that’s life. So, I follow the proven method of taking a break.

- Go for a 20-minute walk in nature without distractions like electronic devices.
- Meditate.
- Do a physical exercise.
- Take a shower.
- Sleep.
- Work on something different and small.

## 8. Ask for help.

Sometimes, a single perspective can't see the solution. So, I ask others to get their perspective on the situation. However, effectively communicating the situation is a skill in itself, and I have a [practical guide](https://baransblog.com/how-to-communicate-effectively-a-simple-and-practical-guide/) on how to do it efficiently.

- AI.
- Real people.

# Next steps

If I managed to solve the issue, it's time to celebrate; because I deserved it! After celebrating, I take a few actions to prevent the same issue from happening, and improve the process for next time.

1. Write automated tests to document the behaviour and automatically prevent the same mistake from happening in the future.
2. Add instrumentation (e.g., logger) to reproduce the same issue quickly.
3. Evaluate what went well.
4. Evaluate what went wrong, and improve it for next time.
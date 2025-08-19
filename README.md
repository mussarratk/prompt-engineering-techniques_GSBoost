# 🚀 Prompt Engineering Cheatsheet: 17 Techniques for 1B+ LLMs

This cheatsheet summarizes 17 powerful prompt engineering techniques, from beginner to advanced. Use these methods to get more accurate, creative, and reliable responses from Large Language Models (LLMs), including smaller 1B parameter models.

---

## Table of Contents

1.  [Controlling the Output: Key Parameters](#controlling-the-output-key-parameters)
2.  [Zero-Shot Prompting](#1-zero-shot-prompting)
3.  [Few-Shot Prompting](#2-few-shot-prompting)
4.  [Role Prompting](#3-role-prompting)
5.  [Style Prompting](#4-style-prompting)
6.  [Emotion Prompting](#5-emotion-prompting)
7.  [Contextual Prompting](#6-contextual-prompting)
8.  [Chain-of-Thought (CoT) Prompting](#7-chain-of-thought-cot-prompting)
9.  [System Prompting](#8-system-prompting)
10. [Explicit Instructions Prompting](#9-explicit-instructions-prompting)
11. [Output Priming](#10-output-priming)
12. [Rephrase and Respond (RaR)](#11-rephrase-and-respond-rar)
13. [Step-Back Prompting](#12-step-back-prompting)
14. [Self-Critique & Refinement](#13-self-critique--refinement)
15. [Goal Decomposition Prompting](#14-goal-decomposition-prompting)
16. [Meta-Prompting](#15-meta-prompting)
17. [ReAct (Reason + Act)](#16-react-reason--act)
18. [Thread-of-Thought (ThoT)](#17-thread-of-thought-thot)

---

## Controlling the Output: Key Parameters

Before diving into techniques, understand these core settings. They control the LLM's response behavior.

### Temperature
* **What it is:** Temperature controls the randomness of the output. **Lower values** (e.g., `0.0` - `0.2`) make the response more focused, deterministic, and predictable. This is great for factual queries, extraction, and summarization. **Higher values** (e.g., `0.7` - `1.0`) make the output more creative and diverse, which is useful for brainstorming or story writing.
* **How to use it:** For most of the factual techniques below, **start with a `Temperature` of `0.1`**. If you need creative variations, increase it.

### Output Token Limit
* **What it is:** This sets the **maximum length** of the model's response in tokens (pieces of words).
* **How to use it:** If you expect a short answer, set a low limit (e.g., `50`) to save resources. If you need a detailed explanation or a long piece of text, increase it to `1024` or higher to prevent the response from being cut off.

### Top-P
* **What it is:** Top-P (or nucleus sampling) is an alternative way to control randomness. It considers only the most probable tokens that add up to a certain probability mass (`P`). A **lower `Top-P`** (e.g., `0.8`) results in less random, more focused text. A `Top-P` of `1.0` considers all tokens.
* **How to use it:** Generally, it's best to **modify either `Temperature` or `Top-P`, but not both** at the same time. Start with `Temperature`, as it's more intuitive for most use cases.

---

## 1. Zero-Shot Prompting

> **Core Idea:** Ask the model to perform a task directly, without any prior examples. This is your first attempt at any prompt.

* **When to Use:**
    * Simple questions, translations, or summaries.
    * When the task is straightforward and relies on the model's general knowledge.
* **How to Implement:** Directly state your request. For factual tasks, set **`Temperature` to `0.1`**.
* **Example (Data Extraction):**
    * **Before:**
        ```
        Here is some text: "The new OrbitX Pro laptop features the M3 Ultra chip and costs $1999." What is the price?
        ```
    * **After (More focused):**
        ```
        Extract the product name and price from the following text: "The new OrbitX Pro laptop features the M3 Ultra chip and costs $1999."
        ```
    * *Improvement:* The "After" prompt is a direct command, reducing conversational filler and yielding a more direct answer instead of a full sentence.

---

## 2. Few-Shot Prompting

> **Core Idea:** Provide a few examples (input/output pairs) to show the model the desired task and format before giving the final query.

* **When to Use:**
    * When you need a specific output format (like JSON).
    * For nuanced classification or extraction tasks.
    * To guide the model on a non-standard task.
* **How to Implement:** In platforms like Vertex AI, use the **`Add Examples`** feature. Otherwise, structure them directly in the prompt before your final input.
* **Example (JSON Formatting):**
    * **Before (Zero-Shot):**
        ```
        Extract the name and role from this sentence: "Maria Garcia is the lead engineer." Format it as JSON.
        ```
    * **After (Few-Shot):**
        ```
        -- EXAMPLE 1 --
        Input: "John Smith is the marketing director."
        Output: { "name": "John Smith", "role": "Marketing Director" }

        -- EXAMPLE 2 --
        Input: "The CEO is Sarah Chen."
        Output: { "name": "Sarah Chen", "role": "CEO" }

        -- FINAL TASK --
        Input: "Maria Garcia is the lead engineer."
        Output:
        ```
    * *Improvement:* The model learns the exact JSON structure from the examples, ensuring reliable and correctly formatted output.

---

## 3. Role Prompting

> **Core Idea:** Instruct the LLM to adopt a specific persona or character to tailor its tone, style, and expertise.

* **When to Use:**
    * Generating content for a specific audience (e.g., experts, beginners, children).
    * Simulating dialogues or expert perspectives.
* **How to Implement:** Start the prompt with `Act as a [Role]...` or `You are a [Role]...`.
* **Example (Cybersecurity Explanation):**
    * **Before:**
        ```
        Explain the risks of phishing attacks.
        ```
    * **After:**
        ```
        Act as a senior cybersecurity analyst briefing a non-technical executive team. Explain the business risks of phishing attacks in simple, impactful terms.
        ```
    * *Improvement:* The response shifts from a generic definition to a targeted, C-suite-level summary focusing on business impact, which is far more useful in this context.

---

## 4. Style Prompting

> **Core Idea:** Guide the LLM to write in a particular literary, artistic, or brand-specific style.

* **When to Use:**
    * Creative writing, marketing copy, or content adaptation.
    * Matching a specific brand voice (e.g., witty, formal, academic).
* **How to Implement:** Specify the style: `Write ... in the style of [Specific Person/Brand/Style]`.
* **Example (Marketing Copy):**
    * **Before:**
        ```
        Write a product description for a new smart coffee mug.
        ```
    * **After:**
        ```
        Write a product description for a new smart coffee mug. Write it in the witty, slightly informal, and benefit-focused brand voice of Apple.
        ```
    * *Improvement:* Instead of a generic description, the model adopts a well-defined, successful style, making the output more engaging and brand-aligned.

---

## 5. Emotion Prompting

> **Core Idea:** Instruct the LLM to generate a response that conveys a specific emotion.

* **When to Use:**
    * Crafting messages with a specific emotional impact (apologies, congratulations).
    * Adding depth to creative writing.
* **How to Implement:** Explicitly state the desired emotion: `Write this... The tone should be [emotion].`
* **Example (Customer Apology):**
    * **Before:**
        ```
        Write an email to a customer about their delayed order.
        ```
    * **After:**
        ```
        Write an email to a customer about their delayed order. The tone should be genuinely apologetic and empathetic, reassuring them that we are resolving the issue.
        ```
    * *Improvement:* The output changes from a cold, robotic notification to a warm, customer-centric message that can help retain goodwill.

---

## 6. Contextual Prompting

> **Core Idea:** Provide the LLM with all necessary background information, data, and constraints for the request.

* **When to Use:**
    * Personalized recommendations, problem-solving, and content generation about specific subjects.
* **How to Implement:** Include all relevant details directly in the prompt.
* **Example (Email Writing):**
    * **Before:**
        ```
        Write an email to my team about a new project.
        ```
    * **After:**
        ```
        Write an email to my engineering team.
        Project Name: Project Phoenix
        Goal: Redesign the user dashboard.
        Key Deadline: First draft by September 26th.
        Context: This is a high-priority initiative to improve user engagement.
        Call to Action: Ask them to review the attached brief and come to the kickoff meeting on Friday with initial ideas.
        ```
    * *Improvement:* The "After" prompt provides all the necessary specifics, resulting in a complete, actionable email draft instead of a uselessly generic template.

---

## 7. Chain-of-Thought (CoT) Prompting

> **Core Idea:** Encourage the LLM to "think step by step" to break down complex problems, improving its reasoning process.

* **When to Use:**
    * Arithmetic, commonsense, and symbolic reasoning tasks.
    * When you want to see the model's logic to verify its conclusion.
* **How to Implement:** Add `Let's think step by step.` to the end of your prompt.
* **Example (Logic Puzzle):**
    * **Before:**
        ```
        A farmer has 15 cows. All but 8 died. How many are left?
        ```
    * **After:**
        ```
        A farmer has 15 cows. All but 8 died. How many are left? Let's think step by step.
        ```
    * *Improvement:* The "Before" prompt might elicit an incorrect answer (7) because the model processes the numbers superficially. By forcing a step-by-step process, the model correctly interprets the phrase "All but 8" and arrives at the right answer (8), showing its reasoning.

---

## 8. System Prompting

> **Core Idea:** Provide high-level instructions or a persona in the dedicated "system" message to guide the model's behavior across an entire conversation.

* **When to Use:**
    * To ensure a consistent persona or output format for multiple, consecutive queries.
    * To set overarching rules or constraints without repeating them in every prompt.
* **How to Implement:** Use your platform's `system` prompt field.
* **Example (Code Generation):**
    * **Before (in every user prompt):**
        ```
        Write a Python function to do X. Add comments explaining the code.
        ```
    * **After (in System Prompt):**
        ```
        System: You are an expert Python developer. All code you provide must follow PEP 8 standards and include detailed docstrings explaining the function's purpose, arguments, and return value.
        User: Write a function that calculates the factorial of a number.
        ```
    * *Improvement:* The system prompt establishes a baseline quality for all subsequent requests, making each user prompt shorter and the overall interaction more efficient and consistent.

---

## 9. Explicit Instructions Prompting

> **Core Idea:** Be crystal clear and direct about your requirements, leaving no room for ambiguity. Specify the format, content, length, and what to exclude.

* **When to Use:**
    * When the output must adhere to strict requirements.
    * To avoid unwanted information or topics.
* **How to Implement:** Detail exactly what you want and what you don't want.
* **Example (Summarization):**
    * **Before:**
        ```
        Summarize this article. [article text]
        ```
    * **After:**
        ```
        Summarize the attached article.
        Format: A bulleted list with exactly 3 points.
        Content: Focus only on the economic impacts discussed.
        Constraint: Do not mention any political figures or historical background.
        ```
    * *Improvement:* The output is precisely tailored to the user's needs—a scannable, focused list—rather than a generic paragraph that might include irrelevant information.

---

## 10. Output Priming

> **Core Idea:** Provide the beginning of the desired response to nudge the model towards a specific structure or format.

* **When to Use:**
    * To force a specific output format like JSON, Markdown, or a list.
    * To ensure a specific starting phrase or tone.
* **How to Implement:** End your prompt with the first few characters of the expected output.
* **Example (JSON Extraction):**
    * **Before:**
        ```
        Extract the name and company from: 'Anna Schmidt is the CTO of QuantumLeap Inc.' and provide it as JSON.
        ```
    * **After:**
        ```
        Extract the name and company from: 'Anna Schmidt is the CTO of QuantumLeap Inc.' and provide it as JSON.
        {
          "name": "
        ```
    * *Improvement:* Priming significantly increases the probability that the model will complete the JSON object correctly, as it's already "on the right track."

---

## 11. Rephrase and Respond (RaR)

> **Core Idea:** Ask the LLM to first rephrase your request and state its plan before generating the full response, ensuring it has understood the task correctly.

* **When to Use:**
    * For complex, multi-faceted requests where misinterpretation is costly or likely.
* **How to Implement:** Add `First, rephrase my request to confirm your understanding. Then, execute the task.`
* **Example (Marketing Proposal):**
    * **Before:**
        ```
        Draft a marketing proposal for our new app launch.
        ```
    * **After:**
        ```
        Draft a marketing proposal for our new app launch. Before you begin, first summarize your understanding of my request, including the implicit goal and target audience. Then, write the full proposal.
        ```
    * *Improvement:* This forces the model to align with your intent upfront, preventing it from generating a detailed but fundamentally misguided response based on a wrong assumption.

---

## 12. Step-Back Prompting

> **Core Idea:** Guide the LLM to first consider broader concepts or principles before addressing a specific, nuanced question.

* **When to Use:**
    * For questions involving complex definitions, comparisons, or debated topics.
* **How to Implement:** Ask a general, principle-based question first, then ask your specific question.
* **Example (Concept Comparison):**
    * **Before:**
        ```
        What is the difference between machine learning and artificial intelligence?
        ```
    * **After:**
        ```
        First, provide a simple definition of artificial intelligence (AI).
        Next, provide a simple definition of machine learning (ML).
        Finally, using these definitions, explain the relationship between AI and ML and clarify if one is a subset of the other.
        ```
    * *Improvement:* By building from foundational principles, the model produces a clearer, more structured, and more accurate explanation of the nuanced relationship.

---

## 13. Self-Critique & Refinement

> **Core Idea:** Instruct the LLM to generate an initial response, critically evaluate it against given criteria, and then provide an improved version.

* **When to Use:**
    * To improve the quality of creative writing, complex explanations, or code.
* **How to Implement:** Provide explicit steps: 1. Generate. 2. Critique based on [criteria]. 3. Revise.
* **Example (Headline Generation):**
    * **Before:**
        ```
        Write a headline for a blog post about the benefits of remote work.
        ```
    * **After:**
        ```
        I need a headline for a blog post about the benefits of remote work. Please follow this process:
        1.  **Draft:** Generate one initial headline.
        2.  **Critique:** Analyze the draft. Is it engaging? Is it too generic? Does it create curiosity?
        3.  **Revise:** Based on your critique, provide a final, improved headline.
        ```
    * *Improvement:* This structured reflection forces the model to move beyond its first, often generic, idea and produce a more polished and effective final output.

---

## 14. Goal Decomposition Prompting

> **Core Idea:** Break down a large, complex task into smaller, explicit sub-goals within a single prompt.

* **When to Use:**
    * For any output that requires multiple distinct parts, like a report, a plan, or a detailed analysis.
* **How to Implement:** Use headings or a numbered list to outline the required sections of the output.
* **Example (Meeting Plan):**
    * **Before:**
        ```
        Create a plan for a project kickoff meeting.
        ```
    * **After:**
        ```
        Create a plan for a project kickoff meeting. Structure the output with the following headers:
        1.  **Objective:** (A single sentence stating the meeting's goal)
        2.  **Attendees:** (List of required roles)
        3.  **Agenda:** (A bulleted list of topics with time estimates)
        4.  **Desired Outcomes:** (What should be achieved by the end of the meeting)
        ```
    * *Improvement:* The output is perfectly structured, comprehensive, and immediately usable as a formal meeting plan.

---

## 15. Meta-Prompting

> **Core Idea:** Use the LLM to help you write a better prompt. Ask it to act as a prompt engineer and optimize a prompt for a specific task.

* **When to Use:**
    * When you're unsure how to best phrase a complex request.
    * To optimize a prompt for higher quality or more specific results.
* **How to Implement:** Describe your goal and ask the LLM to write the ideal prompt to achieve it.
* **Example (Recipe Generation):**
    * **Before (user's thought):**
        *I need a prompt to get good, simple chicken recipes.*
    * **After (Meta-Prompt):**
        ```
        You are a prompt engineering expert. I want to generate simple, healthy 30-minute chicken recipes. The output for each recipe must include an ingredients list, step-by-step instructions, and calorie count.

        Write the ideal, detailed prompt I should use to get the best possible results.
        ```
    * *Improvement:* The LLM generates a highly effective, structured prompt (likely incorporating techniques like Role Prompting and Goal Decomposition) for you to use for your actual task.

---

## 16. ReAct (Reason + Act)

> **Core Idea:** Instruct the model to follow a "Thought, Action, Observation" cycle to break down a problem that requires information synthesis or simulated tool use.

* **When to Use:**
    * Multi-hop question answering or fact verification.
    * Complex problems requiring the synthesis of different pieces of information.
* **How to Implement:** Provide a template for the Thought-Action-Observation loop.
* **Example (Fact Synthesis):**
    * **Before:**
        ```
        What programming language was used to create the operating system of the computer that guided the Apollo 11 mission to the moon?
        ```
    * **After:**
        ```
        Answer the following question by following a ReAct process: "What programming language was used to create the operating system of the Apollo 11 mission's computer?"

        Use this format:
        Thought: [Your reasoning on what you need to find out first]
        Action: [The specific question you need to answer]
        Observation: [The hypothetical answer to your action]
        ... repeat until you have the final answer.

        Final Answer: [Your final answer]
        ```
    * *Improvement:* This forces the model to explicitly break down the problem (1. What computer was used? 2. What OS did it run? 3. What language was the OS written in?), making its path to the answer transparent and more likely to be correct.

---

## 17. Thread-of-Thought (ThoT)

> **Core Idea:** Instruct the LLM to maintain a coherent, connected line of reasoning or narrative across a long and structured piece of text.

* **When to Use:**
    * Generating long-form content like articles, essays, or reports.
    * Explaining a complex process that requires logical flow.
* **How to Implement:** Provide a clear outline and explicitly instruct the model to connect each section logically.
* **Example (Business Strategy Doc):**
    * **Before:**
        ```
        Write about our company's Q4 strategy.
        ```
    * **After:**
        ```
        I need a document outlining our Q4 strategy. Structure it with these sections: 1. Q3 Performance Review, 2. Q4 Primary Objectives, 3. Key Initiatives, 4. Budget Allocation.

        Crucially, ensure you maintain a clear "thread of thought" by explicitly connecting how the learnings from Q3 inform the Q4 objectives, and how the initiatives and budget directly support those objectives.
        ```
    * *Improvement:* The prompt demands not just structure, but logical coherence *between* the structured parts, resulting in a persuasive and well-reasoned strategic document.

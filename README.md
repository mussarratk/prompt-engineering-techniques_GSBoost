# 🚀 Prompt Engineering Cheatsheet: 17 Techniques for 1B+ LLMs

This cheatsheet summarizes 17 powerful prompt engineering techniques, from beginner to advanced. Use these methods to get more accurate, creative, and reliable responses from Large Language Models (LLMs), including smaller 1B parameter models.

---

## Table of Contents

1.  [Zero-Shot Prompting](#1-zero-shot-prompting)
2.  [Few-Shot Prompting](#2-few-shot-prompting)
3.  [Role Prompting](#3-role-prompting)
4.  [Style Prompting](#4-style-prompting)
5.  [Emotion Prompting](#5-emotion-prompting)
6.  [Contextual Prompting](#6-contextual-prompting)
7.  [Chain-of-Thought (CoT) Prompting](#7-chain-of-thought-cot-prompting)
8.  [System Prompting](#8-system-prompting)
9.  [Explicit Instructions Prompting](#9-explicit-instructions-prompting)
10. [Output Priming](#10-output-priming)
11. [Rephrase and Respond (RaR)](#11-rephrase-and-respond-rar)
12. [Step-Back Prompting](#12-step-back-prompting)
13. [Self-Critique & Refinement](#13-self-critique--refinement)
14. [Goal Decomposition Prompting](#14-goal-decomposition-prompting)
15. [Meta-Prompting](#15-meta-prompting)
16. [ReAct (Reason + Act)](#16-react-reason--act)
17. [Thread-of-Thought (ThoT)](#17-thread-of-thought-thot)

---

## 1. Zero-Shot Prompting

> **Core Idea:** Ask the model to perform a task or answer a question directly, without providing any prior examples.

* **When to Use:**
    * Simple questions and answers.
    * Basic text summarization.
    * Quick brainstorming.
    * Tasks where the LLM likely has sufficient general understanding.

* **How to Implement:** Directly state your request. You can improve it by adding context, such as the target audience.

* **Example:**
    * **Before:**
        ```
        What is photosynthesis?
        ```
    * **After:**
        ```
        Explain photosynthesis to a 5-year-old child.
        ```
    * *Improvement:* The "After" prompt guides the model to produce a simpler, more engaging, and age-appropriate response.

---

## 2. Few-Shot Prompting

> **Core Idea:** Provide a few examples (input/output pairs) to show the model the desired task and format.

* **When to Use:**
    * Sentiment analysis with specific labels.
    * Simple code generation.
    * Data extraction or reformatting.
    * Generating text in a highly specific, non-standard style.

* **How to Implement:** Include examples of `user` input and desired `assistant` output before your actual query.

* **Example (Sentiment Classification):**
    * **Before (Zero-Shot):**
        ```
        Classify the sentiment of this movie review: 'The movie was okay, not great but not terrible.'
        ```
    * **After (Few-Shot):**
        ```
        User: Classify: 'This is the best movie I have ever seen!'
        Assistant: Positive

        User: Classify: 'I hated this film, it was a waste of time.'
        Assistant: Negative

        User: Classify: 'The movie was okay, not great but not terrible.'
        ```
    * *Improvement:* The model learns the desired single-word output format and provides a more direct answer.

---

## 3. Role Prompting

> **Core Idea:** Instruct the LLM to adopt a specific persona or character (e.g., "Act as a seasoned travel guide").

* **When to Use:**
    * Making interactions more engaging.
    * Explanations for specific audiences.
    * Generating creative content in a particular voice.
    * Simulating dialogues or expert perspectives.

* **How to Implement:** Typically at the start of a prompt: `You are [Role]...`

* **Example:**
    * **Before:**
        ```
        Tell me about black holes.
        ```
    * **After:**
        ```
        System: You are Professor Astra, a friendly and slightly eccentric astronomer who loves making complex topics exciting.
        User: Professor Astra, I'm curious! Can you tell me all about those mysterious black holes?
        ```
    * *Improvement:* The response becomes more engaging, vivid, and uses analogies fitting the persona.

---

## 4. Style Prompting

> **Core Idea:** Guide the LLM to write in a particular literary, artistic, formal, or informal style.

* **When to Use:**
    * Creative writing (stories, poems).
    * Content adaptation for different audiences.
    * Matching a specific brand voice.

* **How to Implement:** Specify the desired style in the prompt: `Write ... in the style of [style]`.

* **Example:**
    * **Before:**
        ```
        Write a short description of a sunset.
        ```
    * **After:**
        ```
        Write a short description of a sunset in the style of a haiku.
        ```
    * *Improvement:* The output adheres to specific stylistic constraints (e.g., 5-7-5 syllables for a haiku).

---

## 5. Emotion Prompting

> **Core Idea:** Instruct the LLM to generate a response conveying a specific emotion or from an emotional perspective.

* **When to Use:**
    * Adding emotional depth to creative writing.
    * Crafting messages with specific sentiment (e.g., thank you notes, apologies).
    * Generating empathetic customer service responses.

* **How to Implement:** Explicitly state the desired emotion: `Write this ... make it sound [emotion].`

* **Example (Thank You Note):**
    * **Before:**
        ```
        Write a thank you note for a gift I received.
        ```
    * **After:**
        ```
        Write a thank you note for a book I received as a gift. Make it sound very excited and deeply grateful because I've wanted this book for ages.
        ```
    * *Improvement:* The note becomes warmer, more personal, and emotionally expressive.

---

## 6. Contextual Prompting

> **Core Idea:** Provide the LLM with sufficient background information relevant to the request.

* **When to Use:**
    * Personalized recommendations.
    * Problem-solving with specific data.
    * Generating content about niche topics.
    * Maintaining continuity in multi-turn conversations.

* **How to Implement:** Include all necessary details, history, preferences, or constraints in your prompt.

* **Example (Gift Suggestion):**
    * **Before:**
        ```
        Suggest a gift.
        ```
    * **After:**
        ```
        I need a gift suggestion for my sister's 30th birthday. Her interests include fantasy novels, gardening, and tea. My budget is around $50.
        ```
    * *Improvement:* Suggestions become specific, relevant, and tailored to the recipient.

---

## 7. Chain-of-Thought (CoT) Prompting

> **Core Idea:** Encourage the LLM to "think step by step" or show its work, especially for tasks requiring multi-step reasoning.

* **When to Use:**
    * Mathematical word problems.
    * Logical reasoning puzzles.
    * Commonsense reasoning questions.
    * Debugging LLM responses by making the thought process visible.

* **How to Implement:** Add a phrase like "Let's think step by step." to your prompt.

* **Example (Math Word Problem):**
    * **Before:**
        ```
        Roger has 5 tennis balls. He buys 2 more cans of tennis balls. Each can has 3 tennis balls. How many tennis balls does he have now?
        ```
    * **After:**
        ```
        Roger has 5 tennis balls. He buys 2 more cans of tennis balls. Each can has 3 tennis balls. How many tennis balls does he have now? Let's think step by step.
        ```
    * *Improvement:* The LLM shows its reasoning, which increases the likelihood of a correct final answer for complex problems.

---

## 8. System Prompting

> **Core Idea:** Provide high-level instructions, context, or persona guidelines in a separate "system message" to apply across an entire conversation.

* **When to Use:**
    * Ensuring consistent LLM behavior or persona.
    * Defining overall goals or constraints for a session.
    * Simplifying user prompts by offloading standing instructions.

* **How to Implement:** Use the `system` role to provide persistent instructions.

* **Example (Concise Summarization):**
    * **Before (in User Prompt):**
        ```
        Summarize the following text in one sentence: '[text]'
        ```
    * **After (Using System Prompt):**
        ```
        System: You are a 'Concise Summarizer'. Your goal is to provide the shortest possible, grammatically correct summary. Aim for just one clear sentence.
        User: [text_to_summarize]
        ```
    * *Improvement:* This method is more robust and scalable for multiple summarization tasks that require the same constraint.

---

## 9. Explicit Instructions Prompting

> **Core Idea:** Be crystal clear, direct, and unambiguous in your requests, leaving little room for misinterpretation.

* **When to Use:**
    * When you need a specific output structure or format.
    * When you need to avoid certain topics.
    * When length or conciseness is important.
    * For complex tasks that could be interpreted in multiple ways.

* **How to Implement:** Detail exactly what you want: format, length, content to include/exclude, etc.

* **Example (Writing about Apples):**
    * **Before:**
        ```
        Write about apples.
        ```
    * **After:**
        ```
        Write a short paragraph about apples focusing on their nutritional benefits and common varieties. The paragraph should be exactly 3 sentences long. Mention at least two specific varieties. Do not discuss apple cultivation or history.
        ```
    * *Improvement:* The output is significantly more targeted and useful according to the stated requirements.

---

## 10. Output Priming

> **Core Idea:** Provide the LLM with the beginning of its desired response to nudge it towards a specific structure, format, or tone.

* **When to Use:**
    * Guiding the output format (e.g., lists, JSON).
    * Ensuring a specific tone or starting phrase.

* **How to Implement:** End your prompt in a way that naturally leads into the desired output format.

* **Example (Listing Ingredients):**
    * **Before:**
        ```
        What are the ingredients for a simple vanilla cake?
        ```
    * **After:**
        ```
        What are the ingredients for a simple vanilla cake? Please list them out.

        Here are the ingredients for a simple vanilla cake:
        -
        ```
    * *Improvement:* The LLM is cued by the priming and is highly likely to generate a bulleted list.

---

## 11. Rephrase and Respond (RaR)

> **Core Idea:** Ask the LLM to first rephrase your request or explain its understanding of the task before generating the main response.

* **When to Use:**
    * Complex, multi-faceted, or potentially ambiguous requests.
    * When the cost of an incorrect response is high.
    * To ensure the LLM has grasped all key constraints.

* **How to Implement:** Instruct the LLM to rephrase and wait for confirmation, or to state its plan first.

* **Example (Story Writing):**
    * **Before:**
        ```
        Write a story about a journey.
        ```
    * **After:**
        ```
        I'd like a story about a journey. First, briefly describe the kind of journey you are planning to write about (who, where, why). Then, write a short story based on your description.
        ```
    * *Improvement:* Forces the LLM to commit to a specific interpretation, leading to a more focused and relevant story.

---

## 12. Step-Back Prompting

> **Core Idea:** Guide the LLM to first consider broader concepts or general principles related to a specific question before answering it.

* **When to Use:**
    * Questions involving nuanced distinctions (e.g., "Is a virus alive?").
    * Complex or debated topics.
    * Problem-solving that benefits from first-principles thinking.

* **How to Implement:** Ask the LLM to explain general principles first, then apply them to your specific query.

* **Example (Tomato Classification):**
    * **Before:**
        ```
        Is a tomato a fruit or a vegetable?
        ```
    * **After:**
        ```
        First, please explain:
        1. What is the botanical definition of a fruit?
        2. What is the general culinary understanding of a vegetable?

        Now, using those definitions, explain whether a tomato is considered a fruit or a vegetable and in which contexts.
        ```
    * *Improvement:* The response is more comprehensive and educational, explaining the "why" behind the classification.

---

## 13. Self-Critique & Refinement

> **Core Idea:** Instruct the LLM to generate an initial response, critically evaluate its own response based on given criteria, and then generate a revised version.

* **When to Use:**
    * Improving creative tasks (slogans, story ideas).
    * Ensuring clarity and accuracy in complex explanations.
    * Refining summaries or generated code.

* **How to Implement:** Provide explicit steps for generation, critique (with criteria), and refinement.

* **Example (Slogan Generation):**
    * **Before:**
        ```
        Write a short, catchy slogan for a new eco-friendly water bottle.
        ```
    * **After:**
        ```
        I need a slogan for a new eco-friendly water bottle. Please follow these steps:
        1. Generate one initial slogan.
        2. Critically evaluate your slogan: Is it catchy? Does it communicate "eco-friendly"? What are its weaknesses?
        3. Based on your critique, provide one improved slogan.
        ```
    * *Improvement:* Encourages a more deliberative and refined generation process through structured self-reflection.

---

## 14. Goal Decomposition Prompting

> **Core Idea:** Break down a large, complex task into smaller, manageable sub-goals or steps within the prompt itself.

* **When to Use:**
    * Outputs with several distinct parts (e.g., planning an event, writing a report with sections).
    * Ensuring all necessary components of a request are addressed.
    * Guiding the LLM through a specific workflow.

* **How to Implement:** List the components or stages of the desired output for the LLM to address sequentially.

* **Example (Planning a Trip):**
    * **Before:**
        ```
        Plan a simple weekend trip to a nearby nature spot.
        ```
    * **After:**
        ```
        I want to plan a weekend trip. Please create a plan that includes the following sections:
        1. Destination: Suggest one specific type of nature spot.
        2. Packing List: List 3-4 essential items to pack.
        3. Itinerary: Suggest one main activity for Saturday and one for Sunday.
        4. Safety Tip: Provide one important safety tip relevant to the location.
        ```
    * *Improvement:* The output is far more structured, comprehensive, and actionable.

---

## 15. Meta-Prompting

> **Core Idea:** Use an LLM to help you create better prompts for another (or the same) LLM. Ask: "How should I ask you to do task X effectively?"

* **When to Use:**
    * When you are unsure how to best phrase a complex request.
    * To discover more effective ways to prompt for specific outputs.
    * To optimize prompts for quality, specificity, or creativity.

* **How to Implement:** Describe the task you want an LLM to do and the desired characteristics of the output, then ask the current LLM to write the ideal prompt for you to use.

* **Example (Generating Fantasy Story Ideas):**
    * **Before (User's internal thought):**
        ```
        "How do I ask for good story ideas?"
        ```
    * **After (Meta-Prompt):**
        ```
        I want to use an LLM to generate 3 distinct fantasy story ideas. For each idea, I need a unique main character, a compelling conflict, and a unique magical element.

        Please write the actual, detailed prompt I should use to get the best results.
        ```
    * *Improvement:* The LLM generates a much more detailed and structured prompt for the user to then use for their original task.

---

## 16. ReAct (Reason + Act)

> **Core Idea:** Enable an LLM to solve complex tasks by interleaving reasoning (breaking down the problem) with acting (simulating information gathering or tool use).

* **When to Use:**
    * Multi-hop question answering.
    * Fact verification.
    * Complex problem solving requiring decomposition and information synthesis.
    * Making the LLM's problem-solving process explicit.

* **How to Implement:** Instruct the LLM to follow a "Thought, Action, Observation" cycle until it can answer the main question.

* **Example (Multi-part Question):**
    * **Before:**
        ```
        Who was the U.S. president when the first person walked on the moon, and what was the name of that astronaut?
        ```
    * **After:**
        ```
        To answer "Who was the U.S. president when the first person walked on the moon, and what was the name of that astronaut?", please follow a ReAct-like process. For each step, state:
        Thought: [Your reasoning on what to do next]
        Action: [The specific information you need to find]
        Observation: [The hypothetical result of your action]
        ...continue this cycle until you have all the information needed.

        Finally, provide the final answer.
        ```
    * *Improvement:* Forces the LLM to break down the question and make its "knowledge lookup" process more explicit and reliable.

---

## 17. Thread-of-Thought (ThoT)

> **Core Idea:** Encourage the LLM to maintain a coherent and connected line of reasoning or narrative across a long piece of generated text.

* **When to Use:**
    * Long-form content generation (essays, articles).
    * Complex explanations requiring a logical flow.
    * Extended conversations where maintaining focus is key.

* **How to Implement:** Provide a structured outline and explicitly ask the LLM to ensure logical flow, use transition phrases, and connect each part to the previous one.

* **Example (Explaining a Complex Process):**
    * **Before:**
        ```
        Explain how a bill becomes a law in the US.
        ```
    * **After:**
        ```
        I need an explanation of how a bill becomes a law in the US. Please structure your explanation using the following outline:
        1. Introduction
        2. Bill Introduction and Sponsorship
        3. Committee Action
        4. Floor Action
        5. Presidential Action
        6. Conclusion

        Throughout your explanation, ensure each stage logically follows the previous one, maintaining a clear "thread of thought".
        ```
    * *Improvement:* The resulting explanation is much more coherent, structured, and easy to follow.

---

Happy prompting! ✨

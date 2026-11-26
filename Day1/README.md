# Day 1: First LLM Chatbot (Sweet Crumbs Bakery Assistant)

## Goal
Build a first chatbot that calls an LLM through LangChain and Groq, and give it a system prompt so it answers inside a defined task.

## What this notebook does
1. Installs and imports `langchain_groq`.
2. Reads the Groq API key from Colab Secrets (`GROQ_API_KEY`) and creates the model:
   `ChatGroq(model="openai/gpt-oss-120b", temperature=0)`.
3. Defines a system prompt for the Sweet Crumbs Home Bakery assistant:
   - **Facts:** bakes to order, 24 hours' notice, all cakes have eggless versions
   - **Rules:** never promise delivery times, say so if the answer is unknown
   - **Format:** polite reply in under 60 words
4. Wraps the model call in an `ask(question)` function that sends a `system` message and a `human` message.
5. Tests the bot with the question "can i get my order tomorrow".
6. Runs a **Prompt Guard** model (`meta-llama/llama-prompt-guard-2-22m`) on a prompt-injection example. It returns a **score** (close to 1 means the prompt looks like an injection or jailbreak attempt), not text.

## Key ideas
- Message roles: `system` (instructions), `human` (the person), `ai` (the model's reply)
- A system prompt keeps answers inside the task
- Prompt injection and how a guard model can flag it

## How to run
1. Get a free key at https://console.groq.com/keys
2. In Colab, open **Secrets** (key icon), add `GROQ_API_KEY`, and turn on **Notebook access**.
3. Run the cells in order.

## Files
- `Day01.ipynb`

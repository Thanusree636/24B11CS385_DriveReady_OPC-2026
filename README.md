# Day 5: Memory and RAG — The Little Lantern

## Purpose

This project applies the Day 5 task to the RAG workflow from `RAG-2.ipynb`. The original notebook extracts PDF text, creates embeddings, retrieves relevant content, and uses a Groq model to answer questions. The updated version uses ChromaDB to store story facts and adds an agent with conversation memory.

## What Is Implemented

### 1. Knowledge in ChromaDB

Eighteen separate facts from *The Little Lantern* are stored as individual documents in a ChromaDB collection. These facts cover Arjun, his grandmother, the lantern, the storm, the traveler, the help offered, the village's change, and the moral of the story.

Each fact has its own ID, allowing the system to retrieve relevant information for different questions.

### 2. Search Tool

The `search_story(question)` tool searches the ChromaDB collection for facts related to the user's question.

Its docstring lists the topics covered by the knowledge base. When a question is unrelated to the story or the information cannot be found, the tool returns `NO_MATCH` rather than treating unrelated information as a fact.

For example, when someone asks, "How did Arjun help the traveler?", the search tool retrieves the relevant facts about the lantern and the traveler.

### 3. Agent and Checkpointer

The search tool is connected to an agent using Groq's `openai/gpt-oss-20b` model.

The agent uses `InMemorySaver` as a checkpointer to maintain conversation history. Each conversation is identified by a `thread_id`.

Two different thread IDs, `reader-ravi` and `reader-sravani`, are used to test whether separate conversations maintain their own histories.

Using the same thread ID continues an existing conversation, while using a different thread ID creates a separate conversation.

### 4. Handling Unknown Questions

The chatbot is instructed to answer using information retrieved from the story. If the story does not contain the required information, it should respond:

"That information is not available in The Little Lantern story."

For example, the story does not specify Arjun's age, school name, or the name of the village. These questions help test whether the chatbot avoids making up answers.

If the chatbot guesses an unsupported answer, the system rules and retrieval threshold should be reviewed.

### 5. Testing and Pass Rate

The notebook includes eleven retrieval checks. Each test displays either PASS or FAIL, and the final cell calculates the overall pass rate.

The target given in the assignment is at least 80%.

The actual pass rate should be recorded after running the tests in Google Colab. It should not be assumed before execution.

## How to Run the Project

1. Download the corrected `RAG-2_Day5_Little_Lantern_Corrected.ipynb` notebook.
2. Open Google Colab.
3. Select **File → Upload notebook** and upload the notebook.
4. Run the cells from top to bottom.
5. Upload `The_Little_Lantern.pdf` when prompted.
6. Enter a valid Groq API key when requested. Keep the key private.
7. Run the conversation memory tests using both thread IDs.
8. Run the unsupported-question tests.
9. Run the retrieval tests and record the actual pass rate.
10. Run the interactive chatbot cell to ask your own questions. Type `exit` to stop the conversation.

## Example Questions

- Who is the main character in the story?
- Why does Arjun's grandmother light a lantern?
- What happened during the storm?
- How did Arjun help the traveler?
- What did Arjun and his grandmother offer the traveler?
- What did Arjun learn at the end of the story?
- What is the moral of the story?
- What is Arjun's school name?
- How old is Arjun?
- What is the name of the village?

The last three questions test whether the chatbot correctly identifies information that is missing from the story.

## Technologies Used

- **Python:** Main programming language.
- **Google Colab:** Environment for running the notebook.
- **Groq:** Provides access to the language model.
- **ChromaDB:** Stores and retrieves story facts.
- **Sentence Transformers:** Converts text into embeddings for similarity search.
- **LangChain:** Connects the language model and search tool.
- **LangGraph:** Creates the agent and manages conversation checkpoints.

## Limitations

- The knowledge base contains facts about the provided story rather than general world knowledge.
- ChromaDB and `InMemorySaver` use in-memory storage in this notebook, so their data is cleared when the Colab runtime restarts.
- Retrieval thresholds may need adjustment depending on the questions and embedding results.
- A successful retrieval test does not guarantee that every generated answer will be correct. Unsupported-question responses should also be checked manually.

## Final Checklist

- [ ] Store 15–20 facts in ChromaDB, one fact per document.
- [ ] Create `search_story()` with a docstring listing the covered topics.
- [ ] Add the search tool to the agent with a checkpointer.
- [ ] Test two different `thread_id` values.
- [ ] Ask questions that the story does not cover.
- [ ] Confirm that the chatbot does not invent unsupported facts.
- [ ] Run all tests and record the actual pass rate.
- [ ] Aim for a pass rate of at least 80%.

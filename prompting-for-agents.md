The YouTube video "Prompting 101" by Anthropic introduces **prompt engineering** as the practice of communicating with a language model to achieve desired outcomes by writing **clear instructions**, providing **context**, and **arranging information effectively**. It emphasizes that prompt engineering is an **iterative empirical science**, where you continuously build and refine prompts based on feedback and test cases.

The video illustrates this with a scenario involving a Swedish car insurance company analyzing accident reports and sketches. An initial simple prompt led Claude to mistakenly interpret the situation as a "skiing accident". To overcome such issues, the speakers recommend a structured approach for prompts, especially when using an API for task completion.

Key elements of a great prompt structure include:
*   **Task description:** Clearly define Claude's role and task upfront.
*   **Content:** Provide dynamic content like images and forms.
*   **Detailed instructions:** Offer a step-by-step list for Claude's reasoning.
*   **Examples:** Show Claude how to respond to certain content.
*   **Important reminders:** Emphasize critical information at the end.

Specific best practices highlighted include:
*   **Task and Tone Context:** Define the scenario (e.g., "AI assistant helping a human claims adjuster review car accident forms in Swedish") and the desired tone (e.g., factual and confident, not guessing).
*   **Background Detail/Data:** Provide static, unchanging information (like the structure of a form) in the **system prompt** to help Claude understand and process data more efficiently.
*   **Organization:** Claude benefits from structured input. Use **delimiters like XML tags** to label and organize information within the prompt, which helps Claude refer back to specific details.
*   **Few-Shot Examples:** Incorporating examples of data and desired responses can significantly "steer" Claude, especially for complex or tricky scenarios.
*   **Conversation History:** For user-facing applications, including conversation history in the system prompt can enrich Claude's context.
*   **Concrete Reminders:** Reminding Claude of the task and important guidelines (e.g., "answer only if very confident," "refer back to what you've seen in the form") helps prevent hallucinations and guides factual claims. The **order of operations** specified in these instructions is crucial (e.g., analyze the form *before* the sketch).
*   **Output Formatting:** Specify desired output formats using XML tags (e.g., `<final_verdict>`) for easy integration into other applications.
*   **Pre-filled Responses:** Start Claude's output with a specific format (e.g., a JSON structure or an XML tag) to shape its response and avoid unwanted preamble.
*   **Extended Thinking (Scratchpad):** Claude 3.7 and 4 models offer "extended thinking," where they output their reasoning process (thinking tags). Analyzing this internal thought process helps improve prompt engineering and understand how the model processes data.

The "Prompting for Agents" video shifts focus to **agents**, which are defined as **models using tools in a loop** to work continuously, update decisions, and complete complex tasks independently. Agents are best suited for **complex and valuable tasks** where the exact steps to accomplish the task are unclear, tools can be defined, and errors are recoverable. Examples include coding from a design document, search, computer use, and data analysis.

Key best practices for prompting agents include:
*   **Think Like Your Agents:** Develop a mental model of the agent's environment and how it interacts with tools. Simulate its process to understand potential confusion.
*   **Give Agents Reasonable Heuristics:** Instill concepts and desired behaviors (e.g., "avoid irreversible actions," "stop searching when the answer is found," "budget tool calls"). Be very clear to avoid misinterpretations.
*   **Tool Selection is Key:** Explicitly guide the agent on when and which tools to use, especially in specific contexts (e.g., which database to search). Avoid giving tools with similar names or descriptions.
*   **Guide the Thinking Process:** Prompt agents to use their extended thinking effectively by planning tasks, reflecting on results, and verifying information.
*   **Be Aware of Unintended Side Effects:** Autonomous agent operation means prompt changes can have unexpected outcomes (e.g., telling an agent to "keep searching" might lead to endless searching if the perfect source doesn't exist).
*   **Help Agents Manage Context Window:** Strategies include **compaction** (summarizing prior context), writing to **external files** for memory, and using **sub-agents** to delegate tasks and compress results.
*   **Let Claude Be Claude:** Start with **simple, barebones prompts** and tools. Iterate by adding instructions and examples only when specific edge cases or flaws are identified through testing.
*   **Good Tool Design:** Ensure tools have simple, accurate names, clear descriptions, are tested, and are distinct from one another.

Finally, the video discusses **evaluations for agents**, noting they are more challenging than for classification tasks due to their long-running and unpredictable nature. Recommendations include:
*   **Start Small and Manually:** Begin with a small, consistent set of realistic test cases rather than delaying with large, automated evals.
*   **Use LLM as Judge:** Employ an LLM with a clear rubric to evaluate agent outputs, as it is robust to variations in text structure.
*   **Evaluate Answer Accuracy:** Check if the agent's final answer is correct, robustly handling different formats.
*   **Evaluate Tool Use Accuracy:** Programmatically verify if the agent used the correct tools the appropriate number of times during its process.
*   **Tobenchi (Final State Evaluation):** This open-source benchmark allows checking if agents reach a desired final state (e.g., a database updated, a file modified).

The speakers also advise against overly prescriptive few-shot examples for agents, as modern models are smart enough to determine their own process. Instead, focus on guiding their thinking and providing less rigid examples.
## Basic Summary
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


## Elaboration

Certainly, let's delve deeper into the insights from the "Prompting 101" and "Prompting for Agents" YouTube videos by Anthropic.

### Prompting 101: Building Effective Prompts

**Prompt engineering** is defined as the practice of communicating with a language model to elicit desired behavior. This involves **writing clear instructions**, providing the necessary **context**, and **arranging information effectively** to achieve the best results. It's fundamentally an **iterative empirical science**, meaning you continuously build upon and refine your prompts based on testing and feedback.

The video illustrates this with a practical scenario: a Swedish car insurance company analyzing accident reports and hand-drawn sketches.

**Initial Prompting Challenges:**
*   A **very simple initial prompt** instructing Claude to review an accident report and determine fault led to a misinterpretation, with Claude believing it was a "skiing accident".
*   This highlights the need for more detailed instruction and context, as Claude, without proper guidance, will often make innocent (but incorrect) assumptions.

**Structure of a Great Prompt (for API-based task completion):**
When interacting with Claude via an API for a specific task rather than a conversational chatbot style, a structured approach is recommended. A robust prompt structure includes:
*   **Task Description:** Clearly state Claude's role and the task it needs to accomplish upfront.
*   **Content:** Provide all dynamic information relevant to the task, such as images (like the form and sketch in the example) or data retrieved from other systems.
*   **Detailed Instructions:** Offer a step-by-step list guiding Claude's reasoning process.
*   **Examples (Few-Shot):** Show Claude how it should respond to certain content or scenarios.
*   **Important Reminders:** Emphasize critical information and guidelines at the end of the prompt.

**Best Practices for Prompt Development:**
1.  **Task and Tone Context:**
    *   **Specify Claude's Role and the Scenario:** Clearly define who Claude is (e.g., "AI assistant helping a human claims adjuster") and the environment it's operating in (e.g., "reviewing car accident report forms in Swedish"). This prevents misinterpretations, like the initial "skiing accident" error, by giving Claude clear boundaries.
    *   **Set the Desired Tone:** Crucially, instruct Claude to be **factual and confident**, and to **avoid guessing** if it cannot confidently understand the information. This ensures reliability, especially for critical assessments like determining fault.

2.  **Background Detail/Data (in System Prompt):**
    *   **Provide Static Information:** For information that remains constant across queries (e.g., the structure of a specific form), include it in the **system prompt**.
    *   **Benefits:** This helps Claude spend less time analyzing the basic structure each time, leading to more efficient and accurate data processing. For the car accident example, this included details about the form's 17 checkboxes, columns for Vehicle A and B, and how humans might imperfectly fill it out (scribbles, circles instead of Xs).

3.  **Organization with Delimiters:**
    *   **Structure is Key:** Claude processes structured input much more effectively.
    *   **Use XML Tags or Markdown:** Tools like **XML tags** are highly recommended because they allow you to specifically label and categorize different sections of information within the prompt (e.g., `<user_preferences>`, `<final_verdict>`). This helps Claude refer back to specific pieces of information later in its reasoning process.

4.  **Examples (Few-Shot Prompting):**
    *   **Powerful Steering Mechanism:** Providing examples of data inputs and their corresponding desired outputs (even visual ones encoded in Base64) can significantly "steer" Claude's behavior, especially for complex or "tricky" scenarios.
    *   **Empirical Science:** This allows you to iteratively refine your prompt by adding examples that address specific edge cases where the model initially fails. For an insurance company, this could involve tens or hundreds of examples of difficult or "gray area" accidents.

5.  **Conversation History:**
    *   **Context for User-Facing Applications:** While not used in the car accident demo (as it's a backend process), for user-facing LLM applications with ongoing dialogues, including past conversation turns in the system prompt enriches Claude's context and understanding.

6.  **Concrete Reminders and Order of Operations:**
    *   **Prevent Hallucinations:** Reminding Claude of important guidelines at the end helps prevent it from inventing details or making unsubstantiated guesses. For instance, instruct it to "answer only if very confident" or to "refer back to what it has seen in the form" when making factual claims.
    *   **Guide Reasoning Flow:** The **order in which Claude analyzes information is crucial**. For the car accident scenario, instructing Claude to *first* carefully analyze the form and *then* move to the sketch ensures it interprets the less clear sketch with the context gained from the form. This structured thinking leads to more accurate assessments. An observed side effect of such detailed instruction is that Claude might "show its work" more extensively.

7.  **Output Formatting:**
    *   **Streamline Data Extraction:** To integrate Claude's output into other applications (like a SQL database or an analytics tool), specify a desired output format using delimiters like **XML tags** (e.g., `<final_verdict>`). This allows a data engineer to easily parse out only the "nitty-gritty information" needed, ignoring any preamble.

8.  **Pre-filled Responses:**
    *   **Shape Initial Output:** You can explicitly tell Claude to **begin its output with a certain format** (e.g., an opening XML tag or a JSON structure). This is a simple yet effective way to shape Claude's response from the very start, avoiding unwanted introductory text.

9.  **Extended Thinking (Scratchpad):**
    *   **Internal Reasoning:** Claude 3.7 and especially Claude 4 models offer **"extended thinking,"** which produces "thinking tags" or a "scratchpad" showing the model's internal reasoning process.
    *   **Crutch for Prompt Engineering:** Analyzing this thinking process is invaluable for prompt engineering, as it helps you understand how Claude interprets data and identify areas where your prompt can be improved to better guide its reasoning.

### Prompting for Agents: Models Using Tools in a Loop

**Agents** are defined as **models using tools in a loop**. They are given a task and work continuously, updating decisions based on tool feedback, and operating independently until the task is complete.

**When to Use Agents:**
Agents are not always necessary and can be resource-intensive. They are best suited for:
*   **Complex Tasks:** Where the exact step-by-step process is **unclear** to a human, but the desired end state is known. If you can easily define the steps, a simpler workflow might be better.
*   **Valuable Tasks:** When the successful completion of the task by the agent yields **high value** (e.g., revenue-generating, saving significant human effort).
*   **Doable Parts (Tool Definable):** When the necessary tools for the agent to accomplish the task can be **clearly defined and provided**. If tools or information are inaccessible, the task might need to be scoped down.
*   **Recoverable Errors:** If errors that occur during the agent's independent operation are **not too costly** or can be easily corrected. If errors are hard to detect or costly, a human-in-the-loop approach might be better.

**Examples of Agent Use Cases:**
*   **Coding:** Going from a design document to a pull request (PR) where the exact steps are unknown, but the end goal is clear and high-value.
*   **Search:** Errors are recoverable through citations and double-checking.
*   **Computer Use:** Errors can be recovered by re-attempting actions.
*   **Data Analysis:** Knowing desired insights or visualizations but not the exact process due to varying data formats or issues.

**Best Practices for Prompting Agents:**
1.  **Think Like Your Agents:**
    *   **Mental Model of Environment:** Develop a deep understanding of the agent's environment (its tools and their responses).
    *   **Simulate the Process:** Imagine yourself in the agent's "shoes" to identify potential confusion or unclear instructions. If a human can't understand it, an AI won't either.

2.  **Give Agents Reasonable Heuristics (Conceptual Engineering):**
    *   **Instill Concepts and Behaviors:** Prompting for agents isn't just about words; it's about defining the **concepts** and **behaviors** the model should follow.
    *   **Be Explicit and Clear:** Provide clear, crisp instructions to avoid misinterpretation. Examples include:
        *   **Irreversibility:** Claude Code is instructed to avoid actions that might cause irreversible harm to the user's environment.
        *   **Stopping Criteria:** For research agents, tell them to "stop searching when the answer is found" to prevent unnecessary tool calls.
        *   **Budgeting Tool Calls:** Guide agents on how many tool calls to use for simple vs. complex queries (e.g., under five for simple, up to 10-15 for complex).
    *   Think of it like training a new intern – be very precise about principles and practices.

3.  **Tool Selection is Key:**
    *   **Guide Tool Usage:** Even with many tools available (Claude Sonnet 4 and Opus 4 can handle hundreds), you must explicitly guide the agent on **when and which tools to use** in specific contexts (e.g., default to searching Slack for company info).
    *   **Avoid Similar Tools:** Giving agents multiple tools with very similar names or descriptions (e.g., six slightly different search tools) can confuse the model; combine them if possible.

4.  **Guide the Thinking Process:**
    *   **Prompt for Effective Thinking:** While modern models often use chain-of-thought inherently, you can further optimize performance by prompting the agent to use its thinking process effectively.
    *   **Planning and Reflection:** Instruct agents to plan their tasks (e.g., "plan out its search process," "decide how many tool calls should I use").
    *   **Interleaved Thinking:** Use the new capability in Claude 4 models to prompt for reflection *between* tool calls (e.g., reflecting on the quality of search results, verifying information, adding disclaimers).

5.  **Be Aware of Unintended Side Effects:**
    *   **Autonomous Loops:** Because agents operate autonomously in a loop, changes to prompts can have unexpected outcomes.
    *   **Example:** Telling an agent to "keep searching until you find the highest quality possible source" might lead to it endlessly searching if such a perfect source doesn't exist, hitting the context window limit. You might need to add instructions like "you can stop after a few tool calls".

6.  **Help Agents Manage Context Window:**
    *   **Strategies for Long Tasks:** Even with large context windows (200k tokens for Claude 4), long-running agent tasks can hit limits. Strategies include:
        *   **Compaction:** A tool that summarizes or compresses prior context into a dense, accurate summary when the window limit is approached, then passes it to a new instance of Claude.
        *   **External Files:** Agents can be prompted to write memory to external files, effectively extending their context.
        *   **Sub-Agents:** Delegate parts of a complex task to sub-agents, who then compress their results back to a lead agent, reducing the context load on the main agent.

7.  **Let Claude Be Claude (Start Simple):**
    *   **Begin with Barebones:** Start with **simple, barebones prompts and tools**.
    *   **Iterate on Flaws:** Claude is often surprisingly capable out of the box. Only add more instructions or examples when you identify specific edge cases or flaws during testing.

8.  **Good Tool Design:**
    *   **Clarity is Key:** Ensure tools have **simple, accurate names**, **clear descriptions**, and are **well-tested**.
    *   **Distinct Tools:** Avoid giving agents tools with very similar names or descriptions, as this can confuse the model.

### Evaluations for Agents

Evaluating agents is **more challenging** than for simpler tasks like classification, due to their long-running, unpredictable nature.

**Tips for Agent Evaluations:**
1.  **Start Small and Manually:**
    *   **Avoid Delaying:** Don't wait to set up huge, fully automated evaluations with hundreds of test cases when starting out.
    *   **Get Started Quickly:** Begin with a **small, consistent set of realistic test cases** and evaluate manually to gain quick signal on prompt effectiveness. Consistency in test cases is vital for measuring progress.
    *   **Realistic Tasks:** Ensure your test cases reflect real-world tasks your agent will perform, rather than arbitrary or competitive programming problems.

2.  **Use LLM as Judge:**
    *   **Robust to Variation:** An LLM can be used as an evaluator, especially when given a **clear rubric**. This is powerful because LLMs are robust to variations in text structure or output formats (e.g., "47" vs. "forty-seven") that would break programmatic checks.
    *   **Example Rubric:** For search tasks, an LLM judge might evaluate if the model looked at the right sources, got the correct answer, or if the numerical answer falls within a realistic range.
    *   **Human Evals are irreplaceable:** Despite LLM judges, manual human evaluation of transcripts and model behavior remains critical for deep understanding and progress.

3.  **Evaluate Answer Accuracy:**
    *   Check if the agent's final answer is correct. An LLM as a judge is good for this because it can handle varied output formats.

4.  **Evaluate Tool Use Accuracy:**
    *   Programmatically verify if the agent used the **correct tools** and the **appropriate number of times** throughout its process (e.g., "should use web search at least five times").

5.  **Tobenchi (Final State Evaluation):**
    *   **Open-Source Benchmark:** This benchmark allows you to evaluate whether agents reach a **desired final state**.
    *   **Example:** For a customer service agent changing a flight, you'd check if the flight was indeed changed in the database at the end of the process. This is a robust method for many use cases.

**Regarding Few-Shot Examples for Agents:**
*   **Less Prescriptive for Agents:** Unlike traditional prompting for classification, **few-shot examples are generally less effective for state-of-the-art models and agents if they are too prescriptive**.
*   **Focus on Guiding Thinking:** Modern models are "smarter than you can predict" and have chain-of-thought capabilities trained in. Instead of dictating exact processes, focus on telling the model *how to use its thinking* (e.g., "use your thinking process to plan out your search") and reminding it of specific things in its thought process. Examples should be less rigid for agents.
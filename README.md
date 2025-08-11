# Deep-Research-Agent-with-Flowise-AI

Here's a GitHub README file based on the information provided in the sources:

```markdown
# 🧠 Intelligent Deep Research Agent (Flowise AI)

This repository contains the Flowise AI workflow for building a powerful, multi-agent deep research system designed to provide comprehensive and accurate reports with citations, addressing complex queries without common issues like hallucination.

## 🌟 Overview

Inspired by Anthropic's multi-agent research system, this Flowise AI project demonstrates how to create a sophisticated research agent that can iteratively refine its approach, utilize various research tools, and synthesize findings into a detailed, structured report. It was developed to overcome challenges faced with other research models, such as Perplexity Sonar, which sometimes "hallucinated" information.

**Key features include:**
*   **Multi-Agent Architecture**: Leverages an orchestrator (Planner) to manage and coordinate specialized sub-agents.
*   **Iterative Research Process**: Dynamically refines research plans and spawns additional sub-agents based on initial findings, ensuring thorough investigation.
*   **Robust Tool Integration**: Sub-agents are equipped with powerful research tools, including:
    *   **Web Search (Tavily API)**: For broad information gathering.
    *   **Web Scraper**: To extract content from web pages.
    *   **Arxiv Access**: To delve into academic research papers.
*   **Structured Output**: The Planner generates a clear, JSON-structured list of sub-agent tasks, facilitating organized execution.
*   **Consolidated Findings**: The iteration node efficiently collects and consolidates all research outputs from sub-agents.
*   **High-Quality Report Generation**: A dedicated Writer agent transforms raw research findings into a well-structured, long-form markdown report, complete with a compelling title, abstract (200-300 words), detailed insights, and precise citations.
*   **Self-Correction Mechanism**: A Condition node evaluates the generated report against the original query. If further research is needed, the system intelligently loops back to the Planner for additional investigation, ensuring comprehensive coverage.

## 🏗️ How It Works (The Flow)

The deep research agent operates through a series of interconnected nodes in Flowise AI:

1.  **Start Node (Form Input)**:
    *   Initiates the flow by receiving the user's research query through a form input. The query is stored as a `query` variable in the flow state.
    *   Initializes `subAgents` (list of sub-agents) and `findings` (research results) variables in the flow state.

2.  **Planner Agent (LLM)**:
    *   An LLM node configured as the "Planner" using **GPT-4.1 Mini**.
    *   Takes the user's `query` and, based on a system prompt, formulates a comprehensive research strategy.
    *   Generates a **JSON array of specific sub-agent tasks** (e.g., "research OpenAI's training models," "research Meta's target markets").
    *   **Updates the `subAgents` state variable** with this list of tasks.

3.  **Iteration Node**:
    *   Receives the `subAgents` array from the Planner agent's output.
    *   **Iterates through each task** in the `subAgents` array, dynamically spawning a "Sub Agent" for each.
    *   Consolidates all the outputs from the dynamically spawned sub-agents.

4.  **Sub Agent (Agent Node)**:
    *   An Agent node, named "Sub Agent," also using **GPT-4.1 Mini**.
    *   For each iteration, it receives a **specific `research task`** (e.g., "research OpenAI's approach to training models") from the Iteration node.
    *   **Utilizes integrated tools** (Tavily API for web search, Web Scraper, and Arxiv) to complete its assigned task.
    *   Produces a detailed report, including citations, for its specific area of research.

5.  **Writer Agent (LLM)**:
    *   An LLM node, named "Writer," connected to the Iteration node's consolidated output, using **GPT-4.1 Mini**.
    *   Receives all the raw findings from the sub-agents.
    *   Its system prompt instructs it to synthesize these findings into a **clear, structured, long-form markdown report**. This report includes a compelling title, a 200-300 word abstract, and detailed insights with citations.
    *   **Disables memory** to prevent context length issues with extensive conversation history.
    *   Takes the original `research topic` (query), existing `findings`, and the `new findings` (from the iteration node) to formulate the report.
    *   **Updates the global `findings` state variable** with the newly generated report.

6.  **Condition Agent Node ("more sub agents")**:
    *   Connected to the Writer agent's output, using **GPT-4.1 Mini**.
    *   Evaluates the generated report based on the original `research topic`, `previous sub agents`, and their `findings`.
    *   Determines whether **"more sub agents are needed"** or if the **"findings are sufficient"** to fully address the query.

7.  **Loop Node ("back to planner")**:
    *   If the Condition node determines **"more sub agents are needed,"** this node is activated.
    *   It **loops the flow back to the Planner agent**, allowing for an iterative refinement of the research plan and spawning of new sub-agents for deeper investigation. The maximum loop count is set to 5.

8.  **Direct Reply Node ("generate report")**:
    *   If the Condition node determines the **"findings are sufficient,"** this node is activated.
    *   It retrieves the final, comprehensive report from the `findings` state variable and **presents it directly to the user**.

## ⚙️ Setup and Installation

To set up and run this deep research agent in Flowise AI, follow these steps:

1.  **Install Flowise AI**: If you haven't already, set up your Flowise AI instance. You can find instructions on the official Flowise AI documentation.
2.  **Download the Flow**: The complete Flowise AI workflow file can be downloaded from the description of the original YouTube video: ["Build a DEEP Research Agent That Doesn't Suck (Flowise AI Tutorial)"](https://www.youtube.com/watch?v=YOUR_VIDEO_LINK_HERE). *[Note: The provided transcript does not include the direct link, so you'll need to find the video on Leon van Zyl's channel and check its description.]*
3.  **Import the Flow**: In your Flowise AI interface, import the downloaded workflow file.
4.  **Configure Credentials**:
    *   **OpenAI API Key**: This flow extensively uses OpenAI's GPT-4.1 Mini model for its Planner, Sub Agents, Writer, and Condition nodes. You will need to provide your OpenAI API credentials within Flowise AI.
    *   **Tavily API Key**: For web search capabilities, the sub-agents use Tavily.
        *   Go to [tavily.com](https://tavily.com) and sign up for an account. Free credits are available.
        *   Copy your API key from your Tavily dashboard.
        *   In Flowise AI, create a new credential for Tavily API and paste your API key.
5.  **Review Node Configurations**: While the imported flow should be pre-configured, it's good practice to double-check the settings for each node (LLM models, system prompts, tool selections). The system prompts are crucial for the agent's behavior.

## 🚀 Usage

Once the flow is set up and configured in Flowise AI:

1.  Open the chat interface for the "Deep Research Tutorial" flow.
2.  You will be presented with a **form input** instead of a chat interface.
3.  Enter your **research query** into the provided input field (e.g., "The differences between OpenAI, Meta, and Anthropic in how they approach training models and their target markets").
4.  Submit the query. The agent will then process it, spawning sub-agents, performing research, synthesizing findings, and iterating if necessary, to produce a detailed report with citations.

*****

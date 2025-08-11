# 🧠 Intelligent Deep Research Agent (Flowise AI)

> A powerful multi-agent research workflow in [Flowise AI](https://flowiseai.com/) that generates **high-quality, citation-backed reports** for complex queries — without the usual hallucination pitfalls.

---

## 🌟 Overview

Inspired by Anthropic's multi-agent research framework, this project implements a **sophisticated research agent** capable of:

- Iteratively refining its research approach  
- Coordinating specialized sub-agents  
- Using multiple research tools in harmony  
- Producing structured, long-form markdown reports with citations  

It was designed to overcome limitations found in some other research tools (e.g., hallucinated sources, shallow coverage).

---

## ✨ Key Features

- **🧩 Multi-Agent Architecture** – Orchestrator (“Planner”) manages and coordinates specialized sub-agents.  
- **🔁 Iterative Research Process** – Dynamically refines plans, spawning additional sub-agents for deeper coverage.  
- **🛠️ Integrated Research Tools**:  
  - **Tavily API** – Broad web search  
  - **Web Scraper** – Extracts on-page content  
  - **ArXiv Access** – Academic paper retrieval  
- **📋 Structured Task Planning** – Planner outputs a JSON-structured sub-agent task list.  
- **📚 Consolidated Findings** – Iteration node merges all sub-agent outputs.  
- **📝 High-Quality Report Generation** – Writer agent produces a title, 200–300 word abstract, detailed sections, and citations.  
- **✅ Self-Correction Loop** – Automatically re-researches if coverage is incomplete (up to 5 iterations).

---

## 🏗 How It Works

The workflow is built as a series of connected nodes inside Flowise AI:

1. **Start Node (Form Input)**  
   - Receives the research query (`query`) from the user.  
   - Initializes `subAgents` (task list) and `findings` (results).  

2. **Planner Agent (GPT-4.1 Mini)**  
   - Generates a **JSON array of research tasks**.  
   - Updates the `subAgents` state variable.  

3. **Iteration Node**  
   - Loops over each task in `subAgents`.  
   - Spawns a Sub Agent for each task.  
   - Consolidates all results.  

4. **Sub Agent (GPT-4.1 Mini + Tools)**  
   - Handles a single research task.  
   - Uses Tavily Search, Web Scraper, and ArXiv tools.  
   - Outputs a **detailed, citation-rich report** for its task.  

5. **Writer Agent (GPT-4.1 Mini)**  
   - Synthesizes findings into a **long-form markdown report**.  
   - Includes title, abstract, detailed insights, and citations.  
   - Updates the global `findings` variable.  

6. **Condition Agent**  
   - Decides whether further research is needed.  

7. **Loop Node**  
   - If more research is required, sends control back to the Planner.  

8. **Direct Reply Node**  
   - If research is complete, outputs the final report to the user.  

---

## ⚙️ Setup & Installation

1. **Install Flowise AI**  
   - Follow the [Flowise AI Installation Guide](https://docs.flowiseai.com/).  

2. **Get the Workflow File**  
   - Download from Leon van Zyl’s YouTube tutorial:  
     ["Build a DEEP Research Agent That Doesn't Suck (Flowise AI Tutorial)"](https://www.youtube.com/watch?v=YOUR_VIDEO_LINK_HERE)  
     *(Check the video description for the `.json` flow file.)*

3. **Import into Flowise AI**  
   - Use the **Import** feature in the Flowise dashboard.

4. **Set Up Credentials**  
   - **OpenAI API Key** – Required for Planner, Sub Agents, Writer, and Condition nodes.  
   - **Tavily API Key** – Required for web search.  
     - Sign up at [tavily.com](https://tavily.com) for free credits.  
     - Add your key in Flowise under **Credentials**.

5. **Review Node Settings**  
   - Verify LLM models, prompts, and tool configurations.

---

## 🚀 Usage

1. Open the **Deep Research Tutorial** flow in Flowise AI.  
2. Enter your **research query** (e.g.,  
   > The differences between OpenAI, Meta, and Anthropic in how they approach training models and their target markets  
   ).  
3. Submit the query.  
4. The agent will:
   - Plan the research  
   - Spawn sub-agents  
   - Gather findings  
   - Iterate if necessary  
   - Output a **comprehensive, citation-rich report**  

---

## 📜 Credits

- **Leon van Zyl** – Original tutorial creator  
- **Anthropic** – Inspiration for the multi-agent research concept

---

## 📄 License

This project is provided for **educational purposes**. Check the licenses of all third-party tools and APIs used before commercial deployment.

---

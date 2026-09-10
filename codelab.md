# Building Intelligent AI Agents with Antigravity IDE and Google Agent Development Kit (ADK)

**Created by:** [Shadrack Inusah](https://github.com/KojoShaddy)  
**Google Developer Expert, Cloud AI**

Welcome to the Google Agent Development Kit (ADK) Codelab!

In this hands-on tutorial, you will learn how to build, run, and interact with an AI agent using the Google ADK Python framework and Antigravity IDE.

By the end of this codelab, you will have created a working AI agent, configured it to use Gemini, tested it from the terminal and browser, and customised its behaviour with dynamic context.

> **Note:** This codelab focuses on getting your first ADK agent running. It provides a foundation for exploring more advanced capabilities such as tools, multi-agent systems, MCP, evaluation, and deployment.

## What You Will Build

By the end of this codelab, you will:

- Set up a Python environment for Google ADK.
- Create an ADK agent using the ADK CLI.
- Configure Gemini API access.
- Explore the structure of an ADK agent project.
- Run your agent from the terminal.
- Interact with your agent through the ADK web interface.
- Customise your agent with dynamically generated context.

---

## Table of Contents

1. [Introduction to Google ADK](#1-introduction-to-google-adk)
2. [Prerequisites & Environment Setup](#2-prerequisites--environment-setup)
3. [Scaffolding a New Agent](#3-scaffolding-a-new-agent)
4. [Exploring the Agent Structure](#4-exploring-the-agent-structure)
5. [Configuring API Credentials](#5-configuring-api-credentials)
6. [Running Your Agent](#6-running-your-agent)
   - [Method A: Terminal CLI](#method-a-terminal-cli)
   - [Method B: Interactive Web UI](#method-b-interactive-web-ui)
7. [Customising the Agent with Dynamic Context](#7-customising-the-agent-with-dynamic-context)
8. [Conclusion & Next Steps](#8-conclusion--next-steps)

---

## 1. Introduction to Google ADK

The **Google Agent Development Kit (ADK)** is an open-source, code-first framework for building, testing, and deploying AI agents.

ADK provides developers with a structured way to define agents, instructions, tools, models, and agent workflows using code. It is designed to support the development of agents from local experimentation through to production deployments.

### Key advantages of ADK

- **Code-first development:** Define agent behaviour, instructions, tools, and workflows using familiar programming patterns.
- **Model flexibility:** Use supported Google AI and Google Cloud backends depending on your application and deployment requirements.
- **Built-in development tools:** Use the ADK CLI to create, run, and interact with agents during development.
- **Extensibility:** Extend agents with tools, custom logic, multi-agent workflows, and other capabilities.
- **Production ready:** ADK can be used as a foundation for deploying agents to Google Cloud environments.

For this codelab, we will use **Python**, **Gemini**, and **Antigravity IDE**.

---

## 2. Prerequisites & Environment Setup

Before you begin, make sure you have the following installed and ready.

### Prerequisites

- **[Python 3.10+](https://www.python.org/downloads/)** – Required for running the Google ADK Python package.
- **[Git Bash](https://git-scm.com/download/win)** – Recommended for Windows users because this codelab includes Bash commands.
- **[Antigravity IDE](https://www.antigravity.dev/)** – Recommended development environment for this codelab.
- **[VS Code](https://code.visualstudio.com/)** – Optional alternative if you prefer to use another development environment.
- A **Google AI Studio API key** for accessing Gemini through the Google AI backend.
- An active internet connection.

> **Recommended:** Use the same versions and environment configuration demonstrated during the workshop. ADK is actively evolving, so commands, generated files, and available model options may change between releases.

### Setup Checklist

Before continuing, make sure you have:

- [ ] Python installed
- [ ] Antigravity IDE installed
- [ ] Git Bash installed if you are using Windows
- [ ] A Google AI Studio API key
- [ ] A working internet connection

### Step A: Workspace Setup in Antigravity IDE or VS Code

1. Open **Antigravity IDE**.

   > **Alternative:** You can use VS Code if Antigravity IDE is not available.

2. Click on **File** > **Open Folder** (or click **Open Folder** on the welcome screen).

3. Create a new directory named `Agent` on your local system (for example, on your Desktop) and select it to open the workspace.

4. Open the integrated terminal in the IDE:
   - **Antigravity IDE:** Go to **Terminal** > **New Terminal** or press `Ctrl + \``.
   - **VS Code:** Go to **Terminal** > **New Terminal** or press `` Ctrl + ` ``.

### Step B: Initialise a Virtual Environment

Using a virtual environment keeps your project dependencies isolated and helps avoid package conflicts. It also helps prevent the common mistake of installing the unrelated `adk` package instead of Google's `google-adk` package.

```bash
# Create the virtual environment
python -m venv .venv

# Activate the virtual environment
# On Windows (Command Prompt):
.venv\Scripts\activate.bat

# Windows Bash / Git Bash
source ./.venv/Scripts/activate

# On Windows (PowerShell):
.\.venv\Scripts\Activate.ps1

# On macOS/Linux:
source .venv/bin/activate
```

After activation, your terminal should indicate that the `.venv` environment is active.

### Step C: Install the Correct SDK Package

Run the following command to install the official Google Agent Development Kit package:

```bash
pip install google-adk
```

> [!WARNING]
> Do NOT run `pip install adk`. This installs an unrelated RPC development library and is not the Google Agent Development Kit package.

> **Version note:** For workshops and reproducible codelabs, it is recommended to record the `google-adk` version that was tested with this tutorial.

You can check the installed version with:

```bash
pip show google-adk
```

---

## 3. Scaffolding a New Agent

Now that your environment is ready, use the ADK CLI to generate the initial project structure for your agent.

To automatically generate the boilerplate files for your agent, run:

```bash
adk create my_agent
```

During this command, the CLI prompts you to configure the initial agent:

1. **Choose a model:** Select `1` for `gemini-3.5-flash`.
2. **Choose a backend:** Select `1` for `Google AI`.
3. **Google API Key:** Enter your key, or enter a placeholder and configure it later.

> [!NOTE]
> The available model and backend options may vary depending on the version of Google ADK installed. Follow the options presented by your installed version of the CLI.

> [!NOTE]
> On Windows, some terminal configurations may display a `UnicodeEncodeError` after the files have already been created successfully. If the `my_agent` directory and the expected files were created, you can continue with the next step.

The generated project provides the basic structure required to start developing your agent.

---

## 4. Exploring the Agent Structure

Navigate to your newly created directory to find the following files:

```text
my_agent/
├── .env          # Stores credentials and API configuration
├── .gitignore    # Prevents sensitive credentials (.env) from being committed
├── __init__.py   # Marks the directory as a Python package
└── agent.py      # Main file defining the agent logic
```

### The Agent Definition (`agent.py`)

Open `my_agent/agent.py`. By default, it initializes a basic agent:

```python
from google.adk.agents.llm_agent import Agent

root_agent = Agent(
    model='gemini-3.5-flash',
    name='root_agent',
    description='A helpful assistant for user questions.',
    instruction='Answer user questions to the best of your knowledge',
)
```

At this stage, the agent has a model, a name, a description, and an instruction that defines how it should respond.

You will customise this agent later in the codelab.

---

## 5. Configuring API Credentials

Open `my_agent/.env`. It should look like this:

```ini
GOOGLE_GENAI_USE_ENTERPRISE=0
GOOGLE_API_KEY=YOUR_GEMINI_API_KEY
```

Replace `YOUR_GEMINI_API_KEY` with a valid Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey).

> [!WARNING]
> Never commit your API key to Git or include it directly in your Python source code. Keep credentials in your environment configuration and make sure `.env` remains excluded through `.gitignore`.

If you are using Git, it is good practice to verify that `.env` is not being tracked before pushing your project to a remote repository.

---

## 6. Running Your Agent

Make sure your virtual environment is active, and then choose one of the options below to interact with your agent.

### Method A: Terminal CLI

You can test your agent directly in the terminal using the ADK CLI:

```bash
# Set console encoding to UTF-8 on Windows to avoid emoji errors
export PYTHONIOENCODING="utf-8"

# Run in interactive CLI mode
adk run my_agent
```

You will see an interactive prompt where you can chat directly with your agent.

Try questions such as:

- `What can you help me with?`
- `Explain what an AI agent is.`
- `Give me three ideas for building with Google ADK.`

The terminal runner is useful for quickly testing agent behaviour without starting a web interface.

---

### Method B: Interactive Web UI

ADK also provides a local web interface for developing and testing agents from your browser.

```bash
# Start the web UI server
adk web my_agent
```

- By default, the development server runs on `http://127.0.0.1:8000`.
- Open your browser and navigate to the address shown by the terminal.
- Select your agent and start chatting.

![ADK Web UI](adk_web.jpg)

> **Development note:** The ADK web interface is intended for local development and testing. It should not be treated as a production deployment.

---

## 7. Customising the Agent with Dynamic Context

So far, your agent has a static instruction. In this section, you will provide the agent with dynamically generated information from your Python application.

For this example, you will generate the current system time and inject that value into the agent's instructions.

Modify `my_agent/agent.py` to match the following:

```python
from datetime import datetime
from google.adk.agents.llm_agent import Agent

# Get current system time to inject
current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

root_agent = Agent(
    model='gemini-3.5-flash',
    name='root_agent',
    description='A helpful assistant for user questions.',
    instruction=f'Answer user questions to the best of your knowledge. The current time is {current_time}. Always keep answers concise.',
)
```

Restart the agent (`adk run my_agent` or `adk web my_agent`) and ask: *"What time is it?"* to verify that the agent can use the time value injected into its instructions.

> **Important:** The agent is not independently retrieving the current time. Your Python application calculates the time when the agent is initialised and includes that value in the agent's instruction.

This simple example demonstrates an important concept: your Python application can provide additional context to an agent dynamically.

---

## 8. Conclusion & Next Steps

Congratulations! You have successfully created, configured, and run your first Google ADK agent.

You have learned how to:

- Set up a Python environment for Google ADK.
- Scaffold a new agent using the ADK CLI.
- Configure Gemini API access.
- Explore the structure of an ADK project.
- Run an agent from the terminal.
- Interact with an agent through the local web interface.
- Provide dynamic context to an agent using Python.

### Where to Go Next

Now that you have a working agent, you can start exploring more advanced ADK capabilities.

1. **Add Tools:** Give your agent additional capabilities by connecting Python functions and other supported tools.
2. **Multi-Agent Systems:** Build workflows involving multiple specialised agents and explore agent-to-agent communication.
3. **MCP:** Connect your agent to external tools and services using the Model Context Protocol.
4. **Evaluation:** Test and evaluate your agent systematically to understand its reliability and performance.
5. **Deploy:** Move your agent from local development to production using Google Cloud deployment options such as Agent Runtime, Cloud Run, or GKE.

For more advanced tutorials and reference material, continue with the [official Google ADK documentation](https://google.github.io/adk-docs/) and the [ADK Python repository](https://github.com/google/adk-python).

# Google Agent Development Kit (ADK) Codelab

Welcome to the Google Agent Development Kit (ADK) Codelab! In this tutorial, you will learn how to build, run, and interact with AI agents using the Google ADK Python framework.

## Table of Contents
1. [Introduction to Google ADK](#1-introduction-to-google-adk)
2. [Prerequisites & Environment Setup](#2-prerequisites--environment-setup)
3. [Scaffolding a New Agent](#3-scaffolding-a-new-agent)
4. [Exploring the Agent Structure](#4-exploring-the-agent-structure)
5. [Configuring API Credentials](#5-configuring-api-credentials)
6. [Running Your Agent](#6-running-your-agent)
   - [Method A: Terminal CLI](#method-a-terminal-cli)
   - [Method B: Interactive Web UI](#method-b-interactive-web-ui)
7. [Enhancing the Agent (Adding Custom Rules/Logic)](#7-enhancing-the-agent-adding-custom-ruleslogic)
8. [Conclusion & Next Steps](#8-conclusion--next-steps)

---

## 1. Introduction to Google ADK
The **Google Agent Development Kit (ADK)** is an open-source, code-first Python framework designed for building, testing, and deploying reliable AI agents at scale. 

Key advantages of ADK:
- **Simplicity:** Easily define agent instructions, tools, and models in Python.
- **Interoperability:** Seamlessly switch between Google AI (Gemini Developer API) and Vertex AI backends.
- **Built-in tools:** Ships with a built-in interactive CLI runner and a FastAPI web UI.

---

## 2. Prerequisites & Environment Setup

ADK requires **Python 3.10+**. Follow these steps to prepare your directory structure:

### Step A: Initialize a Virtual Environment
Using a virtual environment prevents package name collisions (like the common mistake of installing `adk` instead of `google-adk`).

```bash
# Create the virtual environment
python -m venv .venv

# Activate the virtual environment
# On Windows (Command Prompt):
.venv\Scripts\activate.bat

# On Windows (PowerShell):
.\.venv\Scripts\Activate.ps1

# On macOS/Linux:
source .venv/bin/activate
```

### Step B: Install the Correct SDK Package
Run the following command to download and install the official Agent Development Kit package:

```bash
pip install google-adk
```

> [!WARNING]
> Do NOT run `pip install adk`. This installs an unrelated RPC development library that conflicts with the Google ADK commands.

---

## 3. Scaffolding a New Agent

To automatically generate the boilerplate files for your agent, run:

```bash
adk create my_agent
```

During this command, the CLI prompts you:
1. **Choose a model**: Select `1` for `gemini-3.5-flash`.
2. **Choose a backend**: Select `1` for `Google AI`.
3. **Google API Key**: Enter your key (or enter a placeholder and modify it later).

> [!NOTE]
> On Windows, if the command crashes at the very end with a `UnicodeEncodeError`, don't worry. This is a display formatting bug caused by console emojis. The files are still successfully created under the `my_agent` folder.

---

## 4. Exploring the Agent Structure

Navigate to your newly created directory to find the following files:

```
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

---

## 5. Configuring API Credentials

Open `my_agent/.env`. It should look like this:

```ini
GOOGLE_GENAI_USE_ENTERPRISE=0
GOOGLE_API_KEY=YOUR_GEMINI_API_KEY
```

Replace `YOUR_GEMINI_API_KEY` with a valid Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey).

---

## 6. Running Your Agent

Make sure your virtual environment is active, and then choose one of the options below to interact with your agent.

### Method A: Terminal CLI
You can test your agent directly in the console using:

```bash
# Set console encoding to UTF-8 on Windows to avoid emoji errors
export PYTHONIOENCODING="utf-8"

# Run in interactive CLI mode
adk run my_agent
```

You will see an interactive prompt where you can chat directly with your agent.

---

### Method B: Interactive Web UI
ADK has a built-in FastAPI web server that provides a clean chat UI.

```bash
# Start the web UI server
adk web my_agent
```

- By default, the server runs on `http://127.0.0.1:8000`.
- Open your browser, navigate to the URL, and start chatting with the agent!

---

## 7. Enhancing the Agent (Adding Custom Logic)

You can modify the agent's instructions or logic. For instance, to give the agent awareness of time, import the `datetime` library and append a rule to its instructions.

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

Restart the agent (`adk run my_agent` or `adk web my_agent`) and ask: *"What time is it?"* to confirm it utilizes your custom time injection.

---

## 8. Conclusion & Next Steps
Congratulations! You have successfully configured and run your first Google ADK agent.

To continue building:
1. **Add Tools:** Integrate functions for the agent to call by using python functions.
2. **Multi-Agent Systems:** Connect multiple agents together by adding sub-agents.
3. **Deploy:** Package your agent to run in production using `adk deploy`.

Refer to the official documentation on the [Google ADK GitHub Repository](https://github.com/google/adk) for more advanced tutorials.

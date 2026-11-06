# Memory Bot

A chatbot that runs fully on your own computer using a local LLM (via [Ollama](https://ollama.com)) and remembers the conversation using LangChain memory. It has a clean chat-app interface built with Streamlit, plus a simple terminal version.

---
https://memory-bot-gjggvz6td5o3ydyte6hh8i.streamlit.app/

## Features

- **100% local**: no API key, no internet needed after setup
- **Conversation memory**: remembers what you said earlier in the chat
- **New chat button**: start a fresh conversation any time
- **Chat history sidebar**: switch between or delete old chats
- **Saved chats**: stored in `chats.json`, so they survive restarts
- **Model picker**: choose any model you have installed in Ollama
- **Streaming replies**: text appears as the model writes it
- **Terminal version**: `chatbot.py` for a quick command-line chat

---

## Tech Stack

| Part | Tool |
|------|------|
| Language | Python 3.10+ |
| Local LLM runtime | Ollama (`llama3.2`, `gemma3:1b`, etc.) |
| LLM framework | LangChain (`langchain-core`, `langchain-ollama`) |
| Web UI | Streamlit |

---

## Project Structure

```
memory-bot/
├── .streamlit/
│   └── config.toml      # dark theme settings
├── app.py               # Streamlit web UI (new chat, history, model picker)
├── chatbot.py           # terminal chatbot (LangChain memory)
├── requirements.txt     # Python dependencies
├── chats.json           # auto-created: saved chats (do not commit)
└── README.md
```

---

## Getting Started

### 1. Install Ollama and download a model

Download Ollama from <https://ollama.com>, then run:

```bash
ollama pull llama3.2
```

> Low on RAM? Use a smaller model: `ollama pull llama3.2:1b` or `ollama pull gemma3:1b`.

### 2. Clone the project and create a virtual environment

```bash
git clone https://github.com/<your-username>/memory-bot.git
cd memory-bot

python -m venv .venv

# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run it

**Web app (recommended):**

```bash
streamlit run app.py
```

Then open <http://localhost:8501> in your browser.

**Terminal version:**

```bash
python chatbot.py
```

Commands: `/clear` resets memory, `/quit` exits.

---

## Try the Memory

```
You: My name is Rhema
You: What's my name?
Bot: Your name is Rhema.
```

Click **New chat** and ask again. The new chat starts with a clean memory.

---

## How Memory Works

- **Terminal version (`chatbot.py`)**: uses LangChain's `RunnableWithMessageHistory` with a custom `WindowedHistory` class that keeps only the last N messages, so the prompt never outgrows the model's context window.
- **Web version (`app.py`)**: each chat stores its own message list; the last `MAX_MESSAGES` messages are sent to the model with every question. Chats are saved to `chats.json`.

You can change the memory size by editing `MAX_MESSAGES` at the top of either file.

---

## Configuration

| Setting | Where | Default |
|---------|-------|---------|
| Default model | `DEFAULT_MODEL` in `app.py`, `MODEL_NAME` in `chatbot.py` | `llama3.2` |
| Memory window | `MAX_MESSAGES` | 10 to 12 messages |
| System prompt | `SYSTEM_PROMPT` in `app.py` | friendly, concise assistant |
| Theme | `.streamlit/config.toml` | dark |

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `model 'llama3.2' not found (404)` | Run `ollama pull llama3.2` and wait for `success` |
| `Connection refused` | Ollama is not running. Start it with `ollama serve` |
| `ModuleNotFoundError` | Activate the venv and run `pip install -r requirements.txt` |
| VS Code: `Import "streamlit" could not be resolved` | Press `Ctrl+Shift+P`, choose **Python: Select Interpreter**, pick the `.venv` one |
| `requirements.txt` not found | Make sure your terminal is inside the project folder (`dir` or `ls` to check) |
| Slow replies | Use a smaller model such as `llama3.2:1b` or `gemma3:1b` |

---

## Deployment Note

Local Ollama models cannot run on Streamlit Community Cloud (no Ollama or GPU on the server). To deploy publicly, either:

1. Swap `ChatOllama` for a hosted model (for example Google Gemini via `langchain-google-genai`) and add your API key in Streamlit Secrets, or
2. Share your local app temporarily with a tunnel (for example `cloudflared tunnel --url http://localhost:8501`), or
3. Host it on your own server with Ollama installed.

---


## Author

Made by **<Your Name>**, CS student at SDNB Vaishnav College, Chennai.

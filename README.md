# LangGraph Chatbot UI

A chatbot built with **LangGraph** and **Google Gemini**, with a **Streamlit** chat interface that streams responses and supports multiple conversations.

## Demo

<!-- Replace the line below with the video link GitHub gives you (see "Adding the demo video") -->
VIDEO_LINK_HERE

## Features

- **Streaming responses**: the reply appears token by token, like ChatGPT
- **Multiple conversations**: start a new chat from the sidebar and switch between past chats
- **Conversation memory**: each chat keeps its own history through LangGraph's checkpointer, keyed by a unique `thread_id`

## How it works

```
User ──> Streamlit UI ──> LangGraph graph ──> Gemini
              ^            (START -> chat_node -> END)
              |                     |
              +──── streamed reply ─+
```

| File | What it does |
|------|--------------|
| `langgraph_backend.py` | Defines the graph: one `chat_node` that sends the message history to Gemini. Uses `InMemorySaver` so each thread remembers its messages. |
| `streamlit_frontend.py` | Basic version: a single chat with streaming. |
| `streamlit_frontend_threading.py` | Full version: sidebar with **New Chat** and a list of past conversations. |

## Setup

**1. Clone the repo**

```bash
git clone https://github.com/aayushbishtt/langgraph-chatbot-ui.git
cd langgraph-chatbot-ui
```

**2. Create a virtual environment and install the dependencies**

```bash
python3 -m venv venv
source venv/bin/activate        # on Windows: venv\Scripts\activate
pip install -r requirements.txt
```

**3. Add your Gemini API key**

Get a key from [Google AI Studio](https://aistudio.google.com/apikey), then create a `.env` file in the project folder:

```
GOOGLE_API_KEY=your_api_key_here
```

**4. Run the app**

```bash
streamlit run streamlit_frontend_threading.py
```

The app opens at http://localhost:8501.

## Limitations

- Conversations are kept **in memory** only, so they're lost when the app restarts.
- Past chats in the sidebar are labelled with their thread ID instead of a title.

## Tech stack

- [LangGraph](https://github.com/langchain-ai/langgraph)
- [LangChain Google GenAI](https://python.langchain.com/docs/integrations/chat/google_generative_ai/)
- [Streamlit](https://streamlit.io/)

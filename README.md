# Multi Agent AI Research Assistant

Multi Agent AI Research Assistant is a small multi-agent research pipeline with a Streamlit demo and a plain Python CLI. Specialized components collaborate to turn web research into a structured report:

1. **Search agent** finds relevant sources with Tavily.
2. **Reader agent** scrapes and cleans source pages.
3. **Writer chain** creates a structured research report.
4. **Critic chain** reviews and scores the report.

The project is intended for developers experimenting with LangChain-style agents, web search, scraping, and multi-step LLM workflows.

## Stack

- Python
- Streamlit for the interactive UI
- LangChain with Gemini API integration
- Tavily for web search
- Requests and BeautifulSoup for page scraping

## Project Structure

```text
.
├── app.py             # Streamlit interface
├── agents.py          # Agent and chain construction
├── pipeline.py        # CLI pipeline runner
├── tools.py           # Tavily search and URL scraping tools
├── requirements.txt   # Python dependencies
└── .env               # Local API keys; do not commit
```

## How It Works

`tools.py` provides two tools:

- `web_search`: searches the web through `TavilyClient` and returns titles, URLs, and snippets.
- `scrape_url`: downloads a page with `requests`, removes non-content elements, and returns cleaned text.

`agents.py` composes these tools with a Gemini model to build the search and reader agents, plus the writer and critic chains. Both `app.py` and `pipeline.py` use the same search, reader, writer, and critic flow.

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/AkarshVyas/Multi-agent-research-system.git
cd Multi-agent-research-system
```

### 2. Create and activate a virtual environment

macOS/Linux:

```bash
python -m venv .venv
source .venv/bin/activate
```

Windows:

```powershell
python -m venv .venv
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure API keys

Create a `.env` file in the project root:

```env
TAVILY_API_KEY=your_tavily_key_here
GEMINI_API_KEY=your_gemini_key_here
```

`TAVILY_API_KEY` is required for web search. `GEMINI_API_KEY` is required for Gemini model access.

## Run the Application

For the interactive Streamlit interface:

```bash
streamlit run app.py
```

The UI accepts a research topic and displays the search results, scraped content, generated report, and critic feedback. It also provides a download option for the final report.

To run the command-line pipeline:

```bash
python pipeline.py
```

The CLI prompts for a research topic and runs the same four-step workflow in the terminal.

## Configuration Notes

- The application uses the Gemini API for language-model requests.
- Configure the selected Gemini model in `agents.py` if you need to change the model or generation settings.
- `scrape_url` currently returns approximately the first 3,000 characters of cleaned page text.
- Scraping can be improved for production use with caching, rate limiting, retries, and stricter content extraction.
- Keep API keys in `.env` or environment variables. Never commit them to Git.

## License

This project is licensed under the MIT License.

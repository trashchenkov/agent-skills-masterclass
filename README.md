# Один агент, любая задача: реализуем скиллы на LangChain с нуля

Материалы мастер-класса Сергея Тращенкова (GigaChain / Сбер).

## Что внутри

```
agent-skills-masterclass/
├── masterclass.ipynb   # основной ноутбук
├── skills/             # папка для скиллов (заполняется в процессе)
└── slides/             # теоретические слайды (Slidev)
```

## Быстрый старт

Нужны Python 3.12+ и [`uv`](https://docs.astral.sh/uv/).

```bash
git clone https://github.com/trashchenkov/agent-skills-masterclass
cd agent-skills-masterclass

uv init . --python 3.12 --no-workspace
uv add jupyter \
    langchain langchain-core langchain-ollama langchain-openai langchain-openrouter \
    deepagents langgraph \
    arize-phoenix openinference-instrumentation-langchain \
    opentelemetry-sdk opentelemetry-exporter-otlp \
    python-docx openpyxl python-dotenv

echo "OPENROUTER_API_KEY=sk-or-v1-..." >> .env

uv run jupyter notebook masterclass.ipynb
```

## Структура ноутбука

**Setup** — окружение, LLM, Phoenix, skill-creator

**Блок 1 — docx** — изучаем референсный скилл Anthropic → создаём свой `docx`-скилл → демо

**Блок 2 — xlsx** — то же для Excel, плюс формулы

**Блок 3 — LangChain v1** — создаём документ с требованиями → LangChain-агент читает его и генерирует `bug-report`-скилл → тест на реальном баге

## Модели

По умолчанию используется GLM-5.1 через OpenRouter. В ноутбуке закомментированы альтернативы:
- OpenRouter free (например, `meta-llama/llama-3.3-70b-instruct:free`)
- Ollama Cloud
- OpenAI

## Наблюдаемость

После запуска Phoenix откройте [http://localhost:6006](http://localhost:6006) — там трейсы всех агентов.
Инструментирование через `openinference-instrumentation-langchain` работает одинаково для Deep Agents и LangChain v1.

## Слайды

```bash
cd slides
npx slidev slides.md
```

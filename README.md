# Один агент, любая задача: скиллы в Deep Agents

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
git clone -b codex/deep-agents-only-materials https://github.com/trashchenkov/agent-skills-masterclass
cd agent-skills-masterclass

uv init . --python 3.12 --no-workspace
uv add jupyter \
    langchain langchain-core langchain-ollama langchain-openai langchain-openrouter \
    deepagents langgraph \
    arize-phoenix openinference-instrumentation-langchain \
    opentelemetry-sdk opentelemetry-exporter-otlp \
    openpyxl python-dotenv

echo "OPENROUTER_API_KEY=sk-or-v1-..." >> .env

uv run jupyter notebook masterclass.ipynb
```

## Структура ноутбука

**Setup** — окружение, LLM, Phoenix, клонирование `anthropics/skills`, подключение `skill-creator`.

**Практика — xlsx-skill** — изучаем proprietary `xlsx` из Anthropic как reference, создаём собственный `skills/xlsx/` через `skill-creator`, затем в новой сессии генерируем Excel-отчёт с формулами `SUM` и `AVERAGE`.

**Выводы и материалы** — что даёт skills-подход, почему нужна проверка результата и куда смотреть дальше.

## Модели

По умолчанию используется GLM-5.1 через OpenRouter. В ноутбуке закомментированы альтернативы:
- OpenRouter free
- Ollama Cloud
- OpenAI

## Наблюдаемость

После запуска Phoenix откройте [http://localhost:6006](http://localhost:6006) — там видны трейсы агента: вызовы модели, инструменты, чтение файлов, расход токенов и время выполнения.

## Слайды

```bash
cd slides
npx slidev slides.md
```

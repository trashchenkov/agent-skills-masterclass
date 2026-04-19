---
theme: default
title: "Один агент, любая задача: реализуем скиллы на LangChain с нуля"
info: |
  Теоретическая презентация к мастер-классу
  «Агенты со скиллами: LangChain v1 и Deep Agents».
class: text-left
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
mdc: true
css: unocss
aspectRatio: 16/9
canvasWidth: 1280
layout: cover
---

# Один агент, любая задача

## Реализуем скиллы на LangChain с нуля

<div class="mt-10">
Сергей Тращенков<br>
<span class="opacity-70">Python-разработчик GigaChain, Сбер</span>
</div>

<!--
Цель: за 7–10 минут дать рамку перед ноутбуком — пока участники скачивают репозиторий и настраивают окружение. QR-слот зарезервирован под ссылку на репозиторий.
-->

---
layout: default
---

# Немного о себе

<div class="grid grid-cols-2 gap-12 mt-6">
<div>

**GigaChain / Сбер**

Делаем open source для агентов:

- SDK под GigaChat API (`gigachat`)
- интеграционный пакет для LangChain (`langchain-gigachat`)
- прокси-сервер `GPT2GIGA`
- универсальный ИИ-агент `GigaAgent`

</div>
<div>

**Сергей Тращенков**

ИИ-энтузиаст, Python-разработчик

</div>
</div>

---
layout: default
---

# Что разберём

<v-clicks>

1. Почему умный агент всё равно может ошибаться в специфических задачах
2. Что такое harness и почему он важнее, чем выбор модели
3. Что дают skills и как они живут внутри harness
4. Чем skills отличаются от memory
5. Как скачать материалы и запустить ноутбук мастер-класса

</v-clicks>

<!--
Коротко: сначала проблема, потом архитектура, потом практика.
-->

---
layout: section
---

# Главная проблема

## Агент умный, но не эксперт в ваших процессах

---
layout: default
---

# «Гениальный дилетант»

LLM может отлично рассуждать, писать код и объяснять концепции.

Но когда задача требует знания конкретного процесса, агент часто начинает импровизировать:

<v-clicks>

- оформляет документ «примерно похоже», но не по шаблону;
- обрабатывает баг-репорт не по вашему процессу;
- выбирает не тот формат результата;
- забывает проверить важные ограничения.

</v-clicks>

<v-click>

<div class="mt-8 text-2xl font-bold">
Проблема не в интеллекте. Проблема в отсутствии знаний о ваших процессах.
</div>

</v-click>

<!--
Аналогия: математический гений, который никогда не работал бухгалтером. Он умный, но не знает ваших процессов.
-->

---
layout: default
---

# Три пути к специализации

| Подход | Что происходит | Сложность |
|---|---|---|
| Один агент знает всё | Огромный system prompt | Перегруз контекста, путаница |
| Много агентов-специалистов | Отдельный агент под каждую задачу | Сложный деплой и оркестрация |
| Один агент + skills | Компетенция загружается по запросу | Нужно проектировать хорошие скиллы |

<div class="mt-8 text-xl">
В мастер-классе используем третий путь: универсальный агент + библиотека переиспользуемых процессов.
</div>

---
layout: default
---

# Аналогия: процессор, ОС, приложения

| В компьютере | В агентной системе | Что даёт |
|---|---|---|
| **Процессор** | **LLM** | Интеллект: рассуждение, генерация, понимание |
| **Операционная система** | **Harness** | Среда: инструменты, память, контроль, безопасность |
| **Приложения** | **Skills** | Экспертиза для конкретных задач |

<div class="mt-8 text-xl">
Скиллы — это не гонка за «лучшим процессором». Это слой программного обеспечения поверх модели.
</div>

<!--
Важная точка: многие команды тратят время на выбор модели, хотя проблема — в отсутствии harness и skills.
-->

---
layout: default
---

# Harness: всё, кроме модели

<div class="grid grid-cols-2 gap-8 mt-4">
<div>

```text
Harness
├─ tools & MCP
├─ context management
│  ├─ memory      ← всегда в промпте
│  ├─ skills      ← по запросу
│  └─ compaction
├─ infrastructure
├─ planning
├─ safety & HITL
└─ observability
```

</div>
<div class="flex flex-col justify-center">

<div class="text-3xl font-mono font-bold mb-6">
Agent = Model + Harness
</div>

**2025 год был про агентный цикл.**  
**2026-й — про harness.**

Anthropic, OpenAI, LangChain, Manus и научное сообщество работают над созданием эффективных обвязок вокруг модели.

<div class="mt-4 text-sm italic opacity-70">
«Anytime you find an agent makes a mistake, you take the time to engineer a solution such that the agent never makes that mistake again.»<br>
— Mitchell Hashimoto, My AI Adoption Journey
</div>

</div>
</div>

<!--
Skills живут внутри context management — они загружаются по запросу в контекстное окно. Memory — фиксированный набор файлов, всегда в промпте. Compaction — сжатие при переполнении окна.
-->

---
layout: default
---

# LangChain vs Deep Agents

| | LangChain v1 | Deep Agents |
|---|---|---|
| Что это | ReAct loop | Готовый harness |
| Planning | — | `write_todos` из коробки |
| Filesystem | — | `ls`, `read`, `write`, `edit` из коробки |
| Subagents | — | `task` из коробки |
| Memory | checkpointer явно | `memory=[...]` — файлы в system prompt |
| Skills | нет нативной поддержки | `skills=[...]` |

<div class="mt-6 text-lg">
В ноутбуке разберём оба подхода — чтобы понять, что именно делает Deep Agents под капотом.
</div>

---
layout: two-cols
---

# create_agent()

```python
from langchain.agents.middleware import ToolRetryMiddleware

agent = create_agent(
    model=llm,
    tools=[search, run_code],
    checkpointer=InMemorySaver(),
    middleware=[
        ToolRetryMiddleware(on_failure="continue"),
    ],
)
```

::right::

# create_deep_agent()

```python
from langchain.agents.middleware import ToolRetryMiddleware

agent = create_deep_agent(
    model=llm,
    memory=["/memories/AGENTS.md"],
    skills=["./skills/"],
    tools=[search],
    backend=CompositeBackend(
        default=StateBackend(),
        routes={
            "/memories/": StoreBackend(...),
        },
    ),
    middleware=[
        ToolRetryMiddleware(on_failure="continue"),
    ],
)
```

<!--
model, tools, middleware — одинаковые у обоих. Разница — memory, skills, backend. Deep Agents расширяет, а не заменяет LangChain.
-->

---
layout: default
---

# Что такое skill

Skill — это переносимая папка с инструкциями и ресурсами:

```text
my-skill/
├── SKILL.md        # name + description + инструкции
├── scripts/        # Python, Bash, JS — исполняемые помощники
├── references/     # длинные справочники и документация
└── assets/         # шаблоны, картинки, статические файлы
```

<div class="mt-6 text-xl">
Ключевая идея: агент не обязан помнить всё заранее. Он должен уметь найти и прочитать нужную инструкцию в момент выполнения задачи.
</div>

<!--
Агент видит компактный индекс name: description. Если задача совпала — читает полный SKILL.md. Это прогрессивное раскрытие контекста.
-->

---
layout: default
---

# Skills vs. Memory

| | Skills | Memory |
|---|---|---|
| Что содержит | Процедуры для конкретных задач | Стиль, соглашения, контекст проекта |
| Когда загружается | По запросу, когда задача совпала | Всегда, автоматически в промпт |
| Хранится | Папки на диске, агент читает сам | Фиксированный набор файлов |
| Пример | «Как создать Word-документ по шаблону» | «Пиши на русском, используй snake_case» |

<div class="mt-8 text-2xl font-bold">
Правило: вечные соглашения — в memory, специализированные задачи — в skills.
</div>

---
layout: section
---

# Материалы мастер-класса

## Последние шаги: скачать, установить, запустить

---
layout: default
---

# 1. Скачать репозиторий

Отсканируйте QR-код на слайде или откройте ссылку на репозиторий материалов.

```bash
git clone https://github.com/trashchenkov/agent-skills-masterclass
cd agent-skills-masterclass
```

Если архив скачан вручную:

```bash
unzip masterclass.zip
cd agent-skills-masterclass
```


---
layout: default
---

# 2. Установить окружение

Нужны Python 3.12+ и `uv`.

```bash
uv init masterclass --python 3.12 --no-workspace
cd masterclass
```

Установите зависимости:

```bash
uv add jupyter \
        langchain langchain-core langchain-ollama langchain-openrouter \
        deepagents langgraph \
        arize-phoenix openinference-instrumentation-langchain \
        opentelemetry-sdk opentelemetry-exporter-otlp \
        python-docx openpyxl python-dotenv
```

---
layout: default
---

# 3. Настроить ключи и модель

Для OpenRouter-варианта добавьте ключ в `.env`:

```bash
echo "OPENROUTER_API_KEY=sk-or-v1-..." >> .env
```

В ноутбуке модель задаётся один раз в конфигурационной ячейке:

```python
from langchain_openrouter import ChatOpenRouter

llm = ChatOpenRouter(model="z-ai/glm-5.1")
```

Все агенты дальше используют только переменную `llm`.

---
layout: default
---

# 4. Запустить блокнот

Запускайте Jupyter только через `uv run`, чтобы он видел зависимости проекта:

```bash
uv run jupyter notebook masterclass.ipynb
```

После открытия выполняйте ячейки сверху вниз и смотрите трейсы в Phoenix.

<div class="mt-8 text-xl font-bold">
Не запускайте `jupyter notebook` без `uv run` из системного окружения.
</div>

---
layout: default
---

# Что строим сегодня

<div class="grid grid-cols-3 gap-8 mt-6">
<div class="border border-slate-200 rounded-xl p-5">

**Блок 1 — docx**

Изучаем референсный скилл Anthropic → пишем свой `docx`-скилл с нуля → создаём документ

</div>
<div class="border border-slate-200 rounded-xl p-5">

**Блок 2 — xlsx**

То же для Excel, плюс скилл должен уметь работать с формулами

</div>
<div class="border border-slate-200 rounded-xl p-5">

**Блок 3 — LangChain v1**

Создаём требования к баг-репортам → агент читает документ и генерирует скилл → тест на реальном баге

</div>
</div>

<div class="mt-8 text-lg">
Один агент, одна папка со скиллами — и три разных специализации.
</div>

---
layout: end
---

# Готово

## Переходим к ноутбуку

<div class="mt-8 text-xl opacity-80">
QR-код ведёт в репозиторий с материалами мастер-класса.
</div>

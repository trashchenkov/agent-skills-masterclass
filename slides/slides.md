---
theme: default
title: "Один агент, любая задача: скиллы в Deep Agents"
info: |
  Презентация к вебинару
  «Один агент, любая задача: скиллы в Deep Agents».
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

## Скиллы в Deep Agents

<div class="mt-10">
Сергей Тращенков<br>
<span class="opacity-70">Python-разработчик GigaChain, Сбер</span>
</div>

<!--
Фокус вебинара: не обзор всего Deep Agents, а skills как практический механизм специализации агента.
-->

---
layout: default
---

# Немного о себе и команде

<div class="grid grid-cols-2 gap-12 mt-6">
<div>

**GigaChain / Сбер**

Делаем open source для агентов:

- SDK под GigaChat API (`gigachat`)
- интеграционный пакет для LangChain (`langchain-gigachat`)
- прокси-сервер `GPT2GIGA`
- универсальный ИИ-агент `GigaAgent`
- бенчмарк harness-подходов ([`harness-bench-fast`](https://github.com/ai-forever/harness-bench-fast))
- интеграция Deep Agents с GigaChat ([`deepagents-gigachat`](https://github.com/ai-forever/deepagents-gigachat))

</div>
<div>

**Сергей Тращенков**

Python-разработчик GigaChain, ИИ-энтузиаст

<div class="mt-6 opacity-80">
Интересуюсь LangChain, LangGraph, Deep Agents и агентными harness-подходами
</div>

</div>
</div>

---
layout: default
---

# План

<div class="grid grid-cols-2 gap-7 mt-8">
<div class="border border-slate-200 rounded-xl p-6">

## 1. От agent loop к harness

Почему одной модели и набора tools недостаточно для длинных специализированных задач.

</div>
<div class="border border-slate-200 rounded-xl p-6">

## 2. Deep Agents

Что это за обвязка: LangGraph, файловая система, память, инструменты, middleware и наблюдаемость.

</div>
<div class="border border-slate-200 rounded-xl p-6 bg-slate-50">

## 3. Skills

Как навык выбирается, как читается `SKILL.md`, где живут helper-скрипты и референсы.

</div>
<div class="border border-slate-200 rounded-xl p-6 bg-blue-50 border-blue-300">

## 4. Практика

Соберём `xlsx-skill` и используем его для Excel-отчёта с формулами.

</div>
</div>


---
layout: section
---

# Проблема

## Как затюнить универсального агента под узкоспециализированные задачи — и не перегрузить контекст инструкциями?


---
layout: default
---

# Почему большой prompt не спасает

<div class="grid grid-cols-3 gap-6 mt-8">
<div class="border border-slate-200 rounded-xl p-5">

## System prompt

Подходит для базового поведения агента.

<div class="mt-4 opacity-70">
Плохо масштабируется, если складывать туда все узкие процедуры и исключения.
</div>

</div>
<div class="border border-slate-200 rounded-xl p-5">

## Tools

Дают агенту действия.

<div class="mt-4 opacity-70">
Но не описывают процесс целиком; а когда tools много, их описания раздувают контекст и усложняют выбор.
</div>

</div>
<div class="border border-slate-200 rounded-xl p-5 bg-slate-50">

## Skills

Дают специализацию по задаче.

<div class="mt-4 opacity-70">
В prompt попадает короткий индекс, а подробности открываются только при необходимости.
</div>

</div>
</div>

---
layout: default
---

# ReAct-агент vs agent harness

<div class="mt-4 text-xl opacity-80">
Harness — это слой вокруг модели, который делает agent loop пригодным для длинных задач: даёт среду, состояние, планирование и правила работы с инструментами.
</div>

| | Обычный ReAct-агент | Agent harness |
|---|---|---|
| Взаимодействие с миром | API-вызовы под конкретную задачу | файловая система, shell/code, внешние tools |
| Память | история текущего диалога | файлы, store, checkpointing между шагами |
| Инструменты | узкий набор tools под сценарий | базовый набор операций + подключаемые tools |
| План и контекст | держатся в сообщениях и легко теряются | выносятся в артефакты: todo, файлы, summary |
| Охват задач | агент под один класс задач | general-purpose агент для разных сценариев |

<div class="mt-8 text-2xl font-bold">
Skills ложатся в эту обвязку как способ специализации: агент видит короткий индекс, а инструкции и helper-код читает из файлов по необходимости.
</div>

---
layout: default
---

# Стек: от langchain-core до Deep Agents

<div class="mt-8 space-y-3 text-xl">
<div class="border rounded-xl p-4 bg-blue-50 border-blue-300">
<strong>Deep Agents</strong> — готовый harness: planning · filesystem · subagents · skills
</div>
<div class="border rounded-xl p-4">
<strong>LangChain</strong> — agents, tools, middleware API
</div>
<div class="border rounded-xl p-4">
<strong>LangGraph</strong> — runtime графа, состояние, checkpointing, streaming
</div>
<div class="border rounded-xl p-4">
<strong>langchain-core</strong> — сообщения, модели, tools, Runnable
</div>
</div>


---
layout: default
---

# Deep Agents = открытая обвязка

<div class="grid grid-cols-2 gap-8 mt-6">
<div>

## Что даёт


- один general-purpose агент вместо отдельного агента под каждую задачу;
- преднастроенные механизмы для длинных сценариев;
- возможность менять компоненты обвязки, не меняя модель;
- поведение ближе к coding-agent системам вроде Claude Code / Codex CLI.


</div>
<div>

## Что можно настраивать


- system prompt;
- tools;
- backend файловой системы;
- memory / store / checkpointer;
- middleware;
- subagents;
- **skills**.


</div>
</div>

---
layout: default
---

# Две формы обвязки

<div class="grid grid-cols-2 gap-8 mt-8">
<div class="border border-slate-200 rounded-xl p-6">

## `deepagents`

Библиотека / SDK.

<div class="mt-4 opacity-80">
Встраиваем агента в свой Python-код, настраиваем модель, tools, backend, skills, memory.
</div>

<div class="mt-6 text-lg font-bold">
Сегодня работаем здесь.
</div>

</div>
<div class="border border-slate-200 rounded-xl p-6">

## `deepagents-code`

Готовый coding-agent в терминале.

<div class="mt-4 opacity-80">
Реализован на JS/TS версии SDK: готовый терминальный coding-agent поверх Deep Agents.
</div>

</div>
</div>

---
layout: default
---

# Внутри агента: ReAct-цикл в графе

<div class="mt-7 grid grid-cols-[1fr_auto_1fr_auto_2.2fr_auto_1fr_auto_1fr] gap-3 items-center text-center">

<div class="rounded-xl border border-slate-300 bg-slate-50 p-4">
  <div class="text-xs uppercase opacity-55">graph node</div>
  <div class="text-lg font-bold mt-1">before_agent</div>
</div>

<div class="text-2xl opacity-50">→</div>

<div class="rounded-xl border border-slate-300 bg-slate-50 p-4">
  <div class="text-xs uppercase opacity-55">graph node</div>
  <div class="text-lg font-bold mt-1">before_model</div>
</div>

<div class="text-2xl opacity-50">→</div>

<div class="rounded-2xl border-2 border-blue-300 bg-blue-50 p-5">
  <div class="text-xs uppercase tracking-wide opacity-60">ReAct loop</div>

  <div class="mt-3 rounded-xl bg-white border border-blue-200 p-4">
    <div class="text-xs opacity-60">wrap_model_call</div>
    <div class="text-2xl font-bold">Model</div>
    <div class="text-sm opacity-70">reasoning / tool call / final</div>
  </div>

  <div class="my-2 text-blue-700 font-bold leading-tight">
    ↓ tool call<br>
    ↑ observation
  </div>

  <div class="rounded-xl bg-white border border-amber-300 p-4">
    <div class="text-xs opacity-60">wrap_tool_call</div>
    <div class="text-2xl font-bold">Tools</div>
    <div class="text-sm opacity-70">execute → result</div>
  </div>
</div>

<div class="text-2xl opacity-50">→</div>

<div class="rounded-xl border border-slate-300 bg-slate-50 p-4">
  <div class="text-xs uppercase opacity-55">graph node</div>
  <div class="text-lg font-bold mt-1">after_model</div>
</div>

<div class="text-2xl opacity-50">→</div>

<div class="rounded-xl border border-slate-300 bg-slate-50 p-4">
  <div class="text-xs uppercase opacity-55">graph node</div>
  <div class="text-lg font-bold mt-1">after_agent</div>
</div>

</div>

<div class="grid grid-cols-2 gap-8 mt-8">
<div class="border border-slate-200 rounded-xl p-5">

## `before_*` / `after_*`

Отдельные ноды LangGraph: вставляются в поток выполнения агента.

</div>
<div class="border border-slate-200 rounded-xl p-5">

## `wrap_model_call` / `wrap_tool_call`

Обёртки вызова: перехватывают model-call или tool-call внутри ReAct-цикла.

</div>
</div>

---
layout: default
---

# Skills: специализация по требованию

<div class="text-2xl leading-relaxed mt-5">
Skill — это навык в файловой системе агента: короткое описание видно сразу, подробности открываются только после выбора.
</div>

<div class="grid grid-cols-2 gap-8 mt-8">
<div class="border border-slate-200 rounded-xl p-6 bg-slate-50">

## До выбора

<div class="mt-4 text-xl">
Агент видит только короткий индекс:
</div>

<div class="mt-5 rounded-xl bg-white border border-slate-200 p-4 text-center text-xl font-bold">
name + description
</div>

<div class="mt-4 opacity-75">
Этого достаточно, чтобы понять, подходит ли навык к задаче.
</div>

</div>
<div class="border border-blue-300 rounded-xl p-6 bg-blue-50">

## После выбора

<div class="mt-4 text-xl">
Агент открывает детали навыка:
</div>

<div class="mt-5 grid grid-cols-2 gap-3 text-center font-bold">
  <div class="rounded-xl bg-white border border-blue-200 p-4">SKILL.md</div>
  <div class="rounded-xl bg-white border border-blue-200 p-4">scripts</div>
  <div class="rounded-xl bg-white border border-blue-200 p-4">references</div>
  <div class="rounded-xl bg-white border border-blue-200 p-4">examples</div>
</div>

</div>
</div>

<div class="mt-8 text-center text-xl">
<span class="border rounded-xl px-5 py-3 bg-slate-50">задача</span>
<span class="mx-2">→</span>
<span class="border rounded-xl px-5 py-3 bg-slate-50">выбор skill</span>
<span class="mx-2">→</span>
<span class="border rounded-xl px-5 py-3 bg-blue-50 border-blue-300">чтение инструкции</span>
<span class="mx-2">→</span>
<span class="border rounded-xl px-5 py-3 bg-slate-50">выполнение</span>
<span class="mx-2">→</span>
<span class="border rounded-xl px-5 py-3 bg-slate-50">проверка</span>
</div>

---
layout: default
---

# Из чего состоит skill

<div class="grid grid-cols-3 gap-6 mt-8">
<div class="border rounded-xl p-6 bg-blue-50 border-blue-300">

## `SKILL.md`

- когда использовать навык;
- какой порядок действий;
- какие ограничения помнить;
- как понять, что результат готов.

</div>
<div class="border rounded-xl p-6">

## `scripts/`

- генерация файлов;
- парсинг данных;
- вызов библиотек;
- валидация результата.

</div>
<div class="border rounded-xl p-6">

## `references/`

- шаблоны;
- примеры;
- правила оформления;
- дополнительные материалы.

</div>
</div>

<div class="mt-8 text-2xl font-bold">
Хороший skill переносит повторяемую механику из рассуждения модели в инструкцию и код.
</div>

---
layout: default
---

# Что внутри `SKILL.md`

```md
---
name: xlsx
description: Когда выбирать этот навык
---

# Назначение
Что агент должен уметь делать.

# Порядок работы
1. Что прочитать или уточнить.
2. Какие действия выполнить.
3. Какие scripts использовать.

# Ограничения
Поддерживаемые форматы, запреты, важные договорённости.

# Проверка результата
Как агент понимает, что задача выполнена корректно.
```

<div class="mt-5 text-lg opacity-80">
`description` помогает выбрать навык, тело `SKILL.md` ведёт агента по процессу после выбора.
</div>

---
layout: default
---

# Skills, tools и memory

| | Tools | Skills | Memory |
|---|---|---|---|
| Отвечают на вопрос | Что агент может вызвать? | Как выполнить специализированную задачу? | Что агент должен помнить? |
| Что внутри | Функция/API и её описание | Инструкция, scripts, references | Факты, предпочтения, правила проекта |
| Когда полезно | Нужно действие | Нужен процесс | Нужен устойчивый контекст |
| Пример | `write_file`, `execute`, `read_csv` | «Собрать Excel-отчёт с формулами» | «В этой команде отчёт называется sales_report» |

<div class="mt-8 text-2xl font-bold">
Tools дают действия, memory хранит контекст, skills задают процедуру.
</div>

---
layout: section
---

# Практика

## Реализуем xlsx-skill и делаем Excel-отчёт с формулами

---
layout: default
---

# Почему не берём готовый `xlsx-skill`

<div class="text-xl opacity-80 mt-4">
Источник: репозиторий <code>anthropics/skills</code>. Берём его как reference, потому что Anthropic фактически сформулировали pattern skills для агентов.
</div>

<div class="grid grid-cols-2 gap-8 mt-7">
<div class="border border-green-300 rounded-xl p-6 bg-green-50">

## `skill-creator`

Лицензия: **Apache 2.0**.

<div class="mt-4 opacity-80">
Его можно подключить к Deep Agents и использовать как инструмент для создания своих навыков.
</div>

</div>
<div class="border border-amber-300 rounded-xl p-6 bg-amber-50">

## `xlsx`, `docx`, `pdf`, `pptx`

Лицензия: **proprietary**.

<div class="mt-4 opacity-80">
Их нельзя просто вынести из сервисов Anthropic, скопировать, создать производную версию или распространять.
</div>

</div>
</div>

<div class="mt-8 text-2xl font-bold">
Поэтому процесс такой: открытый `skill-creator` используем, закрытый `xlsx` изучаем как идею, а свой навык пишем заново.
</div>

<div class="absolute bottom-4 left-1/2 -translate-x-1/2 flex flex-col items-center gap-1">
  <img :src="'/qr-anthropic-skills.svg'" class="w-22 h-22 bg-white rounded-lg p-1 shadow" alt="anthropics/skills" />
  <div class="text-[10px] uppercase tracking-wide opacity-70 bg-white/80 rounded-full px-3 py-0.5 shadow-sm">anthropics/skills</div>
</div>

---
layout: default
---

# Что будем собирать: свой `xlsx-skill`

<div class="text-xl opacity-80 mt-4">
Не копия Anthropic skill, а новый навык для Deep Agents: написанный своими словами и со своими helper-скриптами.
</div>

```text
skills/
└── xlsx/
    ├── SKILL.md
    └── scripts/
        ├── create_workbook.py
        └── validate_workbook.py
```

<div class="mt-6 grid grid-cols-2 gap-8">
<div>

## Что берём из reference

Идею устройства навыка: когда он триггерится, как разбивать работу на инструкцию и scripts, какие проверки нужны.

</div>
<div>

## Что пишем сами

`SKILL.md` и Python-код на `openpyxl`: таблица, формулы `SUM` / `AVERAGE`, проверка результата.

</div>
</div>

---
layout: default
---

# Workflow в ноутбуке

1. Клонируем `anthropics/skills` во временную папку
2. Копируем в проект только `skill-creator` — он Apache 2.0
3. Просим агента изучить proprietary `xlsx` как reference: `SKILL.md` и scripts
4. Через `skill-creator` создаём свой `skills/xlsx/` с нуля
5. Проверяем навык на `output/test.xlsx` с формулой `SUM`

<div class="mt-8 text-xl opacity-80">
Практика проходит полный цикл: изучаем reference, создаём свой skill, применяем его и проверяем результат.
</div>

---
layout: default
---

# Задача для xlsx-skill

<div class="mt-6 text-2xl">
Сгенерировать Excel-отчёт по продажам.
</div>

<div class="grid grid-cols-2 gap-8 mt-8">
<div>

## Данные

- регионы;
- товары;
- количество;
- цена;
- сумма строки.

</div>
<div>

## Формулы и проверка

- итоговая сумма через `SUM`;
- среднее значение через `AVERAGE`;
- базовое форматирование;
- проверка, что формулы записаны в ячейки.

</div>
</div>

<div class="mt-8 text-xl font-bold">
Проверку запускаем в новой сессии: индекс skills пересобирается, и новый `xlsx` появляется в системном prompt.
</div>

---
layout: default
---

# Наблюдаемость: Phoenix

<div class="text-xl opacity-80 mt-4">
В ноутбуке мы запускаем локальный Phoenix и подключаем OpenInference-инструментирование LangChain. Так видно не только финальный ответ, но и шаги агента.
</div>

<div class="grid grid-cols-2 gap-8 mt-7">
<div>

## В трейсе видно

- какие сообщения ушли в модель;
- какие tools вызвал агент;
- прочитал ли он `SKILL.md`;
- какие файлы и scripts использовал;
- расход токенов и время выполнения;
- где появились лишние шаги или цикл.

</div>
<div>

## Зачем это в практике

- отлаживать поведение агента по шагам;
- проверять, что новый skill реально сработал;
- собирать удачные и неудачные прогоны в датасет;
- запускать эксперименты и оценки после изменений;
- улучшать `SKILL.md` и helper-скрипты по trace.

</div>
</div>

---
layout: section
---

# Материалы

## Репозиторий, ноутбук, QR-коды

---
layout: default
---

# Скачать репозиторий

Отсканируйте QR-код «репозиторий» или откройте ветку с материалами вебинара.

```bash
git clone --branch codex/deep-agents-only-materials \
  https://github.com/trashchenkov/agent-skills-masterclass
cd agent-skills-masterclass
```

<div class="mt-6 text-xl opacity-80">
Ветка отдельная, чтобы материалы вебинара не смешивались с прошлой конференционной версией.
</div>

---
layout: default
---

# Установить окружение

Нужны Python 3.12+ и `uv`.

```bash
uv init . --python 3.12 --no-workspace

uv add jupyter \
  langchain langchain-core langchain-openrouter \
  deepagents langgraph \
  arize-phoenix openinference-instrumentation-langchain \
  opentelemetry-sdk opentelemetry-exporter-otlp \
  openpyxl python-dotenv
```

<div class="mt-6 text-lg opacity-80">
Для практики с Excel ключевые зависимости: `deepagents`, `langgraph`, `openpyxl`, Phoenix-инструментирование.
</div>

---
layout: default
---

# Запустить ноутбук

Добавьте ключ модели в `.env` и запустите Jupyter через `uv`:

```bash
echo "OPENROUTER_API_KEY=sk-or-v1-..." >> .env
uv run jupyter notebook masterclass.ipynb
```

<div class="mt-8 text-xl font-bold">
Phoenix откроется локально: <code>http://localhost:6006</code>
</div>

<div class="mt-4 text-lg opacity-80">
Там будем смотреть, как агент выбирает skill, читает инструкции, запускает tools и проверяет результат.
</div>

---
layout: end
---

# Переходим к ноутбуку

## Делаем xlsx-skill

<div class="mt-8 text-xl opacity-80">
QR-коды ведут в ветку репозитория с материалами вебинара и в Telegram-канал.
</div>

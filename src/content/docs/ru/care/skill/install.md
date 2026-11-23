---
title: Установка и использование
description: Установите переносимый навык MAESTRO одной командой для Claude Code, Codex, hermes или OpenClaw, сделайте CLI доступным и проверьте.
sidebar:
  order: 2
---

Переносимый навык MAESTRO — небольшой бандл (`maestro/` с `SKILL.md`, `scripts/` и
`references/`), который учит внешнего агента работать с MAESTRO. Ему нужен рабочий
CLI MAESTRO, который навык сам **не** ставит (см. шаг 2). Навык агента внутри
приложения поставляется вместе с приложением, ставить его не нужно.

## 1. Установка навыка

```bash
uvx maestro-install skill
```

`skill` — действие опубликованного мастера `maestro-install`. Оно скачивает бандл и
распаковывает его в каталог навыков выбранного агента. Мастер спросит, какого
агента выбрать, или укажите его через `--agent`:

| `--agent` | Каталог навыков |
| --- | --- |
| `claude` | `~/.claude/skills/` |
| `codex` | `~/.codex/skills/` |
| `hermes` | `~/.hermes/skills/` |
| `openclaw` | `~/.openclaw/skills/` |
| `all` | все четыре каталога выше |
| `<path>` | свой каталог навыков |

```bash
uvx maestro-install skill --agent all
```

С `--fast` (или `--non-interactive`) мастер ничего не спрашивает: ставит навык всем
агентам, у которых есть домашний каталог, а если таких нет — в Claude Code. Верхняя
папка бандла — `maestro/`. Существующая `maestro/` сохраняется как `maestro.bak`.

По умолчанию бандл скачивается с
`https://airi-maestro.github.io/maestro-docs/maestro.skill`. Другой адрес задаётся
через `--skill-url` или `$MAESTRO_SKILL_URL` (зеркало, форк, локальная сборка):

```bash
uvx maestro-install skill --agent claude \
  --skill-url https://airi-maestro.github.io/maestro-docs/maestro.skill
```

:::note[Старый пакет]
В старом пакете `care-install` действия `skill` нет, используйте `maestro-install`.
Обновить сам мастер: `uvx --refresh maestro-install`.
:::

**Вручную.** Скачайте тот же адрес и распакуйте сами:

```bash
curl -fsSLO https://airi-maestro.github.io/maestro-docs/maestro.skill
unzip maestro.skill -d ~/.claude/skills/     # → ~/.claude/skills/maestro/
```

В hermes команда `/skills` покажет **maestro**. Вызов — `/maestro` или просто
описание задачи для MAESTRO. В Claude Code навык срабатывает на задачах MAESTRO сам.

:::caution[Совпадение имён]
Навык определяется полем `name` во frontmatter, а не именем папки. Копия в
`maestro.bak/` тоже объявляет `name: maestro`. Если агент загружает все папки из
каталога навыков, после проверки новой версии удалите или перенесите копию.
:::

## 2. Проверьте CLI из своего агента

Откройте новую сессию и попросите: «Используй скилл MAESTRO. Покажи, как запускается CLI, перечисли доступные команды и проверь локальные настройки без сетевых проб».

Агент выполняет эти команды из каталога установленного скилла:

```bash
bash scripts/care.sh --where
bash scripts/care.sh --help
bash scripts/care.sh doctor --no-probes
```

Первая команда показывает источник CLI. Если локальной установки нет, скрипт может скачать пакет через `uvx` при первом вызове. Вторая команда показывает возможности именно вашей версии. Третья проверяет настройки и установленные зависимости. Ключ модели и запущенная Memory для этой проверки не нужны.

## 3. Настройте рабочую папку для генерации

Попросите агента помочь настроить MAESTRO для вашего проекта. Команда `uvx maestro-install install` запускает интерактивный мастер рабочей папки и провайдера модели. Введите ключ в терминале мастера. Добавьте `~/.local/bin` в `PATH`, чтобы агент видел установленную команду `maestro`.

Чтобы проверить соединения после настройки, попросите скилл выполнить `doctor` с сетевыми пробами. Для сохранения цепочек нужна Memory, для эволюции — также Platform.

:::note[Пути к файлам]
Когда CLI найден через глобальный shim или `uvx`, он работает из вашей **рабочей
папки** MAESTRO, и относительные пути считаются от неё. Передавайте **абсолютные
пути** в `--output`, `--export`, `run --file`, `validate`, `replay`, шаблоны
`import` и `dataset export`.
:::

## Обновление

Повторите `uvx maestro-install skill`: папка заменится, прежняя копия останется как
`maestro.bak`. Навык и CLI версионируются отдельно. Редакция указана в поле `Skill revision` в начале `SKILL.md`. Сам MAESTRO обновляется через
`uvx maestro-install update`, перенастраивается через
`uvx maestro-install reconfigure`.

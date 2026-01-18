# PolyText Format (PTXT) — v1.0

> Multilingual linear text format for educational and interactive reading systems

**File extension:** `.ptxt`
**MIME type:** `text/polytext; charset=utf-8`

---

## English

### Overview

PolyText (PTXT) is a lightweight, line‑based format for storing multilingual documents with precise sentence alignment and optional word‑level translations.

It is designed for:

* language learning platforms,
* interactive textbooks,
* AI‑generated educational content,
* translation‑assisted reading systems.

The format is intentionally simple, human‑readable, and deterministic for reliable parsing and generation by software and AI models.

### Core principles

* One **source language** per document (first language in the first block).
* Unlimited number of additional languages.
* Sentence‑level translations grouped into translation units.
* Word‑level translations via a built‑in dictionary.
* No numeric IDs.
* No closing tags.
* No XML/JSON overhead.

### Block structure

A document is a sequence of blocks.

Each block starts with:

```
::<tag>
```

and ends implicitly when the next block starts or at end of file.

Supported tags:

```
::h1
::h2
::h3
::p
::ul
::ol
::dict
```

### Translation units

Inside content blocks, text is grouped into **translation units**.

A translation unit is:

```
<lang>: <text>
<lang>: <text>
...

```

(terminated by an empty line)

The full source‑language text of the unit acts as its implicit identifier.

### Dictionary block

The dictionary defines word‑level semantic equivalence.

```
::dict
pl: Ziemia
uk: Земля
en: Earth

pl: Słońce
uk: Сонце
en: Sun
```

Each group represents one concept across languages.

### Inline markup (markdown‑lite)

```
**bold**
*italic* or _italic_
__underline__
`code`
```

Processed before word tokenization.

---

## Українською

### Огляд

PolyText (PTXT) — це легкий пострічковий формат для зберігання багатомовних текстів із точною синхронізацією речень та додатковими перекладами слів.

Призначений для:

* платформ вивчення мов,
* інтерактивних підручників,
* навчального контенту, створеного ШІ,
* систем читання з підтримкою перекладу.

Формат простий, читабельний для людини та однозначний для обробки програмами.

### Основні принципи

* Одна **мова‑джерело** в документі.
* Необмежена кількість мов перекладу.
* Переклади на рівні речень (translation units).
* Переклади слів через словник.
* Без нумерації.
* Без закриваючих тегів.
* Без XML або JSON.

### Структура блоків

Кожен блок починається з:

```
::<tag>
```

Підтримувані теги:

```
::h1
::h2
::h3
::p
::ul
::ol
::dict
```

### Одиниці перекладу

```
<код_мови>: <текст>
<код_мови>: <текст>

```

ID одиниці = повний текст мовою‑джерелом.

### Словник

Блок `::dict` задає відповідності слів між мовами як одне поняття.

---

## По‑русски

### Обзор

PolyText (PTXT) — это лёгкий построчный формат для хранения многоязычных текстов с точным выравниванием предложений и дополнительными переводами слов.

Предназначен для:

* языковых обучающих платформ,
* интерактивных учебников,
* образовательного контента, созданного ИИ,
* систем чтения с подсказками перевода.

Формат прост для человека и однозначен для программной обработки.

### Основные принципы

* Один **язык‑источник** в документе.
* Любое количество языков перевода.
* Переводы на уровне предложений.
* Переводы слов через словарь.
* Без нумерации.
* Без закрывающих тегов.
* Без XML и JSON.

---

## MIME type

Recommended MIME type:

```
text/polytext; charset=utf-8
```

---

## Minimal example

```
::p
pl: Ziemia krąży wokół Słońca.
uk: Земля обертається навколо Сонця.
en: The Earth orbits the Sun.

::dict
pl: Ziemia
uk: Земля
en: Earth
```

---

## Validator rules

### Structural rules

1. Every block must start with `::<tag>`.
2. Translation units must be separated by exactly one empty line.
3. Each non‑empty content line must match:

```
^[a-zA-Z]+:\s+.*$
```

4. Each translation unit must contain the source language.
5. `::dict` entries follow the same grouping rules.

### Critical errors

* Unknown block tag.
* Line without `<lang>:` prefix.
* Missing source language line in a translation unit.
* Block header inside a translation unit.

---

## Reference validator (pseudocode)

```pseudo
function validatePTXT(text):
    blocks = parseBlocks(text)

    if blocks is empty:
        error("No blocks")

    sourceLang = detectSourceLanguage(blocks[0])

    for block in blocks:
        for unit in block.units:
            if sourceLang not in unit.languages:
                error("Missing source language")

            for line in unit.lines:
                if not matches "<lang>: <text>":
                    error("Invalid language line")

    return VALID
```

---

## Formal grammar (EBNF)

```
document         = { block } ;
block            = block_header , { translation_unit } ;
block_header     = "::" , tag , newline ;
tag              = "h1" | "h2" | "h3" | "p" | "ul" | "ol" | "dict" ;
translation_unit = { language_line } , empty_line ;
language_line    = lang_code , ":" , space , text , newline ;
lang_code        = letter , { letter } ;
text             = { any_char_except_newline } ;
empty_line       = newline ;
newline          = "\n" | "\r\n" ;
```

---

## Status

PolyText (PTXT) v1.0 — stable reference specification. By Maxopa and Axiom.

## License

This project is licensed under:

PolyText Noncommercial License 1.0.0

Commercial usage requires a separate commercial license.

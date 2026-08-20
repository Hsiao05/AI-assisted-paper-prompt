---
name: pdf-to-markdown
description: Convert short or medium-length PDF documents, including scanned PDFs, and other formats of text files, into high-fidelity Markdown while preserving original text, paragraph logic, heading hierarchy, multi-column reading order, tables, figures, charts, footnotes, and formulas as faithfully as possible.
---

# PDF to Markdown

## Purpose

Use this skill when the user asks to convert a PDF, especially a scanned PDF, or other similar forms of text files, into Markdown for later reading, analysis, archiving, or use by an AI agent.

The task is **transcription and structural conversion**, not summarization, editing, rewriting, translation, proofreading, or content improvement.

The highest priority is fidelity to the source.

---

## Core Rules

1. Do not summarize, rewrite, polish, translate, correct, simplify, or supplement the source text.
2. Except for explicitly required structural conversion, figures, charts, tables, and layout cleanup, preserve source wording as faithfully as possible.
3. Preserve:
   - Chinese, English, Japanese, and other source languages
   - punctuation
   - capitalization
   - numbers
   - dates
   - names
   - quotation marks
   - brackets
   - footnote markers
   - spelling, including apparent source errors
4. Never silently correct an apparent typo, grammar problem, spelling problem, OCR-like source error, or factual error.
5. Never guess unreadable text.
6. If text cannot be reliably recognized, insert:

   `[无法辨认]`

7. If multiple readings are plausible but none can be confirmed, insert:

   `[无法确认：可能为“...”]`

8. Do not add commentary, interpretation, evaluation, or summary.
9. Final output should contain the converted Markdown only unless the user explicitly asks for an explanation.

---

## Paragraph Preservation

Preserve the original paragraph logic as faithfully as possible.

### Paragraph boundaries

- If the source contains one paragraph, keep it as one paragraph.
- If the source starts a new paragraph, start a new paragraph.
- Do not merge distinct source paragraphs.
- Do not split one source paragraph into several paragraphs merely for readability.
- Do not reorganize paragraphs based on semantic interpretation.

### Visual line breaks

Ignore line breaks caused only by page width.

For example, if the PDF visually shows:

`This is an important`  
`example of cultural change.`

but both lines belong to one paragraph, output:

`This is an important example of cultural change.`

### Cross-page paragraphs

If a paragraph continues from one page to the next, restore it as one continuous paragraph.

Example:

Page 10:

`The representation of death in modern`

Page 11:

`society has undergone significant changes.`

Output:

`The representation of death in modern society has undergone significant changes.`

Only merge across pages when the relationship is clear.

If uncertain, insert:

`[跨页段落关系无法确认]`

### English line-break hyphenation

If a word is broken only because of a line break, such as:

`inter-`  
`national`

restore it to:

`international`

Do not remove real lexical hyphens such as:

`well-known`

---

## Heading Structure

Convert source headings into a consistent Markdown hierarchy.

Use:

```markdown
# Level 1
## Level 2
### Level 3
#### Level 4
```

Rules:

1. Keep headings at the same structural level consistent throughout the document.
2. Infer hierarchy from:
   - chapter numbering
   - section numbering
   - font size and layout
   - surrounding document structure
   - repeated heading patterns
3. Preserve heading text exactly.
4. Preserve heading numbering exactly.
5. Do not rename, shorten, translate, or rewrite headings.
6. Do not mistake bold body text for a heading.
7. Do not mistake repeating running headers for headings.

Example:

`Chapter 2`

`2.1 Previous Studies`

`2.1.1 Research Background`

should be mapped consistently to appropriate Markdown heading levels.

---

## Page Numbers, Headers, and Footers

### Page numbers

Remove page numbers used only for navigation, such as:

`37`

`— 37 —`

`Page 37`

Do not remove numbers that belong to body text, tables, references, or contents.

### Repeating headers and footers

Remove repeated running headers and footers that exist only for publication layout, such as:

- book title
- journal title
- current chapter name
- author name
- publisher name
- repeated copyright text

### Footnotes

Do not confuse footnotes with page footers.

Footnotes and endnotes are part of the document content and must be preserved.

---

## Multi-Column Layout

If the PDF uses multiple columns, determine the correct human reading order before transcription.

For a typical two-column paper, the expected order is usually:

`left column top-to-bottom → right column top-to-bottom`

Do not interleave lines from the left and right columns.

Handle:

- two-column pages
- three-column pages
- pages mixing single-column and multi-column regions
- full-width headings over columns
- full-width tables
- full-width figures

The final Markdown does not need to visually reproduce columns. It should preserve the correct logical reading order.

---

## Lists, Quotes, Footnotes, and Endnotes

### Lists

Convert real source lists into Markdown lists when appropriate.

Unordered:

```markdown
- Item one
- Item two
```

Ordered:

```markdown
1. First
2. Second
```

Preserve original numbering where it carries meaning. Do not renumber source items unnecessarily.

### Block quotations

Use Markdown blockquote syntax only when the source clearly contains an independent quotation block:

```markdown
> Quoted content
```

### Footnotes and endnotes

If the source contains footnotes, endnotes, explanatory notes, translator's notes, editorial notes, or other numbered annotations, they must be preserved in the Markdown.

The annotation marker and annotation text must remain correctly paired.

Strict rules:

1. Preserve the annotation number or symbol exactly as it appears in the source.
2. Do not renumber notes automatically.
3. Do not merge two different notes.
4. Do not split one note into several notes.
5. Do not assign a note to a paragraph, sentence, or chapter unless the source relationship is clear.
6. Preserve the note text faithfully. Do not summarize, rewrite, correct, translate, or normalize it.
7. If the source uses non-Arabic note markers, such as `*`, `†`, `①`, Roman numerals, letters, or other symbols, preserve those markers whenever practical.
8. If the note marker is visible but the corresponding note text cannot be reliably identified, preserve the marker and insert an explicit uncertainty notice rather than inventing the note.
9. If the note text is visible but its marker relationship cannot be reliably determined, preserve the note text and mark the relationship as uncertain.

When Markdown footnote syntax can preserve the source numbering faithfully, it may be used.

Example:

```markdown
正文内容[^1]

[^1]: 脚注内容
```

If the original note number is `7`, preserve it:

```markdown
正文内容[^7]

[^7]: 原文中的第7条注释文本
```

Do not change it to `[^1]` merely because it is the first note encountered in the current excerpt.

#### Placement of footnote text

For page footnotes or paragraph-level footnotes, the note text may be moved out of the visual page footer and placed at a logical Markdown boundary, provided that:

- the original note marker remains attached to the correct source text;
- the original note number remains unchanged;
- the note text remains unchanged;
- the relationship between marker and note remains unambiguous.

Preferred placement order:

1. At the end of the relevant paragraph group, when the note clearly belongs to a local passage;
2. At the end of the relevant subsection or section, when several notes belong to that section;
3. At the end of the chapter, when this best reflects the source structure.

Do not scatter footnote text arbitrarily if doing so would make note relationships harder to follow.

#### Placement of endnote text

If the source uses endnotes rather than page footnotes, preserve that logic.

Depending on the original document, place endnote text:

- at the end of the relevant chapter, if the source uses chapter endnotes; or
- at the end of the full document, if the source uses document-level endnotes.

Do not convert endnotes into page-style footnotes unless necessary for faithful Markdown representation.

#### Source structure takes precedence

The placement rules above are flexible. The original document's annotation system takes precedence.

Choose the Markdown placement that best preserves:

- original numbering;
- note-to-text correspondence;
- chapter or document scope;
- reading order;
- source meaning.

The goal is not to reproduce the exact visual position of a footnote at the bottom of a PDF page, but to preserve the annotation system faithfully and make the correspondence clear in Markdown.

---

## Images

Do not preserve the original image in the final Markdown unless the user explicitly asks to keep it.

Replace photographs, illustrations, diagrams, maps, screenshots, and similar visual material with:

`【图片内容：...】`

The description should allow an AI that cannot see the source image to understand its important content.

Include, when visible:

- major people or objects
- scene and spatial relationships
- arrows
- connectors
- labels
- numbers
- visible text
- relevant visual structure

Do not guess:

- identity
- location
- date
- causal meaning
- hidden information

Keep original figure captions as source text. Do not merge the caption into the generated description unless necessary.

Example:

`【图片内容：一张黑白照片。画面中央为一座三层建筑，正面入口上方可见“……”字样。建筑前方站有约十余人，分为两排。图片右下角存在无法辨认的小字。】`

---

## Charts and Graphs

For line charts, bar charts, pie charts, scatter plots, statistical charts, flowcharts, and similar graphics:

First identify, where possible:

- chart title
- chart type
- x-axis
- y-axis
- units
- legend
- series
- labels
- reliably readable values

### If exact data can be recovered reliably

Prefer converting the data into a Markdown table.

Example:

```markdown
| 年份 | A组 | B组 |
|---|---:|---:|
| 2020 | 15 | 18 |
| 2021 | 17 | 21 |
```

Then optionally add:

`【图表内容：该折线图比较了……。A组从……变化至……；B组……。】`

### If exact values cannot be recovered reliably

Do not invent numbers.

Use a structured natural-language description:

`【图表内容：该图为折线图。横轴表示……，纵轴表示……。共有三组数据……。从可见趋势来看……。部分具体数值由于图像清晰度不足无法可靠确认。】`

The goal is to preserve the chart's variables, relationships, structure, and major trend for later AI understanding.

---

## Tables

### Simple tables

Convert to Markdown tables when structure is clear.

Example:

```markdown
| 项目 | 数据A | 数据B |
|---|---:|---:|
| A | 10 | 20 |
| B | 15 | 25 |
```

Rules:

1. Preserve cell text exactly.
2. Preserve numbers exactly.
3. Preserve row and column relationships.
4. Preserve headers accurately.
5. Do not shift data because of blank cells.

### Complex tables

If the source contains:

- merged cells
- multi-level headers
- rowspan
- colspan
- irregular nested structure

and Markdown cannot represent it faithfully, use HTML:

```html
<table>
  ...
</table>
```

### Tables that cannot be reconstructed reliably

Do not fabricate a table.

Use:

`【表格内容：该表共有……列、……行。表头依次为……。第一组数据显示……。第二组数据显示……。由于原表存在复杂合并单元格，无法可靠恢复完整二维结构。】`

Prefer explicit uncertainty over a structurally incorrect table.

---

## Mathematical Formulas

If a mathematical expression can be recognized reliably, convert it to LaTeX.

Inline:

```markdown
$E = mc^2$
```

Display:

```markdown
$$
E = mc^2
$$
```

Do not derive, correct, simplify, or alter formulas.

If recognition is unreliable, insert:

`[公式无法可靠辨认]`

---

## Contents and References

### Table of contents

Preserve entries in source order.

Page numbers may be retained when they are structurally meaningful to the contents page.

### References and bibliography

Transcribe references faithfully.

Do not:

- normalize citation style
- convert APA to MLA or vice versa
- add DOI
- correct author names
- correct titles
- supplement missing fields
- standardize capitalization
- infer missing years or publication data

Preserve the source format.

---

## OCR Ambiguity

Be especially careful with:

- low-resolution scans
- small fonts
- Japanese kanji and kana
- traditional vs simplified Chinese
- `0/O`
- `1/l/I`
- `rn/m`
- hyphens
- superscripts and subscripts
- special symbols
- personal names
- bibliography numbering
- URLs
- DOI strings
- years
- table numbers

Distinguish between:

- confidently recognized
- uncertain
- unreadable

Never hide uncertainty for the sake of producing cleaner output.

---

## Markdown Formatting

Use clean, minimal Markdown.

Rules:

- blank line before and after headings
- one blank line between paragraphs
- remove meaningless hard line breaks caused by PDF layout
- do not insert page markers such as `Page 1`
- remove standalone page numbers
- remove repeated running headers and footers
- do not add decorative horizontal rules unless present and meaningful
- do not wrap the entire output in a Markdown code fence
- do not add an introduction such as “以下是转换结果”
- do not add a completion message at the end

Except for required markers such as:

- `【图片内容：...】`
- `【图表内容：...】`
- `【表格内容：...】`
- `[无法辨认]`
- `[无法确认：...]`
- `[公式无法可靠辨认]`
- `[跨页段落关系无法确认]`

do not add text not present in the source.

---

## Internal Pre-Output Check

Before finalizing, silently verify:

1. No body paragraph was omitted.
2. No text was rewritten, polished, corrected, or translated.
3. Paragraph boundaries were not arbitrarily changed.
4. Cross-page paragraphs were restored where clearly appropriate.
5. Footnotes, endnotes, and other annotations were not mistaken for footers, and their original markers/numbers remain correctly paired with the corresponding note text.
6. Standalone page numbers and repeating headers/footers were removed.
7. Multi-column reading order is correct.
8. Heading levels are consistent across the document.
9. Images were replaced with `【图片内容：...】`.
10. Charts were converted to data or sufficiently detailed descriptions.
11. Table row/column relationships are correct.
12. Unreadable text was not guessed.
13. No summary, commentary, or evaluation was added.

Do not output this checklist or the reasoning used to perform it.

---

## Final Instruction

When a PDF is provided, read the entire document and convert it into one continuous Markdown document according to all rules above.

Start directly from the document content.

Do not output:
- prefaces
- process explanations
- summaries
- completion notices
- comments about the task

Output only the converted Markdown.

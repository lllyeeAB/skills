---
name: knowledge-base-curator
description: Turn troubleshooting notes, chat logs, postmortems, technical discussions, and rough solution notes into knowledge-base-ready Markdown documents. Use this skill whenever the user asks to “沉淀知识库”, “整理成文档”, “写成知识库”, “输出 FAQ/方案文档”, or wants a reusable document optimized for internal AI/RAG retrieval. This skill is especially useful for technical knowledge capture, engineering collaboration writeups, tool usage experience, and problem-solution documentation.
---

# Knowledge Base Curator

Turn raw materials into a clean Markdown document that is ready to be stored in a knowledge base.

## When To Use

Use this skill when:

- The user provides troubleshooting logs, chat records, issue discussions, review conclusions, or a postmortem and wants them turned into a knowledge-base article.
- The user wants to turn one solved problem into a reusable document for teammates.
- The user asks for an FAQ, troubleshooting note, technical explanation, SOP, or solution writeup that should follow a consistent internal style.
- The user does not explicitly say “knowledge base” but clearly wants a reusable document that is easy for humans and AI systems to search.

Do not use this skill as the final mode when:

- The user wants executable automation rather than documentation.
- The user wants to decide whether something should become a knowledge-base article, a prompt template, or a skill.
- The user only wants a quick explanation and does not want a reusable document artifact.

## Goal

Produce a high-signal Markdown document that:

- Stays focused on one core problem.
- Uses a retrieval-friendly title.
- Explains causes and solutions rather than turning into source-code walkthrough.
- Is readable by newcomers.
- Preserves the important evidence and context while removing noisy timeline details.
- Asks clarifying questions when critical decisions are unclear.

## Workflow

1. Identify the single core problem in the source material.
2. Infer how a real user would search for this problem.
3. Extract the most frequent and most scope-defining keywords from the context as candidate title tags.
4. Extract the core facts: symptoms, cause, solution, caveats, scope, and keywords.
5. Generate 3 to 5 candidate knowledge-base titles for the user to choose from, or to refine.
6. Ask the user whether the final knowledge-base article should be written in Chinese or English, unless the user has already made that clear.
7. Ask the user what document style they want when it matters: FAQ, troubleshooting guide, or principle/explanation article.
8. Remove side branches, repetition, and chat noise.
9. If any critical decision is unclear, ask the user before finalizing the article.
10. After the title, language, and style are confirmed, generate the final Markdown document.

If the source material contains multiple problems:

- Default to the most reusable and central one.
- Tell the user the rest should be split into separate articles instead of forcing them into one document.

If the material is incomplete:

- Make the safest possible summary from what is available.
- Explicitly mark uncertain parts as assumptions or items that still need confirmation.
- Do not fabricate missing facts.

## Clarifying Questions

Ask the user before writing when any of the following is unclear:

- What the actual core problem is.
- Which of multiple title directions best matches real search intent.
- Whether the title tag should use a product name, system name, module name, tool name, or internal term.
- Whether the evidence is strong enough to claim a root cause.
- Who the target audience is, such as newcomers, engineers, support staff, or cross-team readers.
- Whether the final article should be written in Chinese or English.
- Whether the document should read more like an FAQ, a troubleshooting guide, or a principle/explanation article.

## Title Tag Rules

The title tag is not a fixed taxonomy and should not depend on one team’s private folder system.

Choose tags using these rules:

- Prefer keywords that appear frequently in the source material and sharply narrow the scope.
- Prefer terms that users would actually search.
- A tag can be a product name, system name, module name, component name, tool name, or process name.
- Keep the tag short.
- Prefer one core tag, not multiple stacked tags.
- If no stable high-signal keyword exists, ask the user instead of forcing a tag.

Acceptable tag examples:

- `[MCP]`
- `[Docker Agent]`
- `[OpenAPI]`
- `[CI]`
- `[权限系统]`
- `[支付网关]`

## Title Rules

Every final title should:

- Be written from the searcher’s point of view, not the author’s point of view.
- Match the way a real user would ask the question.
- Use a tag only when the tag helps retrieval.
- End as a question or a clear problem-solving statement.
- Make the problem and the expected resolution obvious at a glance.

Good title examples:

- `[MCP]openEnv后CDP连接失败无法使用浏览器工具怎么排查？`
- `[Electron]为什么内存占用会持续增长以及如何优化？`
- `[Docker Agent]如何部署和排查启动异常？`
- `[权限系统]角色权限配置不生效怎么定位？`

Avoid these title styles:

- Raw technical keyword piles.
- File-name-like titles.
- Vague titles such as “技术记录”, “问题总结”, or “复盘整理”.
- Author-centric titles such as “本次排查复盘” or “今天遇到的问题”.

When generating titles, think in this order:

1. How would the user ask this question?
2. What keyword would they search first?
3. Which keyword best narrows the scope?
4. Does the title make the problem and the expected outcome obvious?

## Candidate Title Output

Before writing the full article, default to proposing 3 to 5 candidate titles.

These candidate titles should:

- Stay on the same core problem.
- Vary the retrieval angle rather than drifting into different topics.
- Explore useful search entry points such as symptom-first, troubleshooting-first, root-cause-first, or solution-first phrasing.
- Keep the tag stable unless changing the tag materially improves retrieval.

Use an output pattern like this:

```markdown
Suggested title candidates:
1. [MCP]openEnv后CDP连接失败无法使用浏览器工具怎么排查？
2. [MCP]为什么openEnv成功后仍然无法建立CDP连接？
3. [CDP]浏览器工具无法使用且连接失败时怎么定位问题？
4. [MCP]openEnv后截图点击等浏览器工具不可用如何解决？

Please choose one title, or tell me the title style you want and I will continue.
```

If the user already gives a title:

- Check whether it is retrieval-friendly.
- If it has obvious problems, provide 2 to 3 improved alternatives.
- Do not silently replace the user’s title without confirmation.

## Output Format

Always use this structure for the final article:

```markdown
# [Tag]Title

## Background

One short paragraph explaining who this document is for and why it is needed.

## Symptoms / Problem

Describe triggers, visible behavior, and error manifestations. Keep logs short and only include the lines that matter.

## Cause

Explain the underlying mechanism, dependency, chain, or design reason. This is the most important section. Do not stop at vague statements like “bad config” or “network issue.”

## Solution

Give concrete steps. Split by scenario when needed.

## Notes

List caveats, edge cases, version limits, safety concerns, and future prevention advice.

## Scope

State the applicable systems, modules, versions, and scenario boundaries.

## Keywords

List 6 to 12 retrieval keywords. Chinese and English can be mixed if that matches real search behavior.
```

## Writing Constraints

- One problem per document.
- Prioritize cause analysis over code.
- Keep code blocks below roughly 30 percent of the article unless the user explicitly wants code-heavy output.
- Only include commands or code that genuinely help explanation.
- Use direct, professional, reusable language.
- Prefer explaining why and how to judge before explaining how to change.
- If the source material comes from chat logs or troubleshooting records, remove spoken-language repetition and noisy chronology.

## Style Variants

When useful, explicitly ask the user which style they want before drafting the final article.

Supported styles:

- `FAQ`
  Use when the user wants fast lookup, short answers, and direct reuse in support or repeated Q&A scenarios.
- `Troubleshooting Guide`
  Use when the user wants symptom, cause, diagnostic path, and concrete fix steps. This is the default for incident and bug materials.
- `Principle / Explanation`
  Use when the user wants mechanism, architecture, implementation logic, or why the system behaves this way.

If the user does not care about style:

- Default to `Troubleshooting Guide` for incidents, errors, and recovery topics.
- Default to `Principle / Explanation` for architecture, design, mechanism, and implementation topics.
- Default to `FAQ` only when the source material is clearly short-form Q&A oriented.

Style adjustments:

- `FAQ`: keep sections shorter, answers tighter, and step ordering simple.
- `Troubleshooting Guide`: emphasize symptoms, diagnosis, root cause, and repair steps.
- `Principle / Explanation`: spend more space on mechanism and design reasoning, with less emphasis on procedural recovery.

## Output Sequence

Use a two-step output by default.

Step 1: provide title candidates and confirm article language and style.

```markdown
Suggested title candidates:
1. ...
2. ...
3. ...

Should I write the final knowledge-base article in Chinese or in English?
Should the final article be written as an FAQ, a troubleshooting guide, or a principle/explanation article?
Please choose one title, or give me your own title.
```

Step 2: after the title, language, and style are confirmed, provide:

```markdown
Suggested filename: [MCP]openEnv后CDP连接失败无法使用浏览器工具怎么排查？.md
If you want a specific folder, please provide your own knowledge-base classification rule.
```

Then provide the full Markdown article.

## Quality Check

Before finalizing, verify:

- Is the title phrased from the user’s search perspective?
- Does the tag come from a high-signal keyword in the context?
- Did you provide 3 to 5 title choices unless the user explicitly skipped that step?
- Did you confirm whether the final article should be in Chinese or English?
- Did you confirm the intended document style when it materially affects structure?
- Is the article focused on one problem?
- Does the article explain the real cause rather than only describing symptoms?
- Is the filename suggestion clear?
- Is the document readable by newcomers?
- Is it likely to be retrieved successfully by an internal AI system?

If any answer is no, revise before finalizing.

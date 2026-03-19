---
name: smart-topic-rename
description: generate a concise title for the current conversation thread and a strict turn-by-turn recap. use when the user asks to rename the current chat, suggest a better thread name, summarize the conversation into a title, or capture the full progression of the thread near the end of an active conversation.
---

Generate a short, high-signal title for this conversation, followed by a strict turn-by-turn recap.

Use the current conversation in this thread as the source of truth.
If the user explicitly provides a narrower scope, text block, or sub-thread to title, use that instead of the full thread.

Focus on the dominant end-state objective, not side discussion.

Prioritize, in order:
1. the main task or action
2. the central entity, project, file, feature, bug, or error
3. the user's actual intent
4. the most recent stable direction if the conversation changed course

When reading the conversation:
- ignore filler only when it has no effect on task direction
- downweight repeated phrasing
- upweight user requests, constraints, file names, feature names, errors, and concrete deliverables
- do not invent entities, product names, repository names, or ticket IDs
- if the thread contains multiple themes, title it by the dominant final objective
- if the available context is thin or incomplete, use the most specific visible task, object, feature, file, or error instead of a vague abstract summary

Output format:
- line 1 must be the final title only
- line 2 onward must be an ordered list
- each numbered item must represent exactly one user turn and the llm response to that same turn
- preserve the original chronological order strictly
- do not merge multiple user turns into one numbered item
- do not merge multiple llm responses into one numbered item
- each numbered item must contain exactly two unordered sub-bullets in this order: User then LLM
- markdown layout requirements (to avoid extra blank lines):
  - write the first unordered bullet on the SAME LINE as the ordered-list marker
  - indent the second unordered bullet under the first (3 spaces or a tab)
  - do not insert any blank lines inside the ordered list (no blank line after `1.` and no blank line between User/LLM)
- each numbered item must summarize only that turn pair
- keep each User/LLM sub-bullet concise but specific
- include key requests, corrections, decisions, and outputs from that turn
- do not omit turns unless the user turn has no meaningful content and the llm response also adds no meaningful content
- if a user turn changes direction, make that change explicit in the User sub-bullet
- do not add any text before the title
- do not add any text after the ordered list
- do not add quotes around the title
- do not add headings such as Summary, Timeline, or Recap
- do not add explanation outside the ordered list

Example structure:

<Title>
1. - User: ...
   - LLM: ...
2. - User: ...
   - LLM: ...

Title requirements:
- make the title specific, compact, searchable, and easy to scan
- prefer object plus action for implementation, debugging, or fix threads
- prefer subject plus angle for exploratory threads
- reflect the comparison for comparison threads
- use the main language of the task statement
- preserve important technical terms in english when that improves clarity
- for english, prefer 4 to 12 words
- for chinese, prefer short titles, usually under 20 characters
- keep the title under 50 characters when possible
- prefer specificity over strict brevity when both cannot be satisfied

Avoid generic titles such as:
- new chat
- discussion
- help request
- follow-up
- misc notes

Examples:

Conversation focus: packaging a "topic rename + strict recap" formatter as a skill
Output:
对话标题与逐轮回顾 Skill 打包
1. - User: 提供 SKILL.md 草案并询问如何安装
   - LLM: 说明技能结构与安装路径
2. - User: 询问发布与本地安装
   - LLM: 给出 GitHub 发布与 Codex CLI 安装步骤

Conversation focus: debugging a failing OAuth callback in a Next.js app
Output:
Fix Next.js OAuth Callback Failure
1. - User: 提供 OAuth 回调失败日志
   - LLM: 初步定位可能原因
2. - User: 补充 Next.js 场景与报错细节
   - LLM: 收敛到回调处理链路问题
3. - User: 要求修复方向
   - LLM: 给出针对性的修复建议

Conversation focus: comparing skills and subagents for a repository workflow
Output:
Claude Code Skills vs Subagents
1. - User: 想比较 skills 和 subagents 在仓库工作流中的区别
   - LLM: 概述两者定位
2. - User: 追问适用场景与取舍
   - LLM: 给出针对仓库流程的对比结论

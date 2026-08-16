# Agent Note：ask_user_question 透传问题补充说明

Status: implemented

[English](2026-08-16-tool-ask-user-question-detail.md) | 中文

## 问题

`AskUserQuestionItem` 声明了可选的 `detail` 字段，线上 schema 校验该字段，Web 提问 UI 在问题标题下方将其渲染为 markdown。而 `ask_user_question` 工具的 schema 未声明 `detail`，其 execute 映射也丢弃该字段，导致模型完全无法通过工具表达补充说明。

## 决策

`ask_user_question` 工具 schema 将 `detail` 声明为可选字符串，execute 映射将其透传给 `ctx.userQuestions.ask()`。字段描述沿用契约原文：随问题一同渲染、不进入选项标签的补充说明。Web UI 现有的 `detail` markdown 渲染保持不变。`docs/tool-catalog.md` 由 `doc-sync` 重新生成。

## 备选方案

**把更长的问题写入 `question` 文本** —— 否决。Web UI 将 `question` 渲染为纯文本标题，其中的 markdown 不会渲染。

**只声明 `detail` 而不透传** —— 否决。仅存在于 schema 的字段永远不会到达 UI。

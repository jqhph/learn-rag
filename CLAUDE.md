# CLAUDE.md

## 项目简介

这是一个 **RAG 企业级知识库课程** 教学项目。课程从零基础出发，由浅到深讲解从 DEMO 到稳定企业级生产落地的完整 RAG 技能。

目标场景：客服 Q&A + 企业知识库。

## 开始任何课程编写任务前，必须先阅读

1. **`课程大纲.md`** — 全局课程结构、20 节课的完整规划、每节课的前置依赖、技术栈演进路线
2. **`MISSION.md`** — 课程的学习目标与边界
3. **`NOTES.md`** — 用户偏好与课程进度记录

## 课程文件组织规则

- 课程 HTML：`lessons/XXXX-<dash-case-name>.html`
- 速查表 HTML：`reference/XXXX-<dash-case-name>-cheatsheet.html`
- 学习记录 Markdown：`learning-records/XXXX-<dash-case-name>.md`
- 编号 `XXXX` 与 `课程大纲.md` 中的课程编号对齐（如 L8 对应 0008-...）

## 课程编写规范

每节课的标准结构见 `课程大纲.md` 末尾的"每节课的标准结构"章节。

核心约束：
- 所有内容用中文撰写
- 代码示例以 Python 为主
- 每节课基于前面课程的教学产物增量构建，不另起炉灶
- 知识点优先引用 `RESOURCES.md` 中的高质量资源
- 课程末尾需提醒学习者可以对 AI 老师追问

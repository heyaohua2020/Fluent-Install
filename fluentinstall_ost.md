---
name: fluentinstall-ost
description: FluentInstall (Steam 入库) 排查 skill 位置 + 核心踩坑：安装内核会把 opensteamtool.toml 写回作者路径导致库空
type: project
---

用户在 Windows 机器上跑 FluentInstall v3.1.4（Steam 入库工具）。它自带的"SteamTools 内核"实际是 OpenSteamTool(OST)。Steam 装于 `D:\Program Files (x86)\Steam`。

**核心踩坑（2026-09-05 实测）**：FluentInstall「安装内核」有 bug，会把 `Steam\opensteamtool.toml` 写成作者残留路径 `D:/zhouchentao/ruanjian/Steam/config/stplug-in`（还会建空骨架目录）。OST 只读 toml 指定目录 → 库空。任何内核重装/更新后游戏全不显示，先查这个文件。

**Why**: 安装内核流程硬编码 dev 路径，不检测用户 Steam 安装位置。

**How to apply**: 完整排查剧本 + 一键诊断脚本在 skill `fluentinstall-trouble`（`C:\Users\Administrator\.claude\skills\fluentinstall-trouble\`）。用户提到"入库没游戏/FluentInstall/SteamTools 不生效"时，直接跑 `bash /c/Users/Administrator/.claude/skills/fluentinstall-trouble/diag.sh` 或调 skill。普通库功能问题（游戏名搜索）也可能触发，读取诊断态：`config/stplug-in/*.lua` 在不在、内核 3 dll 全不全、toml 路径对不对、Steam 钩子加载没。
---
新电脑标准流程
装 FluentInstall
设置 → 净化 steam
设置 → 安装内核 DLL（自动部署 OpenSteamTool.dll + dwmapi.dll + XInput1_4.dll 到 Steam 根目录）
核对 Steam\opensteamtool.toml 路径 ← 新电脑必踩坑
入库
完整重启 Steam
看库
第 4 步最关键
安装内核 在新电脑同样会把 toml 写成作者残留路径:

toml
2 lines
Copy
[lua]

paths = ["D:/zhouchentao/ruanjian/Steam/config/stplug-in"]   # ❌ 作者机器路径
必改成本机真实路径,例:

toml
2 lines
Copy
[lua]

paths = ["D:/Program Files (x86)/Steam/config/stplug-in"]
不改 → OST 读空 → 库空。每台新电脑装完内核都要查这个文件。

快速核对
新电脑上跑 skill 诊断,5 项一键查:

bash
1 line
Copy
bash /c/Users/Administrator/.claude/skills/fluentinstall-trouble/diag.sh
会自动报 toml 路径对错 + 缺哪个 dll。

记两件
内核 = OST,装一次管所有游戏。中途想入库新游戏,只入库 + 重启 Steam 即可,不用重装内核。
若某天老 SteamTools(驱动版)游戏不显示——那是另一套东西,别混用,会内核冲突。用 Fluent 就用它的 OST 内核。
name: fluentinstall-ost
description: FluentInstall (Steam 入库) 排查 skill 位置 + 核心踩坑：安装内核会把 opensteamtool.toml 写回作者路径导致库空
type: project
---

用户在 Windows 机器上跑 FluentInstall v3.1.4（Steam 入库工具）。它自带的"SteamTools 内核"实际是 OpenSteamTool(OST)。Steam 装于 `D:\Program Files (x86)\Steam`。

**核心踩坑（2026-09-05 实测）**：FluentInstall「安装内核」有 bug，会把 `Steam\opensteamtool.toml` 写成作者残留路径 `D:/zhouchentao/ruanjian/Steam/config/stplug-in`（还会建空骨架目录）。OST 只读 toml 指定目录 → 库空。任何内核重装/更新后游戏全不显示，先查这个文件。

**Why**: 安装内核流程硬编码 dev 路径，不检测用户 Steam 安装位置。

**How to apply**: 完整排查剧本 + 一键诊断脚本在 skill `fluentinstall-trouble`（`C:\Users\Administrator\.claude\skills\fluentinstall-trouble\`）。用户提到"入库没游戏/FluentInstall/SteamTools 不生效"时，直接跑 `bash /c/Users/Administrator/.claude/skills/fluentinstall-trouble/diag.sh` 或调 skill。普通库功能问题（游戏名搜索）也可能触发，读取诊断态：`config/stplug-in/*.lua` 在不在、内核 3 dll 全不全、toml 路径对不对、Steam 钩子加载没。

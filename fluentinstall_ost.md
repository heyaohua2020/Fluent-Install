---
name: fluentinstall-ost
description: FluentInstall (Steam 入库) 排查 + 核心踩坑：安装内核会把 opensteamtool.toml 写回作者路径导致库空
type: project
---

## 新电脑标准流程

1. 装 FluentInstall
2. 设置 → 净化 steam
3. 设置 → 安装内核 DLL（自动部署 OpenSteamTool.dll + dwmapi.dll + XInput1_4.dll 到 Steam 根目录）
4. 核对 Steam\opensteamtool.toml 路径 ← **新电脑必踩坑**
5. 入库
6. 完整重启 Steam
7. 看库

**第 4 步最关键**

安装内核在新电脑同样会把 toml 写成作者残留路径：

```toml
[lua]
paths = ["D:/zhouchentao/ruanjian/Steam/config/stplug-in"]   # ❌ 作者机器路径
```

必改成本机真实路径，例：

```toml
[lua]
paths = ["D:/Program Files (x86)/Steam/config/stplug-in"]
```

不改 → OST 读空 → 库空。**每台新电脑装完内核都要查这个文件。**

## 快速核对

新电脑上跑 skill 诊断，5 项一键查：

```bash
bash /c/Users/Administrator/.claude/skills/fluentinstall-trouble/diag.sh
```

会自动报 toml 路径对错 + 缺哪个 dll。

## 记两件

- **内核 = OST**，装一次管所有游戏。中途想入库新游戏，只入库 + 重启 Steam 即可，不用重装内核。
- 若某天老 SteamTools（驱动版）游戏不显示——那是另一套东西，别混用，会内核冲突。用 Fluent 就用它的 OST 内核。

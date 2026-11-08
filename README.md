<div align="center">

# 🛡 AegisGuard

### Account & Inventory Protector for Roblox

**Stops malicious scripts from touching your inventory, Robux, tokens, and privacy.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Roblox](https://img.shields.io/badge/Roblox-Client-red)](https://www.roblox.com)
[![Lua](https://img.shields.io/badge/Lua-5.1-blue)](https://www.lua.org)
[![Version](https://img.shields.io/badge/version-3.0-brightgreen)]()

</div>

---

## 📖 What is this?

You run a script that promises **"free Robux,"** *or **"auto-farm"** — and inside sits malware.

It reads your `.ROBLOSECURITY` token, sends requests to third-party servers, calls `PerformPurchaseV2` and `PromptRobloxPurchase`, steals items through trade Remotes, takes screenshots of your screen and sends them to the "author."

**AegisGuard intercepts these mechanisms before they fire.**

You don't wake up with an empty inventory and no Robux.

---

## ⚡ Quick start

```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/DevWorksX/AegisGuard/main/main"))()

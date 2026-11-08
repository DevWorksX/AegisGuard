# AegisGuard — Account & Inventory Protector

A runtime patcher for the Roblox client that stops malicious scripts — disguised as "cheats" and "hacks" — from getting to your account, inventory, and Robux.

You run a script that promises "free Robux," "noclip cheat," or "auto-farm" — and inside sits malware. It reads your token files, sends requests to third-party servers, calls `PerformPurchaseV2` and `PromptRobloxPurchase`, steals items through trade Remotes, takes screenshots and sends them to the "author." **AegisGuard intercepts these mechanisms before they fire**, so you don't wake up with an empty inventory and no Robux.

---

## Installation

```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/YOUR_REPO/AegisGuard/main/AegisGuard.lua"))()

What it protects
Your items and Robux
Blocks PerformPurchaseV2, PromptRobloxPurchase, PromptThirdPartyPurchase — a malicious script can't complete a purchase or push a payment window.

Blocks ReportAbuseV3 — protection from mass false reports against your account.

Blocks GetRobuxBalance — malware can't find out how much Robux you have.

Your files and tokens
Blocks readfile, writefile, appendfile, delfile, delfolder, listfiles, makefolder, isfile, isfolder, loadfile, dofile — a malicious script can't read .ROBLOSECURITY, write an autostart, or steal other scripts' configs.

Blocks getgc, getreg, getupvalue, setupvalue, getconstants, setconstant, getprotos, getstack, setstack, getinfo — malware can't scan memory for your data.

Your privacy
Blocks TakeScreenshot and ToggleRecording — nobody screenshots or records your screen with your data.

Blocks RequestAsync, PostAsync, HttpRequestAsync, GetAsyncFullUrl, PostAsyncFullUrl, GetAsync on HttpRbxApiService, MakeRequest on OmniRecommendationsService — data doesn't leak to third-party servers.

Blocks MessageBusService — closes the channel malware can use to talk to an external server outside normal HTTP.

Blocks SendCommand, ReturnToJavaScript, EmitHybridEvent, AddCoreScriptLocal, OpenNativeOverlay — closes low-level code execution channels.

Your client from being broken
Blocks game:Load and access to MessageBusService via GetService — a malicious script can't reload the client into the state it wants.

Intercepts known malicious URLs and suspicious loadstring calls with a rate limit.

HTTP filter returns a fake 200 OK for requests to *.roblox.com (except games.roblox.com) — the script thinks "all good," but data goes nowhere.

Six layers of protection
Metamethod interception — __index and __namecall are hooked via hookmetamethod + newcclosure, hooks are invisible to getgc.

Anti-tamper — every 2 seconds it checks that getfenv is still a native C function. If someone swapped it — signal.

Self-check — every second the patcher verifies its critical functions (loadstring, handler, global key) are alive and not overwritten.

getgenv wrapper — tracks suspicious injections (*hook*, *bypass*, *exploit*).

newcclosure wrapping — critical functions are turned into C closures that can't be hooked via hookfunction.

HTTP filter — requests to *.roblox.com (except games.roblox.com) return a fake 200 OK, known malicious URLs get a safe stub.

Known limitations
AegisGuard is a fence against script kiddies, not a safe. An experienced reverser will bypass it in 10–15 minutes: unhook via getgc, find the handler in memory, restore the original loadstring.

The only real protection is server-side validation and not trusting the client. AegisGuard raises the cost of an attack and cuts off 99% of ready-made malicious scripts from Telegram channels and Pastebin, but it doesn't make you invulnerable.

If something breaks
Another script stopped working after installing AegisGuard — expected. AegisGuard blocks HTTP requests, file I/O, and dangerous metamethods globally for all executor threads. If a legitimate script needs them, it will break. This is a feature, not a bug.

Teleport between places fails — game:Load is blocked. Remove the ar=="Load" check from the __namecall hook if you need teleport.

False anti-tamper alerts every 2 seconds — some executors return a table from getfenv(9999) instead of erroring. Remove the first pcall check in the anti-tamper block, keep only the debug.getinfo check.

License
MIT — use it, fork it, improve it. Pull requests with new attack vectors are welcome.

Disclaimer
This tool is provided for defensive purposes only — to protect Roblox users from malicious scripts distributed under the guise of "cheats." The authors are not responsible for any misuse. If a script asks you to disable AegisGuard — don't run it.

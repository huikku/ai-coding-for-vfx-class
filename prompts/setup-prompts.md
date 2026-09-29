# Setup prompts

*AI Coding for VFX Artists · John Huikku*

Paste one of these into Claude Code, Codex or Antigravity (desktop app is
fine). The assistant works out whether you're on Windows, Mac or Linux and does
the installing for you. **It will ask before installing anything. Read what it
says, then approve.**

Do them in order the first time. Each one is safe to re-run.

---

## 0. Check my machine (read-only, nothing changes)

```
Check my computer and tell me, in plain language:
- which operating system and version I'm on
- whether these are installed, and which version: git, GitHub CLI (gh),
  Python, uv, ffmpeg (and whether it has zscale), Node.js, Blender
Don't install or change anything. Just give me a short table and tell me which
of the setup steps I still need.
```

## 1. Install ffmpeg

```
Install ffmpeg on this computer so I can convert video.
Use the normal package manager for my system (winget on Windows, Homebrew on
Mac, apt or dnf on Linux). I need a full build that includes zscale for HDR
tone-mapping.
Ask me before installing anything, explain each step in one line, and tell me
how to uninstall it. When you're done, prove it works by running
ffmpeg -version and checking zscale is listed.
```

## 2. Python and uv

```
Set me up to run small Python tools: install uv (it manages Python for me).
Ask before installing, explain each step in one line, and tell me how to
uninstall. Then prove it works by running a one-line Python "hello" with uv.
```

## 3. Git, GitHub and my first private repo

```
Help me set up GitHub:
1. Install git and the GitHub CLI (gh) if they're missing.
2. Sign me in to GitHub with gh (I'll do the browser step when you tell me).
3. Turn this folder into a PRIVATE GitHub repo. Add the .gitignore from the
   starter pack so media, caches and keys are never committed.
4. Make the first commit and push it.
Explain each step in one line, as if I've never used git. Ask before
installing anything. At the end, show me the repo's URL and confirm it's
private.
```

## 4. Install the starter pack into a project folder

```
I have the class starter pack at: <PASTE THE STARTER-PACK FOLDER PATH>
Copy these into this folder, without overwriting anything that's already here:
AGENTS.md, CLAUDE.md, .gitignore, the .claude/skills folder, the .agents/skills
folder and the prompts folder.
Show me the list of what you'll copy first (dry run), then do it when I say yes.
Then read AGENTS.md and tell me in 3 lines what rules you'll follow.
```

## 5. MCP for Blender (formerly Blender-MCP)

```
Set up "MCP for Blender" (github.com/ahujasid/mcp-for-blender) so you can
control Blender for me.
- Check the project's current README for the install steps for THIS tool
  (Claude Code, Codex or Antigravity) and my operating system.
- It needs uv, and a Blender add-on that I install inside Blender. Walk me
  through the Blender part step by step.
- Turn on its safe mode if the README offers one.
- Ask before installing anything. When it's done, prove it works by asking
  Blender for the names of the objects in the current scene.
```

## 6. Node.js (for web apps and Tauri)

```
Install Node.js LTS so I can build small web apps. Use the normal package
manager for my system, ask before installing, and prove it works with
node --version and npm --version.
```

## 7. Tauri (desktop apps)

```
Set my computer up to build a Tauri desktop app.
Check the current Tauri prerequisites page for my operating system (Rust, plus
Windows: Microsoft C++ Build Tools and WebView2; Mac: Xcode Command Line
Tools; Linux: the listed system packages).
Tell me what's missing and how big the downloads are, ask before installing,
and warn me that the first build is slow. Prove it works by creating and
running the Tauri starter template with React, Vite and TypeScript.
```

## If something goes wrong

```
That didn't work. Here's the error: <PASTE IT>
Explain what went wrong in one plain sentence, then fix it. Don't try more
than two approaches without checking with me.
```

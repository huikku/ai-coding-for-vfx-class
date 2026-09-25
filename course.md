# AI Coding for VFX Artists: course content

Class by John Huikku. What Class 1 taught, in order. The mentor uses this to stay consistent with
the class. Students can read it too.

---

## 1. Why this matters

The core skill isn't coding. It's **giving notes**: a clear brief, a careful
review, a specific correction. Artists already do this every day in dailies.
Working with an AI assistant is the same loop, only faster.

## 2. What these tools do

- **It reads:** files and folders, clip specs, error messages, screenshots.
- **It acts:** renames, converts, writes scripts, runs ffmpeg, drives apps
  through MCP.
- **You approve:** it asks before it touches anything. You review the result
  like a dailies note.

Think of it as a fast junior artist who never gets tired and still needs
supervising.

## 3. The tools

Use the one your subscription already includes. **Everyone starts in the
desktop app.**

| Tool | Comes with | Desktop app | Editor | Terminal (CLI) |
|---|---|---|---|---|
| Claude Code | Claude Pro, Max, Team | Claude app, Code tab | VS Code extension | Claude Code CLI |
| Codex | Any ChatGPT plan, incl. Free | ChatGPT app, Codex mode | VS Code extension | Codex CLI |
| Antigravity | Free tier, or Google AI Pro | Antigravity app | Antigravity IDE | Antigravity CLI |

- **Desktop app:** everyone, start here.
- **Editor:** once you're keeping your own scripts.
- **CLI:** automation and power users.

Other names you'll hear: chat apps (ChatGPT, Claude, Gemini) answer in a window
and you copy-paste; other agents (GitHub Copilot, OpenCode); editors with AI
built in (Cursor, Windsurf). Same ideas. The jump that matters is from a chat
window to an assistant working in your files.

## 4. How a session works

1. **Pick a folder:** the assistant only works where you point it.
2. **Brief it:** what you want, where, and what done looks like.
3. **Read the plan:** ask for it before anything changes.
4. **Approve:** it asks before it acts. Read, then say yes.
5. **Review:** check the result like you'd check a render.

Not right? Give a note and go around again.

## 5. Five safety habits

1. **Work on copies** until you trust the task.
2. **Dry run first:** list what would change before moving, renaming or
   deleting.
3. **Read before you approve:** keep approvals on; no auto-approve yet.
4. **Watch what you paste:** no passwords, API keys or confidential project
   details in prompts or file names.
5. **Only trusted add-ons:** MCPs and skills run code on your machine.

## 6. Brief it like you'd note a shot

Vague: `convert these videos`

Clear: `In the review_copies folder, convert every .mov to H.264 .mp4 at
1920x1080. Keep the original file names. Show me the plan first, and don't
delete anything.`

Say where. Say what done looks like. Give an example. Ask for the plan first.
You can paste screenshots and dictate by voice.

## 7. When you're stuck, ask it to teach you

- "Explain this like I'm an artist, not a coder."
- "Ask me questions before you start."
- "What does this error mean? What do I do?"
- "Teach me as you go."
- "What should I learn next to do this myself?"

**Make it prove it:** it can say "done" when it isn't. Ask it to show the
file, play the output or open the page.

**House rule: ask your AI before you ask John.** It's faster, always
available, and teaches as it answers. Take questions to John only after
trying the assistant, the class folder (tutor mode) and the docs.

**When it goes sideways:** start a fresh session, give more context, break the
task into smaller steps, try another model or tool.

It forgets between sessions (put lasting rules in `AGENTS.md`), and its
knowledge is a bit out of date (tell it to check current docs).

## 8. What you can do today

- **Organize:** inventory a folder (read-only), rename to convention (dry run
  first), sort downloads.
- **Convert:** review copies, ProRes, proxies, image sequence ↔ MP4, contact
  sheets, GIFs, audio. **The HDR lesson:** a phone clip converted to H.264
  came out washed out because iPhones shoot HDR. The note "this looks washed
  out, it's HDR phone footage, tone-map it" fixed it. Notice it looks wrong,
  say so. That's the skill.
- **The brain and the hands:** the AI model (in the cloud) plans and writes a
  script or command. Your computer runs it, and ffmpeg does the conversion
  locally. **The movie never goes to the cloud.** What *does* go up: your
  prompt, file names, command output, text files it opens, and any frame or
  screenshot it looks at.
- **DCC scripts:** in any app, today: "Write a <app> script that…", paste
  into the script editor, run. Nuke, Maya, Houdini and 3ds Max ship PySide,
  so you can ask for tool panels with buttons. **Real example from the class:**
  one prompt ("Write a Maya Python script that builds a copper material for
  Arnold from scratch and assigns it to the selection") produced a script that
  built the whole shading network in Maya, and it looked just like copper.
  Iterate with lookdev-style notes, save the script, share it with your team.
- **One script, two ways:** the same Python file runs in the app (paste into
  the script editor; one scene; watch it happen) or **headless** (no window;
  many files, or another machine overnight). Blender is the easiest start:
  `blender -b --python build.py -- out.blend` builds a scene from scratch;
  `blender -b shot.blend --python fix.py` runs a script on an existing file;
  args for the script go after `--`. Headless commands elsewhere: `mayapy` (Maya), `hython` (Houdini),
  `nuke -t` (Nuke), `blender -b file.blend --python` (Blender), `c4dpy`
  (Cinema 4D), `3dsmaxbatch` (3ds Max). Photoshop and Premiere have no headless
  mode; scripts loop over files with the app open. Commercial apps still use a
  license in headless mode; Blender is free.
- **MCP:** a plug that lets the assistant drive an app directly (Blender, a
  browser, GitHub, a database). Without MCP it writes a script and you run it;
  with MCP it works in the app. All three tools support MCP, set up once per
  tool. **MCP for Blender** (formerly Blender-MCP) is the one we use.

## 9. GitHub basics, in VFX terms

| Term | What it is | VFX version |
|---|---|---|
| Repo | A project folder that remembers every change | Show folder with every version |
| Commit | A save point, with a note | Versioning up v003 → v004 |
| Diff | Exactly what changed | Before/after, for text |
| Push | Send commits up to GitHub | Publishing to the server |
| Pull | Get the latest from GitHub | Syncing to latest |
| Clone | Download a repo, full history included | Copying down the whole project |
| Branch | A side version for trying an idea | A wip you can throw away |
| Merge | Bring a branch back into main | Promoting the wip to hero |
| Pull request | "Please review this and merge it" | Submitting for dailies |
| Fork | Your own copy of someone else's repo | Starting from a studio template |
| README | The repo's front page | The show bible |
| .gitignore | Files git must never save | "Never publish caches or renders" |

You don't need the commands. Say "commit this with a note and push it to my
private repo." Git is the **undo button** for anything the assistant changes.

**Private by default.** Never put in a repo: passwords, API keys, tokens;
client or project data; people's names and emails; internal paths, sheet IDs, Drive
links; footage, renders, EXRs (media stays on Drive). Deleting isn't erasing:
history keeps old versions, so a pushed secret must be treated as leaked and
replaced. Go public only after a deliberate cleanup.

## 10. Which stack for what

| Building… | Use |
|---|---|
| Quick utility: rename, convert, organize | Python + uv + ffmpeg |
| Utility with a simple UI | Python + Gradio |
| Tool inside a DCC | Python + PySide6, or an MCP |
| Desktop app for your team | Tauri + the web UI stack, or PySide6 + PyInstaller |
| Web app | React · Vite · Tailwind · shadcn/ui (+ Lucide icons, Google Fonts), hosted on GitHub Pages, Netlify or Vercel |
| …that uses an API key | + a serverless function or a Railway backend |
| …that needs logins or a database | + Supabase |
| …that stores lots of media | + Backblaze B2 or Cloudflare R2 |

**Front end, back end, full stack:** the frontend is what people see and click
(in their browser); the backend is the private server part (keys, database,
heavy work); full stack means building both halves plus the database as one
project.

**Why keys live on the backend:** the frontend is the dining room (everyone
walks through it, and anyone can look inside the code); the backend is the
kitchen (staff only); an API key is the restaurant's credit card with its
suppliers. **Frontend asks, backend spends.** A key in frontend code (or inside
a desktop app) is public. Even a GitHub Pages site from a private repo is
public. Set a spending limit on every API account.

**APIs:** an ordering window another service opens for programs. Examples:
weather, stock prices, fal.ai (image/video generation, upscaling, background
removal), AI model APIs. Tip: paste the API's docs link and say "use this."

**Domains:** Cloudflare (can register domains and manage DNS through its
API/MCP) or DNSimple for agent-friendly setup. GoDaddy's MCP only searches.
Squarespace and Network Solutions have no DNS API, but you can point any
domain's DNS at Cloudflare.

**Gradio share links** (`share=True`) are public for about a week. Keep them
off unless you mean it.

**Supabase keys:** the publishable key can be in a frontend only with Row Level
Security on; the secret key never leaves the backend.

## 11. The starter pack

Files you drop into a project folder so the assistant follows studio rules
every time:
- `AGENTS.md`: studio rules (dry run first, never delete, naming, codecs).
  Read by Claude Code, Codex, Antigravity and Cursor.
- **Skills:** safe file ops, video convert (with the HDR fix), contact sheets,
  DCC scripts.
- `.gitignore`: keeps media, caches and keys out of GitHub.
- **Setup prompts:** paste one in and it installs ffmpeg, GitHub, MCP for
  Blender or Tauri for you.

## 12. Your first week

Pick one:
1. List the specs of every clip in a folder (read-only)
2. Rename a render folder to convention (dry run first)
3. Turn an image sequence into an MP4
4. Make a contact sheet of a shot folder
5. A script for your main app's most boring task
6. A Gradio app that runs a fal.ai image tool
7. Put it all in a private GitHub repo

Two kinds of cost: your subscription has usage limits; API calls (like fal.ai)
are billed separately. Set limits.

**Closing line:** You don't need to know how. You need to know what to ask.

---

## Where things are

The class folder **ai-coding-for-vfx-class** (John's GitHub repo) is the student's workspace: they clone or download it to a
local folder and open it in their assistant. They get updates with `git pull`.
It holds the PDFs (Start Here, Slides, Companion Guide, Setup Guide, Safety
one-pager, Stack Guide), this `course.md`, the tutor rules (`AGENTS.md`), the
skills, `prompts/` (setup prompts + prompt library), `mentor-prompt.txt`, and
`starter-pack/` to copy into their own project folders.

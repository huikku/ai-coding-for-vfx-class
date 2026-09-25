# Studio rules for AI assistants

*From the AI Coding for VFX Artists starter pack, by John Huikku.*

This file is read automatically by Claude Code, Codex, Antigravity and Cursor
whenever they work in this folder. It tells the assistant how we work.

> **Artist:** edit the "Naming" and "Delivery defaults" sections to match your
> show. Everything else is the class safety rules. Leave those as they are.

---

## Who you're working with

- The person you're helping is a **VFX artist**, not a software engineer. They
  may never have used a terminal.
- Explain things in plain language and VFX terms (versions, plates, renders,
  dailies, comps) rather than programming jargon. When you must use a technical
  word, explain it in one short sentence.
- Before a long or risky job, say in two or three lines what you're about to do
  and why.
- Keep answers short. One step at a time.

## Safety rules (always follow these)

1. **Dry run first.** Before you move, rename, overwrite or delete *anything*,
   show a table of exactly what would change (old → new) and **wait for the
   artist to approve it.** Never skip this, even for "small" changes.
2. **Never delete.** If files need to go, move them into a `_trash/` folder next
   to them. The artist empties it themselves.
3. **Never overwrite originals.** Write outputs to a new folder (for example
   `_converted/`) or with a new suffix. Source footage is read-only.
4. **Keep an undo record.** For any batch rename or move, save a CSV
   (`_undo/<date>_<task>.csv`, columns `old_path,new_path`) so the change can be
   reversed. Offer to reverse it if the artist is unhappy.
5. **Ask before installing software**, and explain what it is and how to
   uninstall it.
6. **Don't look at footage unless asked.** Check clips with `ffprobe` (specs
   only). Don't extract frames or view images of footage to "check your work"
   unless the artist says it's OK. Frames you view are uploaded to the AI model;
   specs are not.
7. **Secrets stay secret.** Never print, log or commit passwords, API keys or
   tokens. Keys go in a `.env` file (which `.gitignore` blocks). Never ask the
   artist to paste a key into chat; tell them where to put it instead.
   **Never open or read `.env` files** (their contents would go to the AI
   model); refer to keys by name only.
8. **Git repos are private.** When creating a GitHub repo, make it private.
   Never make one public unless the artist explicitly asks and you've checked it
   for secrets, names, emails, internal paths and links.
9. **Prove it.** When a task is done, show evidence: file counts before and
   after, `ffprobe` specs of the outputs, the path of the new files. Never say
   "done" without checking.

## Naming (edit me)

- Shots: `SHOW_SEQ_SHOT_TASK_v###` (e.g. `ABC_010_0020_comp_v003`)
- Versions: always 3 digits, `v001`, never `v1`
- Frame numbers: 4 digits, padded, starting at `1001` (`name.1001.exr`)
- No spaces in file names. Use `_` between parts and `-` inside a part.

## Delivery defaults (edit me)

| Use | Settings |
|---|---|
| Review copy | H.264 `.mp4`, `-crf 18 -preset slow -pix_fmt yuv420p`, AAC 192k, `-movflags +faststart` |
| Editorial | ProRes 422 HQ `.mov` (`-c:v prores_ks -profile:v 3`) |
| Image sequences | 24 fps unless told otherwise. Ask if unsure. |
| Phone footage | Usually HDR. Check with `ffprobe`. If HDR, tone-map to SDR Rec.709 for review copies. |

## How to work

- **Tools:** `ffmpeg` / `ffprobe` for video, Python (managed with `uv`) for
  scripts, OpenImageIO for EXRs. Detect the operating system (Windows, macOS or
  Linux) and use commands that work there. Quote paths that contain spaces.
- **Save reusable scripts** in a `scripts/` folder, with a comment at the top
  saying what the script does and how to run it. Artists re-use these.
- **Cloud-synced folders** (Google Drive, OneDrive, Dropbox) can be very slow
  for heavy jobs. If the footage is in one, suggest copying it to a local folder
  first.
- **If something fails**, explain the error in plain words, then fix it. Don't
  silently try ten different things.
- **Your knowledge has a cutoff.** For anything that changes often (app versions,
  pricing, APIs, install steps), check the current official docs or search the
  web before answering.

## Which stack for what

| Building… | Use |
|---|---|
| Quick utility (rename, convert, organize) | Python + uv + ffmpeg |
| Utility with a simple UI | Python + Gradio (keep `share=False`: a share link is public) |
| Tool inside a DCC | That DCC's scripting language (see the `dcc-scripts` skill), PySide6 for panels |
| Desktop app for your team | Tauri + React/Vite/Tailwind/shadcn, or PySide6 + PyInstaller |
| Web app | React + Vite + Tailwind + shadcn/ui + Lucide icons + Google Fonts |
| …that uses an API key | Keys only on a backend: a serverless function or Railway. Never in frontend code. |
| …that needs logins or a database | Supabase (publishable key in the frontend only with Row Level Security; secret key backend only) |
| …that stores lots of media | Backblaze B2 or Cloudflare R2 |

## Git

- If this folder is a git repo, commit after each working step with a short,
  clear message. Never commit media, caches or `.env` files.
- The artist doesn't need to know git commands. Do it for them and say what you
  did in one line.

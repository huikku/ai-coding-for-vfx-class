# Prompt library

*AI Coding for VFX Artists · John Huikku*

Copy, paste, edit the parts in `<ANGLE BRACKETS>`. Works in Claude Code, Codex
and Antigravity. The starter-pack `AGENTS.md` already tells the assistant to
dry-run, never delete and prove its work, but saying it again never hurts.

---

## Getting unstuck & learning

```
Explain this like I'm a VFX artist, not a programmer.
```
```
Ask me questions before you start.
```
```
What does this error mean, and what should I do? <PASTE ERROR>
```
```
Teach me as you go: explain each step in one line before you do it.
```
```
What should I learn next so I can do this myself?
```
```
This isn't working. Stop, tell me what you've tried, and suggest a simpler way.
```
```
Before you start, check the current official docs for <TOOL>. Your
knowledge might be out of date.
```

## Prove it

```
Prove it's done: show me the output files, their specs, and the file counts
before and after.
```
```
Check the outputs with ffprobe only. Don't open or view any frames.
```

---

## Organize

**Inventory (read-only, a perfect first task):**
```
List every video and image sequence in <FOLDER> with its duration or frame
range, resolution, frame rate and codec. Treat numbered frames as one
sequence. Save it as a CSV and give me a summary. Don't change anything.
```

**Rename to convention:**
```
Rename the renders in <FOLDER> to <SHOW>_<SHOT>_v###.<ext>. Keep frame
numbers intact. Show me a dry-run table of old → new names first and wait
for my OK. Save an undo file.
```

**Sort a messy folder:**
```
Sort <FOLDER> into subfolders: plates, references, renders, audio, other.
Show me the plan first. Move nothing until I approve. Never delete.
```

**Find problems:**
```
Check the image sequences in <FOLDER> for missing frames, duplicate frames
and inconsistent resolutions. Report only, don't fix anything.
```

**Tidy versions:**
```
In <FOLDER>, find every shot that has more than 3 versions. Move all but the
latest 3 into _trash/, after showing me the list and getting my OK.
```

---

## Convert

**Review copies:**
```
Convert every .mov in <FOLDER> to an H.264 .mp4 review copy. Put them in
_converted/. Check each clip first. If any are HDR phone footage,
tone-map them to normal Rec.709. Prove it with ffprobe at the end.
```

**ProRes for editorial:**
```
Make ProRes 422 HQ copies of the clips in <FOLDER> for editorial, into
_converted/. Keep the audio.
```

**Image sequence → MP4:**
```
Turn the image sequence in <FOLDER> into an MP4 at <24> fps. If it's EXR,
make a quick sRGB preview and tell me it's a preview transform.
```

**Proxies:**
```
Make half-resolution H.264 proxies of everything in <FOLDER>, same names
with _proxy on the end.
```

**Contact sheet:**
```
Make a contact sheet of every clip in <FOLDER>: middle frame of each, 5 per
row, file name under each. Save it as a JPG.
```

**GIF for chat:**
```
Make a small GIF (640 wide, 12 fps) of <CLIP> from <00:02> to <00:06>.
```

**Watch folder:**
```
Write me a small tool that watches <FOLDER> and automatically makes an H.264
review copy of any new .mov that lands in it. Explain how to start and stop it.
```

---

## DCC scripts

Paste the result into the app's script editor (the assistant will tell you
where). Ask for `DRY_RUN = True` on anything that renames or deletes.

**Blender**
```
Write a Blender Python script that sets up a three-point light rig and a
turntable camera orbiting the selected object over 120 frames.
```

**Maya**
```
Write a Maya Python script that renames the selected objects to
<prefix>_### with padded numbers, as one undo step.
```

**Maya: build a shader from scratch**
```
Write a Maya Python script that builds a copper material for Arnold from
scratch (aiStandardSurface, physically plausible copper color, metalness and
roughness) and assigns it to the selected objects. Name the nodes clearly.
```

**Nuke**
```
Write a Nuke Python script that sets every selected Read node's frame range
to the project range and reports any Read with missing files.
```

**Houdini**
```
Write a Houdini shelf tool (Python) that creates a camera and a basic
three-light rig looking at the selected object.
```

**Cinema 4D**
```
Write a Cinema 4D Python script that puts every selected object under a new
null named after the first object, with undo.
```

**3ds Max**
```
Write a 3ds Max Python (pymxs) script that lists every material in the scene
and which objects use it, and saves the list to a text file.
```

**After Effects**
```
Write an After Effects script (.jsx) that lists every comp's name, size,
duration and frame rate in a text file.
```

**Photoshop**
```
Write a Photoshop script that exports every layer group as its own PNG named
after the group.
```

**Premiere Pro**
```
What scripting does Premiere Pro <VERSION> support? Then write a script that
lists every clip in the active sequence with its in/out timecodes.
```

**DaVinci Resolve**
```
Write a DaVinci Resolve Python script (for Workspace → Console) that adds a
marker with the clip name at the start of every clip on video track 1.
Tell me if this needs Resolve Studio.
```

**Blender headless: build scenes from scratch**
```
Write a Blender Python script I can run headless (blender -b --python) that
builds a turntable scene from scratch: a copper sphere, a three-point light
rig, a camera orbiting over 120 frames, then saves it to the path I pass
after --. Show me the exact command to run it.
```

**Batch: run a script over many scenes**
```
Turn this script into a batch tool: run it without opening <APP> (batch or
headless mode) on every scene file in <FOLDER>, save each result as a new
version (never overwrite), and write a report of what changed. Do a dry run
on 2 files first.
```

**Tool panel (Maya / Nuke / Houdini / 3ds Max)**
```
Make me a PySide panel for <APP> with buttons for these three tasks:
<TASK 1>, <TASK 2>, <TASK 3>. Explain how to install it so it's always there.
```

**MCP (drive the app directly, once set up)**
```
In Blender, add a 3-point light rig and a turntable camera around the
selected object, then render a clay preview to <FOLDER>.
```

---

## GitHub

```
Turn this folder into a private GitHub repo with the starter-pack .gitignore
and push it. Explain each step in one line.
```
```
Commit this with a short note about what we changed, and push it.
```
```
Make a branch called <try-idea> so we can experiment without touching the
working version.
```
```
Undo what you did in the last commit.
```
```
Before I make this repo public, check every file and the whole git history
for keys, passwords, emails, names, internal paths and links. Report only.
```

---

## Small apps & APIs

**First Gradio tool:**
```
Build a small local Gradio app with uv: I drop in a video, it shows the
specs and gives me a button to make an H.264 review copy. Keep share off.
```

**First API project:**
```
Build a tiny web page that shows the weather for <CITY>, using a free
weather API. Keep the API key on a small backend, not in the page. Tell me
where to put the key; don't ask me to paste it here.
```

**fal.ai image tool:**
```
Build a local Gradio app that sends a prompt to fal.ai and shows the
generated image. Read my FAL_KEY from a .env file. Before you start, check
fal.ai's current docs for the model and Python client.
```

**Web app starter:**
```
Create a new web app with React, Vite, TypeScript, Tailwind, shadcn/ui and
Lucide icons, in a private GitHub repo. Add a Railway backend that holds my
API keys. Explain the folder structure in 5 lines.
```

**Desktop app starter:**
```
Create a Tauri desktop app with React, Vite, Tailwind and shadcn/ui that
lists the videos in a folder I pick and shows their specs. Bundle ffmpeg
with it.
```

---

## Privacy

```
Do this without viewing any frames or images. Use file specs only.
```
```
Don't include file paths or names in anything you save to the repo.
```

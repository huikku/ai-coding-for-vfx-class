---
name: dcc-scripts
description: Write scripts and tool panels for VFX apps (Blender, Maya, Nuke, Houdini, Cinema 4D, After Effects, 3ds Max, Photoshop, Premiere Pro, DaVinci Resolve) that the artist pastes into the app's script editor or installs as a shelf/menu tool. Use for any "write me a script for <app>" request.
---

# Scripts for DCC apps

## 1. Ask two things first (if not already known)

- **Which app and version?** APIs change between versions.
- **What's selected / what should it act on?** Selected objects, a whole scene,
  a folder of files?

## 2. Use the right language and tell them where to paste it

| App | Language | Where to run it |
|---|---|---|
| Blender | Python (`bpy`) | Scripting workspace → Text Editor → Run Script |
| Maya | Python (`maya.cmds`) | Windows → General Editors → Script Editor → Python tab |
| Nuke | Python (`nuke`) | Script Editor panel |
| Houdini | Python (`hou`), VEX for geometry | Windows → Python Shell, or a new Shelf Tool |
| Cinema 4D | Python (`c4d`) | Extensions → Script Manager |
| 3ds Max | Python (`pymxs`) or MAXScript | Scripting → Run Script / MAXScript Listener |
| After Effects | ExtendScript (`.jsx`, JavaScript) | File → Scripts → Run Script File |
| Photoshop | ExtendScript (`.jsx`) or UXP | File → Scripts → Browse |
| Premiere Pro | ExtendScript or UXP (depends on version) | Check the version's scripting options first |
| DaVinci Resolve | Python or Lua | Workspace → Console (some scripting needs Resolve Studio) |

If you're not sure what the artist's version supports, say so and check the
app's current docs rather than guessing.

## 3. Write it safely

- **Wrap changes in one undo step** so a single Ctrl/Cmd+Z reverts the whole
  script: Maya `cmds.undoInfo(openChunk=True)` … `closeChunk`; Nuke
  `nuke.Undo.begin()` … `end()`; Houdini `with hou.undos.group("name"):`;
  Blender operators are undoable, and direct data edits can be undone with
  `bpy.ops.ed.undo_push()`; C4D `doc.StartUndo()` … `EndUndo()`; AE
  `app.beginUndoGroup()` … `endUndoGroup()`.
- **Act on the selection** by default, and stop with a clear message if nothing
  is selected.
- **Preview option:** for anything that renames or deletes, include a
  `DRY_RUN = True` flag at the top that only prints what would change.
- Print a short summary at the end ("Renamed 12 objects").
- Comment the top of the script: what it does, which app/version, how to run it.

## 4. Save it

Save the script in `scripts/<app>/` with a clear name so the artist can re-use
it and share it with their team.

## 5. If it errors

Ask the artist to paste the full error from the script editor, explain it in
one plain sentence, then fix it.

## Batch mode: run it over many files

When the artist wants the same script applied to many scene files, make it
run without the UI:

| App | Run a script without opening the app |
|---|---|
| Maya | `mayapy my_script.py` (start the script with `maya.standalone.initialize()`) |
| Houdini | `hython my_script.py` |
| Nuke | `nuke -t my_script.py` |
| Blender | `blender -b scene.blend --python my_script.py` |
| Cinema 4D | `c4dpy my_script.py` |
| 3ds Max | `3dsmaxbatch my_script.ms` (MAXScript or Python) |
| After Effects | `aerender` for batch renders; scripts run inside the app |
| Photoshop, Premiere | No headless mode; a script loops over files while the app is open |
| Resolve | External scripting (Resolve Studio) drives the running app |

**Blender specifics** (tested with 4.2 LTS):
- Build from scratch: `blender -b --python build.py -- out/scene.blend`
  (start the script with `bpy.ops.wm.read_factory_settings(use_empty=True)`,
  end with `bpy.ops.wm.save_as_mainfile(filepath=...)`).
- Run on a file: `blender -b shot.blend --python fix.py`. The file comes
  **before** `--python`.
- Script arguments go after `--`; read them with
  `sys.argv[sys.argv.index("--") + 1:]`. Don't build paths from `//` when no
  file is open.
- Render: `blender -b shot.blend -o //renders/shot.#### -s 1001 -e 1100 -a`
  (`-o` before `-a`/`-f`; the scene needs a camera).
- Write scripts so they work **both** in the Text Editor and headless.

- Loop over the folder, **open → change → save as a new version** (never
  overwrite the original), log each file, and print a summary.
- **Dry run on 2 files first** and show the results before running the rest.
- Commercial apps still need a license in batch mode (Blender doesn't). If the command isn't found, locate the
  app's install folder rather than guessing.

## Next level: tool panels

Maya, Nuke, Houdini and 3ds Max ship PySide (Qt). For a panel with buttons, use
PySide6 (or PySide2 on older versions) and the app's own way of docking or
parenting a window. Blender uses its own UI API (`bpy.types.Panel`), not Qt.

---
*Part of the AI Coding for VFX Artists starter pack, by John Huikku.*

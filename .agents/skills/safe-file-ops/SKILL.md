---
name: safe-file-ops
description: Safely rename, move, sort, copy or clean up files and folders (renders, plates, downloads, image sequences). Use whenever a task would move, rename, overwrite or remove files. Always does a dry run and waits for approval, never deletes, and keeps an undo record.
---

# Safe file operations

Artists' files are irreplaceable. Follow these steps every time, in order.

## 1. Look (read-only)

- List what's in the target folder: counts by type, sequences detected
  (`name.1001.exr` … `name.1100.exr` is ONE sequence, not 100 files), total size.
- Say what you found in plain words. Change nothing yet.

## 2. Dry run

- Build the full plan as a table: `old path → new path` (or `→ _trash/`).
- For long lists, show the first 10 and last 5 rows plus the total count, and
  save the full plan to `_undo/<YYYY-MM-DD>_<task>_PLAN.csv`.
- **Check for problems and report them before asking to proceed:**
  - two files that would end up with the same name (collisions)
  - names that would break a sequence (frame numbers out of order or re-padded)
  - files that are open or locked, and cloud placeholder files that haven't
    downloaded yet
  - anything outside the folder the artist pointed you at
- Ask: **"Want me to go ahead?"** Wait for a clear yes.

## 3. Do it

- Rename or move only. **Never delete**: anything to remove goes into `_trash/`
  next to it.
- Never overwrite: if a target exists, stop and ask.
- Write the undo record as you go: `_undo/<YYYY-MM-DD>_<task>.csv` with columns
  `old_path,new_path`.
- Prefer a small Python script (saved in `scripts/`) over a long chain of shell
  commands, so the same job can be re-run or reversed.

## 4. Prove it

- Report: files before, files after, and anything skipped (and why).
- Spot-check 3 renamed files by listing them.
- Tell the artist where the undo record is, and that you can reverse it with
  one message: "undo the last rename".

## Undo

To reverse, read the undo CSV and move each `new_path` back to `old_path`, with
the same dry-run → approve → do → prove steps.

---
*Part of the AI Coding for VFX Artists starter pack, by John Huikku.*

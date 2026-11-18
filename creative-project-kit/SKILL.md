---
name: creative-project-kit
description: "Project memory for creative and communication work — brand, campaigns, video, content, design. Sets up and maintains an Obsidian structure (map, index, log, subprojects) so every new AI chat picks up a long project in seconds. Triggers: \"set up the project\", \"new subproject\", \"close the chat\", \"check Obsidian\"."
---

# Creative project kit for Obsidian

For creative and communication projects: a brand identity, an ad campaign, a video or animation, a content series, a website, a book. Such a project lives across many chats. Each new chat should learn what is going on in seconds by reading 2–3 small files, not the whole vault.

Four modes — pick one from the user's phrase:
- "set up the project / structure" → **A. Set up**
- "new subproject", starting a big piece of work → **B. Subproject**
- "close the chat", the chat is getting heavy → **C. Close the chat**
- "check Obsidian", weekly, before a new stage → **D. Check**

In every mode:
- Reply in the user's language. Explain things plainly, step by step, and define any technical term.
- File and folder names stay exactly as in this skill (the check script relies on them).
- Work with the vault through whatever file access the session has (a folder connected to the chat, a local shell, the local file system in Claude Code or Codex). Below, `<vault>` means the project folder.
- No direct file access (e.g. a plain web chat): produce the new or changed files as downloads or code blocks, say exactly where each one goes in the vault, and ask the user to paste back the files you need to read. The check script then runs on the user's computer.
- If the project folder is not reachable, say so in the first line and do not work "from memory".
- Never delete anything without an explicit "yes".

---

## Golden rules (why the structure looks like this)
1. **One truth, one place.** A subproject's status lives only in its `00 MAIN`; prompt phrases and settings live only in its `02 Passport`. Other files link, they don't copy. The map never retells status.
2. **Decided → written to the source file right away.** "Update later" does not count.
3. **Light start.** A project-level chat reads the map + index. A subproject chat reads its `00 MAIN` + files marked `start: yes`. Everything else is opened on demand, via the index. Limits: `00 MAIN` ≤ 10 KB, chat start ≤ 40 KB, any note > 30 KB gets split into "always read" and "read when needed" (don't rewrite the text — only move it, then check that no line was lost).
4. **One chat, one task.** When a chat gets heavy — warn and close it (mode C). Short chats are normal; moving to a new chat costs seconds.
5. **Every file is a line in the index; every chat is a line in the log.**
6. **Sources apart from conclusions.** PDFs and articles go to `Sources/`, with a "… — takeaways" note next to them.
7. **Images in subfolders**, drafts in `_drafts/`, archives outside the project folder. Run Python as `python3 -B` (it doesn't create the `__pycache__` service folder).
8. **Tool notes go stale.** Their header carries the tag `tool` and the line `status: verified YYYY-MM-DD`; before working in a tool, verify the note and rewrite what is outdated.
9. **Separate branches** (posts, side topics) get their own folder; the branch's main file has `branch: separate` in its header. A branch may read project files but writes only to its own folder.
10. **The human decides.** Never record as a decision something the user hasn't approved; the AI's suggestions are written as "proposed, not decided".

---

## A. Set up the project
1. Check that the project folder is reachable. If not — ask the user to create an empty folder in Obsidian and connect it to the chat. If the folder already has files — overwrite nothing, create only what is missing, and add existing files to the index.
2. Ask **in one message** three questions (each with a default answer):
   - project name and goal in one or two lines;
   - is there a stage-by-stage plan (then `00 Program` is needed), and which tools are used;
   - which big parts (subprojects) are already visible. None yet — create them later with mode B.
3. Create the skeleton:
   ```
   <Project>/
     00 Map.md
     00 Index.md
     00 Log.md
     00 Program.md               ← only if there is a stage plan
     Tools/                      ← notes on apps and models
     Lessons/                    ← theory, reference
     Prompts/   images/          ← ready prompt chains
     Sources/                    ← PDFs and articles + "… — takeaways"
     Templates/Obsidian check/   check.py, README.md
     <Subproject>/               ← mode B, one folder per big piece of work
   ```
4. Fill the files from the templates below ("File templates"), using the answers. Dates — today.
5. Write `Templates/Obsidian check/check.py` from the "Check script" section verbatim, plus a short README.md (what it does, how to run it, the `start: yes` and `branch: separate` markers). Run the check — it must pass with no errors.
6. Show the **text for the project instructions** as a code block, ready to copy, and say where to paste it: the project's instructions in Claude Projects, ChatGPT Projects or a similar feature. The AI can't change them itself.
   ```
   Before any work:
   1. Check that the Obsidian folder "<Project>" is connected. If not — say so in the first line and don't work from memory.
   2. For the project, first read "00 Map.md" and "00 Index.md". For a subproject — its "00 MAIN.md" and files marked "start: yes". Open anything else only when the task needs it, via the index.
   3. Obsidian is the main source. If memory or an old chat contradicts Obsidian — trust Obsidian and point out the mismatch.
   4. Once something is decided — write it to its source file right away. At the end of a chat — "close the chat" (creative-project-kit skill).
   5. One chat, one task. When the chat gets heavy — warn me.
   ```
7. First log line: `DD.MM — project structure set up → [[00 Index]]`.
8. Reply with: the tree of created folders, the instructions text, and a phrase to start the first working chat.

## B. New subproject
A subproject is a big piece of work spanning many chats (a video, a website, a campaign, a book chapter).
1. Ask for the name, what it is in two lines, and what parts it consists of (shots, pages, chapters).
2. Create:
   ```
   <Subproject>/
     00 MAIN.md           ← status, what's now, open questions (≤ 10 KB)
     01 Work rules.md     ← how one step is done, who does what
     02 Passport.md       ← style, characters, prompt phrases, settings — the only copy
     Parts/               ← one file per part (may be named by meaning: Shots/, Chapters/)
     images/  scripts/  _drafts/
   ```
   Add topic files (geography, lessons, atlas) as needed with the next numbers (03, 04…). Mark a file needed at every chat start with `start: yes` in its header. If it grows past 30 KB — put the details in a lettered file (`08` → `08b`).
3. Lines in `00 Index` (subproject section), a line in `00 Log`, and in `00 Map` under "Now" — a link to the new `00 MAIN`.
4. A phrase to start a working chat for the subproject.

## C. Close the chat
1. Folder not reachable — say so and show the summary as text so nothing is lost.
2. Collect only the **result**, not the history: what was done and approved; decisions (who decided); open questions; new rules and lessons.
3. Put each item in its place:
   - status, "now / next", open questions — the subproject's `00 MAIN`;
   - a decision that changes a rule, phrase or data — into the source file itself (Passport, Work rules, part file, program);
   - **log**: one line at the top `DD.MM — what was decided or done → [[where it's written]]`;
   - **index**: a file appeared, was renamed or deleted — fix its line;
   - chats of a separate branch write their summary only to their own folder.
4. **Write right away, without asking,** everything the user already approved. Ask for a "yes" only for: deletion; a contradiction with what's already in the files; recording something the user did not explicitly approve. If everything was already written along the way — say so in one line.
5. Run the check script (mode D, step 2). Fix technical issues, show the rest. Also make sure: the map doesn't retell status; headers and titles of touched files match their content; closed questions are removed from `00 MAIN`.
6. End with three lines: what was written and where; what is still open; **a phrase to start the next chat** and what to connect.

## D. Check Obsidian
1. Folder not reachable — say so and stop.
2. **Mechanics — by script** (read-only):
   ```
   cd "<vault>"
   python3 -B "Templates/Obsidian check/check.py" .
   ```
   No script — write it from the "Check script" section. It catches: broken links and images; files missing from the index and index lines pointing to deleted files; sizes and chat-start weight; outdated header dates; tool notes not verified for a long time; "update later" leftovers; a log that lags behind; junk.
3. **Meaning — yourself.** Read the map, index, log, and `00 MAIN` of active subprojects. Check: the map doesn't retell status; the last 5 log lines are really reflected in the files they link to; open questions contain nothing already decided; no rule or phrase is written in two places in different ways; index lines correctly describe recently changed files. Open only files the script or the log points to; don't read the whole folder.
4. **Fix right away** only technical things: an index line, a header date, a broken link with an obvious target, `__pycache__`. **Ask for a "yes"** for everything else (deletion, a contradiction in meaning, splitting a note, closing a question), one line per question. Don't touch anything inside `_to_delete/` — that's the owner's bin.
5. Log line: `DD.MM — folder check: what was fixed → [[00 Index]]`.
6. Reply: **Fixed myself** · **Needs your decision** · or one line "all good".

---

## File templates
Every file starts with a header:
```
---
tags: [...]
updated: YYYY-MM-DD
---
```

**00 Map.md**
```
# <Project> — map
**The AI reads this file first.** Next — [[00 Index]]: use it to decide what to open. What was decided and when — [[00 Log]].
## Goal
## Stages — short (details in [[00 Program]], if it exists)
## Tools
## Where things are
All notes are in [[00 Index]]. Here — only what is not in the folder (AI memory, separate chats).
## Postponed tasks — the AI reminds when it's time
## Now
Only a link to the active subproject's "00 MAIN". Status is not retold here.
```

**00 Index.md**
```
# 00 Index — where things are
← [[00 Map]] · what was decided — [[00 Log]]
**Why.** One line per note. The AI reads it after the map and opens only what's needed.
**Rules.** A file appeared or was deleted — fix its line. Folders where we don't list every file are marked like this: `folder/`.
## Project
- [[00 Map]] — goal, stages, tools. Read first.
- [[00 Index]] — this file.
- [[00 Log]] — what was decided and when.
## Tools · ## Lessons · ## Prompts · ## Sources · ## <Subproject> · ## Templates
- [[Templates/Obsidian check/README|Obsidian check]] — folder check script.
```
(each section is its own `##` heading; don't create empty sections)

**00 Log.md**
```
# 00 Log — what was decided and when
← [[00 Map]] · where each file is — [[00 Index]]
**How to keep it.** One line per closed chat, newest on top: `DD.MM — what was decided or done → [[where it's written]]`. Details live in the source file. Longer than ≈60 lines — fold lines older than a month into one line per month.
## <Month YYYY>
```

**<Subproject>/00 MAIN.md**
```
# <Subproject> — MAIN
**The AI reads this file first for the subproject.** Keep it ≤ 10 KB.
## How a chat starts
This file → files with "start: yes" → everything else only when the task needs it (via the index). One chat — one step.
## The subproject in short (3–5 lines)
## Parts — status (table: part · what · status)
## Where we are now
## Next
## Open questions (remove closed ones)
## Where things are (links to 01, 02, Parts/)
```

**<Subproject>/01 Work rules.md** — the order of one step (who does what: AI / human), what gets fixed with what, where the result is written. Don't copy communication rules (language, tone) here — they belong in the AI's memory or project instructions.

**<Subproject>/02 Passport.md** — format, style, characters, prompt phrases verbatim, model and settings. First line: "Phrases and settings live only here — other files link here."

**Tool note** — header has `tags: [tool]` and `status: verified YYYY-MM-DD`.

---

## Check script
Write verbatim to `Templates/Obsidian check/check.py`. If you improve the script inside a project — suggest updating this skill too.

~~~python
# Obsidian project folder check. Read-only, changes nothing.
# Run: python3 -B "Templates/Obsidian check/check.py" "<project folder>"
# Works in any project with this structure: 00 Map / 00 Index / 00 Log in the root,
# each subproject has its own "00 MAIN.md". Files read at every subproject chat start
# are marked in their header with the line "start: yes".
import os, re, sys, datetime as dt

ROOT = os.path.abspath(sys.argv[1] if len(sys.argv) > 1 else ".")
TODAY = dt.date.today()
SKIP_DIRS = {".obsidian", ".trash", "__pycache__", "_to_delete"}
MAIN_LIMIT = 10_000    # "00 MAIN.md"
BIG = 30_000           # any note larger than this — consider splitting
START_LIMIT = 40_000   # everything a chat reads at start
STALE_DAYS = 30        # tool notes are verified monthly
TAILS = ["update later", "TODO", "add later"]

problems, notes = [], []
def P(g, t): problems.append((g, t))
def N(g, t): notes.append((g, t))

md, allfiles = [], set()
for r, ds, fs in os.walk(ROOT):
    ds[:] = [d for d in ds if d not in SKIP_DIRS and not d.startswith(".")]
    for f in fs:
        rel = os.path.relpath(os.path.join(r, f), ROOT)
        allfiles.add(rel)
        if f.endswith(".md"): md.append(rel)
base = set()
for rel in allfiles:
    base.add(os.path.basename(rel))
    if rel.endswith(".md"): base.add(os.path.basename(rel)[:-3])

def read(rel): return open(os.path.join(ROOT, rel), encoding="utf-8").read()
def text(rel):
    s = re.sub(r"```.*?```", "", read(rel), flags=re.S)
    return re.sub(r"`[^`\n]*`", "", s)
def front(rel):
    m = re.match(r"---\n(.*?)\n---", read(rel), re.S)
    return m.group(1) if m else ""
def size(rel): return len(read(rel).encode())
def resolve(t, src):
    t = t.strip()
    if not t: return True
    for c in (t, t + ".md"):
        if c in allfiles or os.path.normpath(os.path.join(os.path.dirname(src), c)) in allfiles: return True
    return os.path.basename(t) in base

# 1. Links and images
for rel in md:
    s = text(rel)
    for m in re.finditer(r"!?\[\[([^\]|#]+)", s):
        if not resolve(m.group(1), rel): P("Broken links", f"{rel} → [[{m.group(1)}]]")
    for m in re.finditer(r"!\[[^\]]*\]\(<?([^)>]+)>?\)", s):
        t = m.group(1)
        if not t.startswith("http") and not resolve(t, rel): P("Missing image", f"{rel} → {t}")

# 2. Index: file without a line / line without a file
if "00 Index.md" not in allfiles:
    P("Index", "no 00 Index.md file")
else:
    cat = read("00 Index.md")
    folders = re.findall(r"`([^`]+/)`", cat)
    linked = set()
    for m in re.finditer(r"\[\[([^\]|#]+)", re.sub(r"`[^`\n]*`", "", cat)):
        t = m.group(1).strip()
        hits = [x for x in md if x == t + ".md" or x.endswith("/" + t + ".md") or os.path.basename(x) == t + ".md"]
        if not hits: P("Index", f"line points to a missing file: [[{t}]]")
        linked.update(hits)
    for rel in md:
        if rel not in linked and not any(rel.startswith(f) for f in folders):
            P("Index", f"file not in the index: {rel}")

# 3. Sizes and chat-start weight
for rel in md:
    sz = size(rel)
    if os.path.basename(rel) == "00 MAIN.md" and sz > MAIN_LIMIT:
        P("Size", f"{rel}: {sz//1000} KB, limit {MAIN_LIMIT//1000} KB")
    elif sz > BIG:
        N("Size", f"{rel}: {sz//1000} KB — split into 'always read' and 'read when needed'")
starts = {}
root_start = [r for r in ("00 Map.md", "00 Index.md") if r in allfiles]
if root_start: starts["project-level chat (map + index)"] = root_start
for rel in md:
    if os.path.basename(rel) == "00 MAIN.md":
        d = os.path.dirname(rel)
        extra = [x for x in md if os.path.dirname(x) == d and x != rel and re.search(r"^start:\s*yes", front(x), re.M | re.I)]
        starts[f"subproject chat '{d or 'root'}'"] = [rel] + extra
for label, files in starts.items():
    w = sum(size(r) for r in files)
    (P if w > START_LIMIT else N)("Chat start", f"{label}: {w//1000} KB (limit {START_LIMIT//1000} KB)")

# 4. Header dates and tool notes not verified for a long time
for rel in md:
    s, fm = read(rel), front(rel)
    up = re.search(r"^updated:\s*(\d{4}-\d{2}-\d{2})", s, re.M)
    chk = re.search(r"verified\s*(\d{4}-\d{2}-\d{2})", s)
    if not up and not chk:
        N("No date in header", rel); continue
    mtime = dt.date.fromtimestamp(os.path.getmtime(os.path.join(ROOT, rel)))
    if up and mtime > dt.date.fromisoformat(up.group(1)) + dt.timedelta(days=1):
        N("Header date is behind", f"{rel}: header says {up.group(1)}, file changed {mtime}")
    if re.search(r"\btool\b", fm) or rel.startswith("Tools/"):
        d = dt.date.fromisoformat((chk or up).group(1))
        if (TODAY - d).days > STALE_DAYS: N("Not verified for a while", f"{rel}: {(TODAY - d).days} days")

# 5. "Later" leftovers
for rel in md:
    if rel.startswith("Templates/Obsidian check/"): continue
    low = read(rel).lower()
    for t in TAILS:
        if t.lower() in low: N("Leftovers", f"{rel}: '{t}'")

# 6. Log
branches = [os.path.dirname(r) for r in md if re.search(r"^branch:\s*separate", front(r), re.M | re.I)]
if "00 Log.md" in allfiles:
    j = read("00 Log.md")
    if len(re.findall(r"^- ", j, re.M)) > 60: P("Log", "more than 60 lines — fold old ones by month")
    dates = re.findall(r"^- (\d{1,2})\.(\d{2})", j, re.M)
    if dates:
        d, mth = map(int, dates[0]); last = dt.date(TODAY.year, mth, d)
        if last > TODAY: last = dt.date(TODAY.year - 1, mth, d)
        newer = [r for r in md if dt.date.fromtimestamp(os.path.getmtime(os.path.join(ROOT, r))) > last
                 and not any(b and r.startswith(b + "/") for b in branches)]
        if newer: N("Log", f"last line {last}; {len(newer)} files changed after it — was the summary written?")
else:
    P("Log", "no 00 Log.md file")

# 7. Junk
for r, ds, fs in os.walk(ROOT):
    if "_to_delete" in r: continue
    if "__pycache__" in ds: P("Junk", f"__pycache__ in {os.path.relpath(r, ROOT)}")
for rel in allfiles:
    if rel.endswith(".zip") and "_to_delete" not in rel: N("Junk", f"archive in the folder: {rel}")

def out(title, items):
    if not items: return
    print(f"\n### {title}")
    g = {}
    for k, t in items: g.setdefault(k, []).append(t)
    for k, ts in g.items():
        print(f"- {k} ({len(ts)}):")
        for t in ts[:15]: print(f"    {t}")
        if len(ts) > 15: print(f"    … and {len(ts)-15} more")
print(f"Check: {len(md)} notes, {len(allfiles)} files, {TODAY}")
out("Fix", problems); out("Look at", notes)
if not problems: print("\nNothing to fix.")
~~~

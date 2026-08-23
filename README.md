# job-search skill

A Claude skill that screens live job postings against a candidate profile, reads real descriptions
instead of trusting site filters, and returns a marked-up shortlist (or keeps a tracker spreadsheet
in sync on a schedule).

**Works for any profession** — software engineer, office manager, executive assistant, nurse,
teacher, sales rep, whatever the candidate does. Nothing about a specific field is hardcoded.

This skill ships with **no personal data**. Nothing about any specific person's job search is
hardcoded — every candidate builds their own `references/profile.md` through a setup wizard the
first time they use it.

## Install

Pick whichever matches where you use Claude. All of them end up in the same place: this folder
(`SKILL.md` + `references/`) sitting inside a skills directory Claude reads from.

### Option A — Claude.ai / Cowork (easiest)

1. Download `job-search.skill` from this repo (or zip this folder yourself — see "Packaging your
   own `.skill` file" below).
2. In the chat, drop the `.skill` file in, or open it from your downloads — Claude shows a card with
   a **Save skill** button. Click it.
3. Start a new conversation and say something like "set up my job search" or "use the job-search
   skill." Since there's no `references/profile.md` yet, it'll run the setup wizard automatically.

### Option B — Claude Code via `/plugin` (recommended for Claude Code)

This repo is already set up as a self-hosted plugin marketplace (`.claude-plugin/marketplace.json` +
`.claude-plugin/plugin.json` at the root), so you can install it with two slash commands instead of
copying files by hand:

```
/plugin marketplace add <owner>/<repo>
/plugin install job-search@job-search-marketplace
```

(Replace `<owner>/<repo>` with wherever you host this — a GitHub `owner/repo`, a git URL, or a local
path all work for `marketplace add`.) Claude Code will then load `SKILL.md` automatically; no manual
file copying needed, and it stays up to date when you pull the marketplace repo.

### Option C — Claude Code, manual copy

If you'd rather not use a marketplace (e.g. you just want this one skill, no plugin bookkeeping):

1. Copy this folder into your skills directory:
   - Personal (available in every project): `~/.claude/skills/job-search/`
   - Project-only (checked into a repo, shared with teammates): `<repo>/.claude/skills/job-search/`
2. Restart Claude Code (or start a new session) so it picks up the new skill.
3. Ask Claude to "use the job-search skill" or just describe what you want ("find me office manager
   jobs in Chicago") — it'll trigger automatically and run the setup wizard on first use.

### Option D — Any other Claude surface with a skills folder

Same idea: copy `SKILL.md` and `references/profile.template.md` into wherever that surface loads
skills from (check its docs for the exact path — the folder name and structure here are standard).

### First run

However you installed it, the first time you invoke the skill it notices there's no
`references/profile.md` yet and runs a setup wizard: your profession, target roles, location,
skills/qualifications, must-have/exclude rules, which job sites to search, whether to auto-draft
cover letters, whether to track applications in a spreadsheet, and whether to schedule a daily run.
It writes your answers into `references/profile.md` and shows it to you before the first real search.
After that it just loads the profile — the wizard only runs again if you delete the profile or
explicitly ask to reconfigure.

### What you'll need connected

The skill itself has no dependencies baked in, but to actually do anything it needs, at minimum, a
way to browse job sites (e.g. a browser-automation tool like Claude in Chrome) logged into the sites
you want searched. For the optional features, you'll also want: a Google Drive/Sheets connector (or
equivalent) if you want a tracker spreadsheet and/or auto-generated cover letters, an inbox connector
(e.g. Gmail) if you want application/rejection/interview emails picked up automatically, and a task
scheduler if you want the daily automated run. Skip any of these and the wizard will just skip the
matching questions — nothing breaks, you get a smaller feature set.

### Packaging your own `.skill` file

If you're distributing this yourself rather than using a pre-built `job-search.skill`, zip the
`job-search-skill` folder (containing `SKILL.md`, `references/`, this `README.md`, `.gitignore`) and
rename the `.zip` to `.skill`. Don't include a filled-in `references/profile.md` in the zip — only
`references/profile.template.md` should ship; a real `profile.md` is personal to whoever runs the
wizard.

## What it does

- **One-off search:** you ask for a shortlist, it searches your configured job boards, reads each
  posting (not just the title/tags), applies your include/exclude rules, and returns a ranked list
  with a one-line reason for every strong match.
- **Auto-drafted cover letters:** if you turn this on during setup, every posting marked ✅ Strong
  match gets a cover letter drafted and saved to a `Work Search/Cover Letters` folder in your Drive
  (or wherever you configure), with the link written into the tracker's `Cover Letter` column. Only
  Strong matches — never generated for Possible/Skip.
- **Scheduled sync mode:** if you set up a tracker spreadsheet, the setup wizard can also create a
  recurring scheduled task (default: 5:00 AM daily) that runs this skill unattended. It reconciles
  new postings and any inbox signals (application confirmations, rejections, interview requests) into
  the sheet, keeps row colors and statuses current, drafts cover letters for new Strong matches,
  without duplicating rows — and reports what it did each morning.

## Privacy

Your profile (`references/profile.md`) is personal to you — it's not part of this repo's tracked
history if you fork/clone it for your own use; add it to `.gitignore` if you plan to keep your copy
of this skill in version control, so you don't accidentally publish your own target companies, CV
path, or tracker URL.

## Files

- `SKILL.md` — the skill logic: setup wizard, search steps, triage rules, scheduled sync mode.
- `references/profile.template.md` — the structural skeleton the wizard fills in. Never edit this
  directly for a specific person; let the wizard generate `references/profile.md` from it.
- `.claude-plugin/plugin.json` — plugin manifest, makes this folder installable via Claude Code's
  `/plugin install`.
- `.claude-plugin/marketplace.json` — makes this same repo a self-hosted single-plugin marketplace,
  so `/plugin marketplace add <owner>/<repo>` works without a separate marketplace repo.

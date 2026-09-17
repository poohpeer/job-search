---
name: job-search
description: Searches job boards for postings that fit a candidate profile, using browser automation to browse real sites, reading each posting's description (not trusting site filters), applying the profile's include/exclude rules, and returning a marked-up shortlist. Works for any profession — software engineer, office manager, executive assistant, nurse, teacher, sales rep, whatever the candidate does. Use whenever the user wants to find, search, hunt, or screen job openings — phrases like "find me jobs", "search for office manager roles", "check LinkedIn / Indeed / [local job board] for positions", "any new postings for me", "screen these listings", or when they open a jobs page and ask what fits. On first use (no candidate profile saved yet), runs a setup wizard to build one — no personal data, and no assumption about the candidate's field, is hardcoded anywhere in this skill.
---

# Job Search: match live postings to a candidate profile

The user wants a curated shortlist of job openings that actually fit them — not a raw dump of
everything on the page. This works the same way regardless of the field: software engineering,
office/admin work, healthcare, education, sales, hospitality, whatever the candidate does. Your job:
browse the target site(s) with a browser tool, read the real descriptions, apply the candidate's
rules, and hand back a ranked, marked list with reasons.

The hard-won lesson baked into this skill: **site filters lie.** A role's experience field is often
blank or miscategorized while the description demands 8 years; a skills/keyword tag rarely reflects
the real requirements. So the site's own filters are a starting net, never the verdict. The verdict
comes from reading the posting.

This skill ships with no personal data and no built-in profession. Everything specific to a
candidate — name, contact details, target roles, skills/qualifications, location, sites, tracker —
lives in `references/profile.md`, which does not exist until the candidate (or you, on their behalf)
runs the setup wizard below.

## Step 0 — Setup wizard (first use only)

Check whether `references/profile.md` exists next to this file.

- **If it exists**, skip the wizard and go straight to Step 1 — load it as-is, don't re-ask
  questions it already answers.
- **If it does not exist** (fresh install, or the user explicitly asks to "set up" / "start" /
  "reconfigure" the job search), run this wizard before doing anything else. Do not invent or
  assume any answer — every field below must come from the person you're talking to.

Ask conversationally, in a few grouped messages rather than one giant form (people answer better in
batches of 3-5 related questions). Confirm your understanding of each answer before moving on if
anything is ambiguous. Suggested grouping:

**Group 1 — Identity & contact** (used to fill application forms later; leave any field blank if
the person doesn't want to provide it now)
1. Full name.
2. Email address to use for job applications.
3. Phone number (optional).
4. LinkedIn profile URL (optional).
5. Current city / location.

**Group 2 — Where and what**
6. Where are they searching? Get specific: target metro areas / cities / regions, and whether
   remote and/or hybrid roles anywhere (or anywhere in a given country) are acceptable.
7. What job titles are they targeting? Ask for a list, not just one title — people search under
   several near-synonymous titles regardless of field (e.g. "Office Manager" / "Operations
   Coordinator" / "Executive Assistant"; or "Senior Backend Engineer" / "Senior Software Engineer,
   Backend"; whatever applies to their profession). Capture seniority variants explicitly if they're
   open to more than one level.
8. How many years of relevant experience do they have, and what seniority/level are they targeting?
   (Titles for level vary by field — junior/senior, associate/manager/director, coordinator/lead,
   etc. — use whatever terms actually apply to the candidate's profession.)
9. Are they open to people-management / supervisory roles, or individual-contributor-style roles
   only (i.e. doing the work directly rather than managing others who do it)? This applies well
   beyond engineering — e.g. "Senior Executive Assistant" vs. "Office Manager who supervises a
   team." This drives a hard exclude rule later, so get an explicit answer, not an assumption.
10. Is there a seniority **ceiling** — titles they never want to see even if the skills match (e.g.
    "no Director/VP/C-level titles", or in a technical field "no Principal/Staff/Distinguished")?
    Some candidates want everything up to the top of their field; don't assume the candidate's
    current level is automatically the ceiling.

**Group 3 — Skills & match signals**
11. Core skills, qualifications, tools, certifications, or domain experience that should be treated
    as a strong-match signal (e.g. specific software proficiency, licenses/certifications, languages
    spoken, industry background, years in a specialty — whatever is central to this candidate's
    field).
12. Secondary / nice-to-have skills or qualifications that boost confidence but aren't required.
13. Anything they explicitly do NOT want to be matched against — e.g. a required skill, tool,
    certification, or qualification they don't have, that if it's the primary mandatory requirement
    should auto-exclude the posting. This becomes the "hard gate" / "non-negotiable exclusion" rule.
14. Any experience gaps they don't want oversold in cover letters or notes (e.g. "I've only touched
    X briefly — don't imply deep experience with it"). This applies to any field, not just technical
    skills — could be a software tool, a certification still in progress, a language proficiency, etc.

**Group 4 — Filters**
15. How recent should postings be? (default suggestion: last 30 days, but ask)
16. Any other hard requirements or dealbreakers not yet covered (visa/work-authorization needs,
    company size, industry to avoid, etc.)?

**Group 5 — Where to search**
17. Which job boards/sites should be searched? Don't just ask open-ended, and don't just propose one default list to prune — first find out the candidate's country/market (already known from Group 2's location answer), then compile as full a catalog as you reasonably can of relevant sites: well-known general boards for that market (e.g. LinkedIn, Indeed), prominent local/regional boards, ATS-aggregator search tools (e.g. Greenhouse's cross-company search), and — if their target titles suggest a field with its own boards (e.g. healthcare, hospitality, government) — field-specific ones too.
Show the candidate this full catalog during onboarding — every site you found, not just a trimmed default — and ask them which ones to actually use (present it as a checklist if your interface supports multi-select checkboxes; otherwise list it numbered and ask them to say which ones to keep). Don't presume any site is wanted just because it's well-known; the candidate picks from the full list rather than opting out of a pre-chosen one. Always leave room to add sites you didn't think of — ask explicitly "anything else you want searched that isn't on this list?" Don't assume your catalog is complete; candidates often know niche boards specific to their industry or region that you wouldn't guess. Note explicitly for each site the candidate picks whether discovery (searching/scraping) is fine or only manual application is fine — some sites' terms discourage automated scraping (LinkedIn is the most common example: many candidates prefer discovery OFF, apply-only ON). Ask, don't assume.
Concrete examples worth including in the catalog for Israeli tech candidates: devjobs.co.il, AllJobs, JobMaster, and Drushim as local boards (search Hebrew and English on these — postings often mix both); secrethunter.io, an aggregator that pulls postings directly from thousands of Israeli company career pages, including roles never posted to LinkedIn or the big boards; and ATS aggregators that surface roles before they hit the general boards, such as Greenhouse's cross-company search (my.greenhouse.io/jobs — see Step 3b) and Comeet (comeet.com, searched via a web search engine's site: operator since it has no cross-company search of its own).
18. Is the target job market multilingual? If postings on local sites mix languages (common outside
    English-only markets), ask for the target job titles translated into that language too, so
    searches catch both.

**Group 6 — Tracking & applying**
19. Do they want a tracker (spreadsheet) of what's been found/applied to? If yes, ask them to create
    their own new Google Sheet (or other spreadsheet) with columns `Company | Position | Location |
    Match | Status | Applied | Cover Letter | Source Link | Notes`, and share the URL/ID.
    Never reuse a tracker URL from an example or from a different person.
20. Do they have a CV/resume file to reference for uploads and cover letters later? Ask for a local
    file path or a cloud link. Leave blank if not ready yet — never fabricate one.
21. Do they want a cover letter automatically drafted and saved for every ✅ Strong match? If yes:
    - Confirm the storage location — default suggestion is a `Work Search/Cover Letters` folder in
      their Google Drive (or equivalent cloud storage), created if it doesn't exist. Let them
      override the parent folder name.
    - Ask for any tone/content preferences (e.g. formal vs. conversational, must-include points,
      things to never claim) and store them in the profile so cover letters stay consistent and
      honest.
    - Make clear this only fires for Strong matches, not every posting, to avoid spending effort (and
      the person's credibility) on weak fits.
22. If they want Gmail (or another inbox) scanned for application/rejection/interview signals, ask
    what language those emails will arrive in. The default signal phrases below are in English;
    if another language is needed, ask for the equivalent phrases (e.g. what a rejection email
    typically says in that language) rather than guessing a translation.
23. Do they want this to run automatically every morning? If yes, and a task-scheduling tool is
    available in this environment, offer to set up a recurring scheduled task (default suggestion:
    5:00 AM local time, adjustable) that invokes this skill in **Scheduled sync mode** (see below).
    Tell them plainly that this means the skill will search job boards and write to their tracker
    unattended, without asking for confirmation each time — get an explicit yes before creating it.
    After creating the task, run it once immediately (or ask the scheduling tool to do so) so that
    any tool-use permission prompts get pre-approved now rather than silently blocking a 5am run;
    otherwise a scheduled run can stall waiting on a permission dialog nobody is there to answer.
    If no scheduling tool is available in this environment, tell the person and skip this step — do
    not claim you scheduled something you didn't.

Once you have answers, write them into a new `references/profile.md`, using
`references/profile.template.md` as the structural skeleton (same section headers, so the rest of
this skill's instructions — which refer to those sections — keep working unmodified). Show the
person the generated profile and ask them to confirm or correct it before you run your first search.

## Step 1 — Load the candidate profile

Everything domain-specific (target titles, skills/qualifications, locations, include/exclude rules,
search terms) lives in `references/profile.md` — never in this file. Load it first.

- If the user names a different profile file or pastes a profile, use that instead.
- Otherwise use `references/profile.md` in this skill directory (created by the wizard above).
- If neither exists and the wizard hasn't run yet, run Step 0 first.
The profile defines *what* to look for. This SKILL.md defines *how* to look. Read the profile fully
before starting so its rules are in context while you scan.

## Step 2 — Confirm the target site and get set up

- Confirm which site(s) to search from the profile's site list (or ask if the profile doesn't
  specify one for this run).
- Use a browser automation tool (e.g. Claude-in-Chrome) — the user's real, logged-in browser. Many
  boards gate postings or personalize results behind a login.
- Take a screenshot / read the page first to understand the layout: where the search box is, how
  results are listed, how a posting opens, whether there's pagination or infinite scroll.

## Step 3 — Search broad, in every relevant language

- Run the profile's target titles as searches. If the profile notes the market is multilingual, run
  searches in **every** language the profile lists — postings routinely mix languages, and each
  query surfaces listings the other misses.
- Cast a **broad net** deliberately. Under-filtering is recoverable (you read and reject); a too-narrow
  search silently drops good roles you never see.
- Apply the profile's location filter as a convenience to shrink the list, but treat it as soft — a
  city-specific posting may be remote-friendly, a "remote" tag may hide an on-site requirement.

## Step 3b — Additional ATS discovery source (Greenhouse, useful for tech/software roles)

For software/tech-adjacent target titles, also search the Greenhouse cross-company job board at `my.greenhouse.io/jobs` — many companies post there before (or instead of) syndicating to aggregator boards, so skipping it silently misses real openings. Treat it with the same priority as the profile's other required sites for this kind of search; don't drop it silently for time.

- Plain `?query=...` alone is unreliable for location (it mixes in global remote roles even when a country name is typed into the query text). Use the explicit location filter params instead, confirmed working: `https://my.greenhouse.io/jobs/search?query=<title/skill keywords>&location=<Country>&lat=<lat>&lon=<lon>&location_type=country&country_short_name=<ISO code>` (e.g. for Israel: `location=Israel&lat=30.895128&lon=34.874702&location_type=country&country_short_name=IL`). Run a few queries built from the profile's target titles and core skills.
- Individual job links returned this way (`my.greenhouse.io/jobs/<company>/<id>`) are real, linkable posting URLs — use them directly as the Source Link, never the search-page URL.
- Apply the same Pass A/B triage, hard gate, and seniority ceiling to postings found here as to postings from the profile's primary sites.
- If Greenhouse itself is unreachable or errors out, note that explicitly in the report as a gap, the same way you would for any other required site — don't silently drop it.

## Step 4 — Triage each result

Work in two passes to spend effort where it counts:

**Seniority ceiling (hard rule if the profile sets one, applies before anything else).** If the
profile defines a seniority ceiling (whatever titles are above the candidate's target — e.g.
"no Director/VP/Head of/C-level," or in a technical field "no Principal/Staff/Distinguished/
Architect"), never include postings above it, regardless of skills fit. Catch these on sight in
Pass A by title whenever possible; if a posting's true level only becomes clear from the body (e.g. a
neutral title that turns out to be scoped above the ceiling — org-wide or company-wide ownership,
managing other managers, setting strategy rather than executing it), exclude it in Pass B before
scoring anything else. If the profile has no ceiling, skip this check.

**Pass A — title-only fast skip (before opening anything).** The profile lists title patterns that
are an instant skip (e.g. a discipline or specialty outside the candidate's target field named in the
title, junior/intern markers if the candidate is senior, or titles above the seniority ceiling). Skip
those on sight without opening them — this is where you save most of your time. Only open postings
whose title is neutral or explicitly on-target.

**Pass B — read the description of everything that survives Pass A.** Open the posting and evaluate
it against the profile's gates and rules, in order:

1. **Seniority ceiling**, if set. Re-check level from the actual responsibilities/requirements, not
   just the title — some neutral-sounding titles are scoped above the ceiling underneath.
2. **Hard gate.** The profile defines a non-negotiable condition (e.g. a core skill/qualification
   must appear, or the requirement must be experience-level-agnostic on that specific skill). If a
   posting fails the gate, exclude it immediately and stop evaluating it — don't rationalize
   borderline cases past a hard gate.
3. **Exclude rules.** Wrong primary skill set as a mandatory requirement, wrong seniority, wrong
   discipline, pure people-management with no direct-work component if the profile says IC-only —
   whatever the profile disqualifies.
4. **Include rules.** Match against the profile's strong-match and possible-match signals.
When genuinely uncertain after reading, lean toward **including** and let the description speak in
your notes — a borderline "possible match" the user can judge beats a silently dropped opportunity.
This does not override the hard gate or the seniority ceiling: those failures are always out.

## Step 5 — Return the shortlist

Present a numbered list. For each surviving posting, one row:

**Job Title | Company | Location | Posted date | Link**

Then, per the profile's marking scheme, tag each: **✅ Strong match**, **🟡 Possible match**, or
**❌ Skip (reason)**. Order strong matches first. Rules of thumb:

- For every ✅, add one line on *why* it fits — which specific profile skill/experience lines up with
  a specific requirement in the posting. Generic praise is useless; name the overlap.
- Highlight the standout signals the profile cares about (its strong-match skills/qualifications) so
  they're scannable.
- If you show ❌ rows at all, keep the reason to a few words. Often it's cleaner to list only ✅/🟡 and
  give a one-line tally of how many you skipped and the dominant reason.
- If a posting's date, location, or link couldn't be read, say so rather than guessing.

## Step 6 — Generate cover letters for Strong matches (if configured)

Only run this step if the profile's Group 6 answers turned cover-letter generation on. Only do it
for postings tagged **✅ Strong match** — never for Possible or Skip, even if the user seems eager;
weak fits produce weak, generically-truthful-at-best cover letters, and that costs the candidate
credibility.

1. **Find or create the storage folder.** Use the profile's configured parent folder (default
   `Work Search`) with a `Cover Letters` subfolder inside it, in the candidate's cloud storage (e.g.
   Google Drive). Search for the folder first; only create it if it genuinely doesn't exist yet — do
   this once per run, not once per posting.
2. **Check for an existing cover letter first.** If the tracker row already has a value in the
   `Cover Letter` column, skip generation for that row — this keeps the skill idempotent across
   repeated runs (one-off or scheduled) and never overwrites a letter the person may have already
   edited by hand.
3. **Draft the letter** from the candidate's profile — name, background, the skills/experience that
   specifically match this posting's stated requirements — plus the posting's company name, role
   title, and 2-3 concrete requirements it names. Keep it to roughly 250-400 words, professional tone
   by default (or whatever tone the profile specifies). Reuse the profile's "Standard form answers"
   and "Gaps to not oversell" so the letter never claims experience, tools, or years the candidate
   doesn't actually have — when in doubt, leave a claim out rather than inflate it.
4. **Save it as a document** (e.g. a Google Doc) named clearly, such as `{Company} - {Position} -
   Cover Letter`, inside the folder from step 1. Get its shareable link.
5. **Write the link into the tracker's `Cover Letter` column** for that row, using the same
   cell-by-cell editing care described in the Scheduled sync mode's Phase 3 write instructions below.
6. If folder creation, drafting, or the write-back fails for a given posting, don't block the rest of
   the run — leave that row's `Cover Letter` cell blank, note the failure in the row's `Notes` cell or
   in your final report, and continue with the next posting.

## Scheduled sync mode (twice-daily reconcile)

When invoked to "sync", "run the scheduled job search", or on a cron trigger, the goal shifts from
a one-off shortlist to **keeping the tracker sheet current**. Run three phases, then reconcile. The
guiding principle is **idempotency**: the job may run more than once a day, so it must add only what's
genuinely new and change only what actually changed — never duplicate a row, never overwrite a
human's manual edits with stale data. This mode requires the profile to have a tracker configured
(Step 0, Group 6) — if it doesn't, tell the user and fall back to the one-off shortlist mode instead.

**Phase 0 — Read current state.** Load the profile's tracker coordinates. Read the sheet's current
contents first so you know what's already tracked. A Drive/Sheets connector can *read* the sheet
without a browser — use it to pull the current rows into context; if the sheet is large, the read may
truncate, so also confirm the true last row in the browser (e.g. select a cell and press
Ctrl+Down/Ctrl+Up) before assuming you've seen every existing row — writing a "new" lead that's
actually already tracked further down is a duplicate. Build the set of existing `Company`+`Position`
keys and their current Status.

**Phase 1 — Inbox delta (if configured).** If the profile has inbox-monitoring set up, search for
messages since the last run (default: last 1 day, widen to last 3 days if unsure a run was missed).
Classify each using the profile's signal phrases into: new application, rejection, interview/
progress. Ignore recruiter noise. Produce a list of `(Company, Position, new Status, date, note)`
changes. When a message doesn't name the position, carry the company and note "role not named"
rather than inventing a title.

**Phase 2 — New postings.** Run the normal search-and-triage (Steps 2–5 above, including the
Seniority ceiling if set) against the profile's default sites. Keep only survivors whose
`Company`+`Position` is **not already in the sheet** — these are new leads. Cap the browse sensibly
(e.g. the first page or two of freshest results) so a recurring job stays fast; log what you covered.
If cover-letter generation is configured (Step 6), draft one now for each new ✅ Strong match so its
link is ready to write in Phase 3 — no need to wait for a separate pass.

**Phase 3 — Reconcile and write.** Merge the deltas against the current rows:
- Inbox change matches an existing row → update that row's Status/Applied/Notes in place (note any
  decision/rejection date in Notes — there's no separate Decision column).
- Inbox change matches no row → add a new row (a job applied to outside this tool).
- New posting from Phase 2 → add a new lead row (`Status=To apply`/`To review` per its match). If it's
  a ✅ Strong match and cover-letter generation is configured, include the link drafted in Phase 2 in
  the `Cover Letter` column when you write the row.
- A lead that the inbox now shows was applied to → promote the existing row, don't duplicate.
Then write the changes to the sheet **via a browser tool** if your connector can't write cells
directly:
- Ensure exactly one target browser is selected if more than one is connected.
- Open the sheet by URL. Prefer editing **one cell at a time**: use the Name Box to jump to an exact
  cell reference (e.g. `H111`), type the value, press Return, then jump to the next cell. Tabbing
  across a row is faster but risks two silent failure modes worth guarding against instead of using:
  (a) spreadsheet autocomplete can hijack a Tab/Return press mid-row and insert a *different*,
  longer existing value that shares your prefix — disable autocomplete suggestions first if the tool
  offers that setting; (b) a Tab meant to skip a blank column can fail to register, shifting every
  following value one column left. If you do tab across a row, verify immediately after by reading
  back the row's cells.
- If the tracker language uses a right-to-left script or other non-Latin script, text typed via
  automation can land incorrectly (e.g. truncated to the last word only). After typing such a cell,
  click it and check the formula bar to confirm the full string is there; if not, prefer re-entering
  values in a transliterated/Latin form rather than repeatedly fighting the input.
- Work cell-by-cell and verify with a screenshot or the formula bar after each row — typing into a
  live spreadsheet via automation is brittle; a misplaced or corrupted value is worse than a missing
  one.
- **Also during Phase 3, do a status sweep over every row (not just the ones you just touched):**
  scan the Status column, and recolor any row whose fill doesn't match its current status per the
  profile's color-coding rules (see below). This catches rows a human edited manually since the last
  run, so re-check the full column each pass rather than relying only on this run's deltas.
- Apply the profile's row color coding after writing values (a sensible default, unless the profile
  says otherwise): yellow = added today, green = in-process statuses (Interviewing/Applied/Applied
  via referral), gray = closed-out statuses (Rejected/Not relevant), no fill = untouched leads (To
  apply/To review/Hold). Recolor rows whose status changed this run, and clear yesterday's yellow
  from rows that are no longer new.

**Phase 4 — Report.** Summarize what changed: new applications detected, rejections/updates applied,
new leads added, cover letters generated (and any that failed and were left blank), rows recolored in
the sweep, and anything ambiguous a human should check. This is the run's output.

## Notes on doing this well

- **Read before judging.** The entire value of this skill over a raw site search is that you open
  postings and read them. If you find yourself ranking on titles alone past Pass A, stop.
- **Respect the seniority ceiling if one is set.** When in doubt about whether a title crosses it,
  read the responsibilities — company-wide or org-wide ownership, managing other managers, or setting
  strategy/direction rather than executing it all signal above-target scope, whatever the field.
- **Don't apply, don't message, unless the profile or the user explicitly says otherwise.** By
  default this skill screens and reports. Submitting an application, sending a message, or filling a
  form is a separate, side-effectful action — surface the role and let the user decide, unless
  they've explicitly authorized auto-apply for a specific channel.
- **Be honest about coverage.** If a site paginated and you read the first N, or a login wall blocked
  results, say what you covered and what you didn't. A shortlist that overstates its completeness is
  worse than one that names its limits.
- **Never carry data between candidates.** This skill and its SKILL.md ship with zero personal data.
  If you ever find yourself about to reuse a name, email, tracker URL, or CV path from a previous
  person's profile for a new person, stop — that's a sign the wizard was skipped. Run Step 0.
- **Cover letters must stay honest.** Never let a cover letter claim a skill, tool, certification, or
  years of experience beyond what the profile actually states — the profile's "Gaps to not oversell"
  answers exist specifically to prevent this. A generic-but-true letter beats a specific-but-false one.
- **Unattended runs still need a human check-in eventually.** A 5am scheduled run with
  auto-approved tool permissions means nobody is watching in real time — that's the point, but it also
  means mistakes (a bad cover letter, a miscategorized rejection) can accumulate silently. The Phase 4
  report exists so the person can review what happened each morning; don't skip or shorten it.

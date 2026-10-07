# IDS Website Redesign — Discovery Questionnaire

Phase 1 discovery questionnaire for the IDS website redesign: 28 questions across five
sections, 9 of them marked **Priority** because they block the design phase.

## Structure

```
vercel.json                                      root URL → the questionnaire
ids_discovery_questionnaire/questionnaire.html   the whole thing (no build step)
```

Plain HTML, CSS and JavaScript in one file, no dependencies and no web fonts (it uses the
reader's own system fonts).

## Editing the questions

Questions are **data, not markup**. Everything lives in two objects at the top of the
`<script>`:

- `CONFIG` — title, phase label, Formspree endpoint, storage key, footer.
- `SECTIONS` — the sections and their questions. Add, reorder or reword freely; numbering,
  the tab strip, the progress meter, the priority count and the submitted payload all
  follow automatically.

A question is one of three shapes:

```js
{ q: 'Plain question?', hint: 'Optional guidance.' }        // one textarea
{ q: 'Multi-part?', fields: [ {id:'x', label:'…', area:true} ] }  // several labelled boxes
{ q: 'Rank these.', rank: ['A','B','C'] }                   // one select per item
```

Add `star: true` to mark a question as Priority.

## Behaviour

- **Tabbed sections** — one section at a time, sticky tab strip, Back/Next, `#s3` deep links.
  Hidden sections keep their values, so nothing is lost by moving around.
- **Priority filter** — a checkbox hides every non-priority question, for a first pass.
- **Save & resume** — answers save to `localStorage` as they are typed (debounced, plus on
  `pagehide` and `visibilitychange`) and are restored on return, along with the section the
  respondent was on. A resume banner names whose draft it is and offers a fresh start.
- **Backup file** — *Save a copy* downloads the answers as JSON; *Restore* loads one back,
  for moving between devices.
- **Name required** — sending without a name is blocked, and bounces back to "About you".
- Drafts never leave the browser. Nothing is sent until *Send my answers* is pressed.

## Submissions

Posts JSON to Formspree. The respondent's name goes into the email subject and their email
into `_replyto`, so replying reaches them directly. Answers arrive as readable
`question → answer` pairs, numbered, with priority questions prefixed `*`.

> **Set this before sending the link to IDS.** `CONFIG.endpoint` currently points at
> `https://formspree.io/f/xbdakjjj`, the form shared with the HWB questionnaire and the vow
> renewal feedback form — IDS responses would land in that same inbox. Create a separate
> Formspree form and replace the endpoint.

## Deploying

Static site on Vercel — no build command, no framework. Pushing to `master` redeploys.

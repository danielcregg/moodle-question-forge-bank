# Moodle Question Forge — Public Bank

Community-contributed Moodle quiz questions curated for use with the [Moodle Question Forge](https://github.com/danielcregg/moodle-question-forge) app.

## For users

You don't need this repo directly. Open [Moodle Question Forge](https://danielcregg.github.io/moodle-question-forge/), go to the **Public Bank** tab, browse, and click any question to add it to your own question bank.

## How the bank is built

- Each question lives in its own file in [`questions/`](./questions/).
- A GitHub Action ([`.github/workflows/rebuild-bank.yml`](./.github/workflows/rebuild-bank.yml)) concatenates every file under `questions/` into [`bank.json`](./bank.json) whenever a PR is merged to `main`.
- The Moodle Question Forge app fetches `bank.json` via jsDelivr CDN — `https://cdn.jsdelivr.net/gh/danielcregg/moodle-question-forge-bank@main/bank.json` — on every page load, so accepted questions appear to users within minutes of merge.

## How to contribute

The app does this for you automatically. In the Moodle Question Forge:

1. Create a question (AI-generate, PDF-extract, or import).
2. Let it pass AI review (score ≥ 8/10).
3. Click the `⋯` menu on the card → **Submit to Public Bank**.
4. The app runs a client-side AI moderation check.
5. If it passes, the app opens GitHub's "new file" page with your question's JSON pre-filled.
6. You hit **Propose changes** → GitHub forks the repo for you and opens a pull request.
7. A maintainer reviews (a human and/or automation). When merged, the Action rebuilds `bank.json` and your question shows up in everyone's Public Bank on the next page load.

### Contributing without the app

If you've written a question by hand, you're welcome to open a PR directly:

1. Add one JSON file to `questions/` following [`SCHEMA.md`](./SCHEMA.md).
2. Filename convention: `<question_type>-<slug>-<8char-hash>.json` (e.g. `multichoice-binary-search-complexity-a3f9c02b.json`).
3. Open a PR. The Action will validate and rebuild on merge.

## Moderation policy

Questions are rejected if they:
- Contain violence, hate speech, explicit sexual material, or slurs.
- Target specific people (real students, staff, public figures) negatively.
- Contain personal information (emails, phone numbers, addresses).
- Are prank/joke submissions rather than educational.
- Advocate political or religious positions outside clearly labelled historical/academic context.
- Reproduce substantial copyrighted text without fair-use justification.

Legitimate academic treatment of sensitive topics (history of war, biology of reproduction, ethics of AI, etc.) is welcome and encouraged.

## Licensing

All questions submitted to this bank are published under **CC BY-SA 4.0**. Submitters retain attribution via their GitHub username stored in each question's `submitter` field; re-users must credit and share alike.

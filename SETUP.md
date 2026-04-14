# Setup — one-time (you, Daniel)

## 1. Create the public bank repo

Go to https://github.com/new and create an **empty** public repo:

- Owner: `danielcregg`
- Name: `moodle-question-forge-bank`
- Description: *Community-contributed Moodle quiz questions for the Moodle Question Forge app.*
- **Public** (required for jsDelivr).
- **Don't** add a README/license/gitignore — we're pushing seed content.

## 2. Push these seed files

From this repo's root:

```bash
cd /tmp
git clone --depth=1 https://github.com/danielcregg/moodle-question-forge-bank.git
rsync -a /home/daniel.cregg/moodle-question-forge/public-bank-seed/ moodle-question-forge-bank/
cd moodle-question-forge-bank
git add .
git commit -m "Seed public bank with README, schema, sample question, rebuild action"
git push origin main
```

The first push fires the rebuild-bank Action, which builds `bank.json` containing the sample binary-search question.

## 3. Verify jsDelivr is serving

Open in browser: https://cdn.jsdelivr.net/gh/danielcregg/moodle-question-forge-bank@main/bank.json

You should see a JSON array with the one sample question. jsDelivr caches for up to 12 h on the `@main` tag; for faster propagation use a commit hash or purge the cache at https://www.jsdelivr.com/tools/purge.

## 4. Done

The Moodle Question Forge's Public Bank tab now shows questions from this repo. To submit a new question from the app, users click `⋯` → **Submit to Public Bank** on any validated question (score ≥ 8/10), which opens a pre-filled pull request on this repo for your review.

## Moderation workflow

PRs arrive as single-file additions under `questions/`. Review the diff, check the content matches the [moderation policy](./README.md#moderation-policy), and **merge** (or close) as normal GitHub PRs. On merge, the Action rebuilds `bank.json` and users see the new question on their next page load.

## Deleting a question from the bank

Delete its file from `questions/` on `main` (via GitHub's UI or a commit). The Action rebuilds `bank.json` without it.

## When you want to archive this seed folder

Once the public-bank repo is live and verified, you can delete the `public-bank-seed/` folder from this app repo — it's only scaffolding for the initial push.

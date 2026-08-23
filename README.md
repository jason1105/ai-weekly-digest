# AI 技术周刊

A beautiful weekly tech digest, auto-generated every Monday using GitHub Actions + Anthropic Claude API. Published via GitHub Pages.

## How it works

1. **Every Monday 09:00 UTC**, the workflow runs automatically.
2. It fetches the top 20 stories from HackerNews and top GitHub trending repos.
3. It calls an LLM via **OpenRouter** (default `deepseek/deepseek-chat`) to pick the 10 most interesting items and write a Chinese summary for each.
4. The result is saved to `_data/latest.json` and committed back to the repo.
5. GitHub Pages serves `index.html`, which reads the JSON and renders the digest.

You can also trigger a run manually from the **Actions** tab → **Generate Weekly AI Digest** → **Run workflow**.

## Setup (one-time)

### 1. Fork / clone this repo to your GitHub account

### 2. Add the Anthropic API key as a secret

Go to your repo → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**.

| Name | Value |
|------|-------|
| `OPENROUTER_API_KEY` | Your key from [openrouter.ai/keys](https://openrouter.ai/keys) |

> `GITHUB_TOKEN` is provided automatically by GitHub Actions — no configuration needed.

### 3. Enable GitHub Pages

Go to **Settings** → **Pages** → Source: **Deploy from a branch** → Branch: `main` → Folder: `/ (root)`.

### 4. Trigger your first run

Go to **Actions** → **Generate Weekly AI Digest** → **Run workflow**.

After it completes (~30 s), visit `https://<your-username>.github.io/<repo-name>/` to see your digest.

## Project structure

```
.
├── .github/
│   └── workflows/
│       └── weekly-digest.yml   # GitHub Actions workflow
├── _data/
│   └── latest.json             # Generated digest data (committed by the workflow)
├── index.html                  # Single-file Pages site
├── .nojekyll                   # Tells GitHub Pages to skip Jekyll processing
└── README.md
```

## Customization

- **Language / style of summaries** — edit the prompt string inside `weekly-digest.yml`.
- **Number of items** — change `[:20]` (fetch count) and the `10` in the prompt.
- **Schedule** — change the cron expression (`0 9 * * 1` = Monday 09:00 UTC).
- **Model** — set a repository variable `OPENROUTER_MODEL` (Settings → Secrets and variables → Actions → Variables) to any model ID OpenRouter supports. No code change needed; the default is `deepseek/deepseek-chat`.
- **Data sources** — add more API calls (e.g. Reddit, arXiv) before the Claude step.

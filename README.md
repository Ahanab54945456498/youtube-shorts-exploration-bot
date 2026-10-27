# YouTube Shorts Exploration Bot (Starter v1)

A starter repository for researching five YouTube channels and drafting an original Shorts explainer script. GitHub Actions runs the research job; a small web form can trigger it through a Cloudflare Worker.

## What works in v1
- Web form for five YouTube channel URLs, language, and schedule preference.
- GitHub Actions manual run (from the web form via Worker, or Actions > Run workflow).
- Daily scheduled research run (UTC schedule; change in `.github/workflows/daily.yml`).
- YouTube Data API research of recent public videos and a ranked report.
- Original explainer-script *draft* based on the selected video's topic/title/description.
- CSV and Markdown report uploaded as workflow artifacts.

## Not yet automated in this starter
Voiceover, licensed footage acquisition, video rendering, and YouTube publishing are intentionally not enabled yet. They need a TTS choice, a legal footage source, YouTube OAuth credentials, and end-to-end testing. The research report is not a license to download or reuse another creator's video. Re-editing a copied video does not make it copyright-free.

## 1. Create the GitHub repository
1. In GitHub, click **New repository**.
2. Name it `youtube-shorts-exploration-bot`.
3. Choose **Public** if you want others to fork it, or **Private** while testing.
4. Do not add a README/license/gitignore (this ZIP already contains the files).
5. Extract this ZIP and upload all files/folders to the new repository root, then commit.

## 2. Add the YouTube API key
1. Open Google Cloud Console and create/select a project.
2. Enable **YouTube Data API v3** and create an API key.
3. In GitHub repo: Settings → Secrets and variables → Actions → New repository secret.
4. Add `YOUTUBE_API_KEY` with the key as its value. Never put it in `web/index.html` or commit it.

## 3. Test from GitHub first
1. Open Actions and enable workflows if asked.
2. Select **Daily Research / Manual Start** → **Run workflow**.
3. Enter five YouTube channel URLs/handles and choose `bn` or `en`.
4. Open the completed run and download the `research-report` artifact.

## 4. Optional website Start button (Cloudflare Worker)
The website is static; it cannot safely store a GitHub token. Deploy `worker/worker.js` as a Cloudflare Worker, configure secrets/variables:
- `GH_TOKEN`: fine-grained GitHub token with Actions: write permission on this repository only.
- `GH_OWNER`: your GitHub username/org.
- `GH_REPO`: `youtube-shorts-exploration-bot`.
- `GH_WORKFLOW`: `daily.yml`.

Then edit `web/index.html` and replace `YOUR_CLOUDFLARE_WORKER_URL` with the Worker URL. Keep `GH_TOKEN` only in Worker Secrets, never in browser code. The token is used only to dispatch this workflow.

## 5. Daily schedule
The included cron is `30 12 * * *` (12:30 UTC = 18:30 Bangladesh time, UTC+6). GitHub Actions schedules can be delayed during busy periods and may be disabled for inactive public repositories. Update the cron to your preferred UTC time. For scheduled runs, add these repository **Variables**:
`CHANNEL_1` ... `CHANNEL_5`, and `LANGUAGE` (`bn` or `en`).
For schedule runs, those five variables are used. The website Start button passes its own five links and language.

## Security and limitations
- Never commit API keys, OAuth tokens, or passwords.
- Public repository users should fork the repo and set their own secrets and variables. Do not expose a shared upload token to strangers.
- GitHub Actions is not unlimited compute/storage. This v1 does research and drafts text only.
- YouTube API quota limits apply.
- A high-view video is a research signal, not proof that the content is reusable.

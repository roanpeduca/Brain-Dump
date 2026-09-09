# Brain Dump

A small Next.js app that reads and writes your Notion Brain Dump page:
generate today's tasks (sorted into Tutorials / Personal), record a voice
note that transcribes into the notes box, and summarize the day into a
Day Summary block under the TO DOs callout.

I can't deploy this to your Vercel account myself — deploying needs your
own login and your own API keys, which I don't have and shouldn't ask you
to hand over. Here's how to do it yourself, about 10 minutes:

## 1. Get a Notion integration token

1. Go to https://www.notion.so/my-integrations and click "New integration".
2. Name it anything (e.g. "Brain Dump app"), pick your workspace, create it.
3. Copy the "Internal Integration Secret" — this is your `NOTION_TOKEN`.
4. Open your Brain Dump page in Notion, click "..." in the top right →
   "Connections" → add the integration you just created. Without this
   step the app can't see the page.

## 2. Get an Anthropic API key

1. Go to https://console.anthropic.com/settings/keys and create a key.
2. This is your `ANTHROPIC_API_KEY`. Note: API usage is billed separately
   from any Claude.ai subscription.

## 3. Push this code to GitHub

```
cd brain-dump-app
git init
git add .
git commit -m "Initial commit"
```
Create a new empty repo on GitHub, then follow its "push an existing
repository" instructions.

## 4. Deploy on Vercel

1. Go to https://vercel.com/new and import the GitHub repo.
2. Before the first deploy, add three environment variables (Settings →
   Environment Variables, or during the import screen):
   - `ANTHROPIC_API_KEY`
   - `NOTION_TOKEN`
   - `NOTION_PAGE_ID` — already defaulted in `.env.example` to your Brain
     Dump page's ID (`3782edf2-18e8-8027-a0d3-f11c16f5ce43`); only change
     this if you move to a different page.
3. Click Deploy. Vercel gives you a live URL when it finishes.

## Notes on how it works

- The app expects your Brain Dump page's first callout block to be the
  TO DOs list. "Generate tasks" clears and rewrites that callout's
  children (a quote line, then a to-do per task). "Summarize my day"
  reads that callout's checked state and adds a Day Summary block right
  after it, without touching the callout.
- Voice recording uses your browser's built-in speech-to-text (Web
  Speech API) — well supported in Chrome and Edge, not in Safari or
  Firefox.
- To run it locally first: copy `.env.example` to `.env.local`, fill in
  the three values, then `npm install && npm run dev`.

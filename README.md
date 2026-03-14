# /wpdraft — WordPress Draft Publisher for Claude Code

A Claude Code slash command that publishes blog content to your WordPress site as a draft — with one simple command.

Supports Google Docs URLs, PDF/DOCX files, and AI-written text. Images, headings, links, tables, and video embeds are all preserved automatically.

> **Note:** This requires the **Claude Code CLI** (the terminal app) — it does not work on the claude.ai website. [Install Claude Code here.](https://claude.ai/code)

---

## A little backstory

I'm not a developer. I built this as my first ever Claude Code skill because I was tired of copy-pasting blog posts from Google Docs into WordPress and losing all the formatting. I figured there had to be a better way — and there was. I'm sharing it here in case it's useful for anyone else.

---

## What it does

Type `/wpdraft` in Claude Code, then:

- Paste a **Google Docs URL** → it downloads the doc, preserves headings/images/links/tables/videos, and creates a WordPress draft
- Provide a **PDF or DOCX file path** → same thing
- Ask Claude to **write a post from scratch** → it publishes the AI-written content directly as a draft

All posts are saved as **drafts only** — nothing is ever auto-published. You always review before it goes live.

### What gets preserved

| Element | Google Docs | PDF | DOCX |
|---------|------------|-----|------|
| Headings (H1–H6) | ✅ | ✅ | ✅ |
| Paragraphs | ✅ | ✅ | ✅ |
| Bold / Italic | ✅ | ⚠️ Partial | ✅ |
| Hyperlinks | ✅ | ✅ | ✅ |
| Tables | ✅ | ⚠️ Partial | ✅ |
| Images (in position) | ✅ | ✅ | ✅ |
| YouTube / Vimeo embeds | ✅ | ❌ | ❌ |

---

## Requirements

- [Claude Code](https://claude.ai/code) installed and working
- Python 3 installed on your machine
- A WordPress site (self-hosted or WordPress.com Business plan) with **Application Passwords** enabled

---

## Setup (5 minutes)

### Step 1 — Clone this repo

Open Terminal and run:

```bash
git clone https://github.com/rohitchavane/wpdraft-claude-skill.git
cd wpdraft-claude-skill
```

### Step 2 — Install Python dependencies

```bash
pip3 install -r requirements.txt
```

### Step 3 — Create your config file

Copy the example config:

```bash
cp config.example.json config.json
```

Open `config.json` in any text editor and fill in your details:

```json
{
    "wordpress_url": "https://your-site.com",
    "username": "your-wordpress-email@example.com",
    "application_password": "xxxx xxxx xxxx xxxx xxxx xxxx"
}
```

> **Important:** `config.json` is in `.gitignore` — it will never be committed to git. Your credentials stay on your machine only.

### Step 4 — Get a WordPress Application Password

Application Passwords are a secure way to let tools access WordPress without using your main login password.

1. Log into your WordPress dashboard
2. Go to **Users → Profile**
3. Scroll down to **Application Passwords**
4. Type a name (e.g. "Claude Code") and click **Add New Application Password**
5. Copy the password shown — it looks like `xxxx xxxx xxxx xxxx xxxx xxxx`
6. Paste it into your `config.json`

> Your user role must be **Editor** or **Administrator** to publish posts via the API.

### Step 5 — Open Claude Code in this folder

```bash
claude
```

That's it. You're ready.

---

## Usage

Open Claude Code from inside the cloned folder, then type:

```
/wpdraft
```

Claude will ask what you want to publish. You can say things like:

- *"Publish this Google Doc: [paste your link]"*
- *"Publish the DOCX file at /path/to/my-post.docx"*
- *"Write a 1000-word post about [topic] and publish it as a draft"*

---

## Troubleshooting

**401 Unauthorized error**
- Double-check your `application_password` in `config.json` — include the spaces exactly as WordPress shows them
- Make sure your user role is Editor or Administrator (not Subscriber or Contributor)

**Google Docs not working**
- The doc must be shared as "Anyone with the link can view" — it doesn't need to be fully public, just link-accessible

**Images not uploading**
- Check that your WordPress user has permission to upload media (Editor and above do by default)

**Module not found errors**
- Run `pip3 install -r requirements.txt` again to make sure all dependencies are installed

---

## Files in this repo

| File | What it is |
|------|-----------|
| `wordpress_publisher.py` | The Python script that does all the work |
| `.claude/commands/wpdraft.md` | The Claude Code slash command definition |
| `config.example.json` | A blank template — copy this to `config.json` |
| `requirements.txt` | Python libraries needed |
| `.gitignore` | Prevents your real `config.json` from being committed |

---

## License

MIT — use it, modify it, share it however you like.

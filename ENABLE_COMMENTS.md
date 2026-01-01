# How to Enable Comments on Your Blog

Your blog is configured to use **Giscus** for comments - a clean, privacy-friendly commenting system powered by GitHub Discussions.

## Why Giscus?

- **Free & Open Source**: No costs, no tracking
- **GitHub-based**: Readers with GitHub accounts can comment
- **No ads**: Privacy-friendly
- **Markdown support**: Comments can include code, math, links
- **Spam protection**: GitHub's moderation built-in

## Setup Steps (5 minutes)

### 1. Enable GitHub Discussions on Your Repo

1. Go to your repo: `https://github.com/galileoChr/essencepartagee.github.io`
2. Click **Settings** tab
3. Scroll to **Features** section
4. Check **Discussions**
5. Click **Set up discussions**

### 2. Get Your Giscus Configuration

1. Visit [giscus.app](https://giscus.app/)
2. Enter your repository: `galileoChr/essencepartagee.github.io`
3. Choose mapping: **pathname** (recommended)
4. Choose discussion category: **General** or create "Blog Comments"
5. Choose theme: **Preferred color scheme** (auto light/dark)

The site will generate configuration values for you.

### 3. Update hugo.toml

Replace the commented section in `hugo.toml` with your actual values:

```toml
[params.comments.giscus]
  repo = "galileoChr/essencepartagee.github.io"
  repoID = "YOUR_REPO_ID"  # From giscus.app
  category = "General"
  categoryID = "YOUR_CATEGORY_ID"  # From giscus.app
  mapping = "pathname"
  theme = "preferred_color_scheme"
  lang = "en"
```

### 4. Rebuild and Deploy

```bash
hugo
git add .
git commit -m "Enable Giscus comments"
git push
```

## What Readers Will See

At the bottom of each post:
- A comment box (if signed into GitHub)
- Existing comments
- Reactions
- "Sign in with GitHub to comment" (if not signed in)

## Alternative: Simpler Options

If you prefer not to use Giscus, alternatives include:

**1. Utterances** (Similar to Giscus but uses Issues instead of Discussions)
```toml
[params.comments]
  enable = true
  provider = "utterances"
  [params.comments.utterances]
    repo = "galileoChr/essencepartagee.github.io"
    issueTerm = "pathname"
    theme = "github-light"
```

**2. Disqus** (Traditional, has ads in free tier)
```toml
[params.comments]
  enable = true
  disqusShortname = "your-shortname"
```

**3. Email-based** (No system, just invite readers to email you)
Add at end of posts:
```markdown
---
*Questions or thoughts? Email me at [your-email] or open a GitHub issue.*
```

## Current Status

- Comments are configured in hugo.toml
- Waiting for Giscus setup (repo ID, category ID)
- Share buttons enabled
- RSS feed active
- GitHub social link added

Once you complete the Giscus setup, comments will appear automatically on all posts!

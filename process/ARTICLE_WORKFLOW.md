# Article Workflow Standards

The official process for converting transcripts and ideas into published blog posts on theoreticallyimpossible.org. This document is the authoritative SOP — when Kevin asks for a new article or a transcript-to-post, follow these steps in order.

---

## The cardinal rule

**Write only the article HTML. Do not touch `blog.html`, `index.html`, `feed.xml`, or `sitemap.xml` until Kevin explicitly says publish.**

The four index/feed/sitemap files are *publication surfaces*. Touching them is the moment a draft becomes a live post. That moment is Kevin's call, not the model's. Earlier model behavior treated drafting and publishing as one continuous step, which forced revert work and risked unreviewed drafts going live. Drafting and publishing are now two separate phases with an explicit human gate between them.

---

## Pipeline at a glance

```
Source → Discuss angles → Draft HTML → Track in TODO.md → STOP
                                                            ↓
                                                       Kevin reviews
                                                            ↓
                                                  Kevin says "publish"
                                                            ↓
                                                Run publish-step checklist
```

The STOP line is the gate. Everything above it is drafting. Everything below it requires explicit approval.

---

## Step 1: Source

Locate the source material. Most articles start from an Otter.ai transcript in `Content/article transcripts/`. Some start from a fresh idea in conversation.

- Read the entire transcript before responding. No skimming.
- Identify the spine (the one sentence or claim the piece is built around), the stories (concrete scenes that prove the spine), and the throughline (what the reader walks away with).
- If the transcript is messy, that's normal — Otter loses speaker tags and inserts artifacts. Read past them.

## Step 2: Discuss angles

Always discuss before drafting. Use `AskUserQuestion` to surface 3–4 viable angles, plus format and any ambiguity worth resolving up front. A single AskUserQuestion call should cover:

- **Angle** — 3–4 distinct framings of the piece, with one recommended
- **Format** — SITREP vs long-form essay vs short field note
- **Backstory level** — how much personal context to include (ask if the transcript has a backstory worth weighing)

Recommend the strongest angle with reasoning. Wait for Kevin's pick. If he wants a hybrid or has his own framing, refine in conversation before drafting.

## Step 3: Draft

Once the angle and format are settled, write the article HTML. **One file. One location.**

- Path: `Content/website/blog/<slug>.html`
- Slug: kebab-case, derived from a distinctive phrase in the title. Avoid stop words. Avoid generic slugs like `the-post` or `april-26`.
- Match voice and structure to existing posts. Reference `build-for-humans-hand-to-agents.html` for SITREP voice, `the-framework.html` or `origin-story.html` for long-form essay voice.
- See **Article HTML Conventions** below for the boilerplate.

Word count guidance:
- SITREP: 800–1,200 words
- Long-form essay / story: 1,200–1,800 words
- Short field note: 500–700 words

## Step 4: Track in TODO.md

Add an entry to `Content/TODO.md` under `## Up Next` → `### New Articles`, matching the format already in use:

```
- [ ] Article: "Title Here" — DRAFT COMPLETE. <one-line summary of what the piece does>. HTML at website/blog/<slug>.html.
```

Checkbox stays unchecked until publish. This is the backlog. Drafts can sit here for days, weeks, or indefinitely.

## Step 5: STOP and hand back

Reply with the file link to the article HTML. Briefly note:
- The angle/spine you used
- Word count
- Anything you want Kevin to look at specifically (a paragraph you weren't sure about, a section that could be trimmed)

Then **stop**. Do not pre-emptively wire it up. Do not preview what the publish step will look like. Do not create any other artifacts. Wait.

## Step 6: Publish (only on explicit go)

Trigger phrases that mean publish: "publish it," "ship it," "wire it up," "let's go live," "push it live," or anything else where Kevin's intent to publish is unambiguous. If there's any doubt, ask.

When publishing, run the **Publish-Step Checklist** in order. All five steps. Same publish date everywhere.

---

## Publish-Step Checklist

When Kevin approves publish, update these five surfaces in this order. Use today's actual date (verify via shell if needed).

### 1. `Content/website/blog.html`

Add a new `<div class="blog-card">` at the top of `<div class="blog-preview-list">`. Format:

```html
<div class="blog-card">
  <div class="blog-card-body">
    <div class="blog-card-date">May 3, 2026</div>
    <h3><a href="blog/<slug>.html">Article Title</a></h3>
    <p>Article summary, 1–3 sentences, matches meta description.</p>
    <a href="blog/<slug>.html" class="read-more">Read more &rarr;</a>
  </div>
</div>
```

For SITREP posts, the date line is:

```html
<div class="blog-card-date"><span class="sitrep-tag">SITREP</span> &nbsp; May 3, 2026</div>
```

### 2. `Content/website/index.html`

The home page surfaces the **four most recent posts** in a `<div class="blog-preview-list">`. Add the new card at the top using the same card markup as blog.html. Drop the oldest of the four off the bottom.

### 3. `Content/website/feed.xml`

Add a new `<item>` immediately after the closing `</image>` tag (above all existing items). Format:

```xml
<item>
  <title>Article Title</title>
  <link>https://theoreticallyimpossible.org/blog/<slug>.html</link>
  <guid>https://theoreticallyimpossible.org/blog/<slug>.html</guid>
  <pubDate>Sun, 03 May 2026 12:00:00 -0500</pubDate>
  <description>Same summary as blog.html card.</description>
</item>
```

For SITREP posts, the title is `SITREP: Article Title`.

Update `<lastBuildDate>` at the top of the channel to match the new pubDate.

**Known issue:** `feed.xml` is currently truncated (missing closing `</item></channel></rss>`) and `sitemap.xml` is missing `</urlset>`. New entries are inserted near the top so they remain well-formed. If Kevin asks for a fix, patch the closing tags then.

### 4. `Content/website/sitemap.xml`

Add a new `<url>` block after the homepage `<url>`. Format:

```xml
<url>
  <loc>https://theoreticallyimpossible.org/blog/<slug>.html</loc>
  <lastmod>2026-05-03</lastmod>
  <changefreq>monthly</changefreq>
  <priority>0.7</priority>
</url>
```

Update the homepage `<lastmod>` to match.

### 5. `Content/TODO.md`

Move the article entry from `### New Articles` to the `## Completed` section. Format:

```
- [x] Article: "Title" — Published <date>. <one-line summary of what landed>. Blog listing, homepage, sitemap, RSS.
```

If the post is a SITREP, prefix the title with `SITREP: `.

---

## Article HTML Conventions

All blog posts share a common boilerplate. The fastest way to produce a new one is to copy `build-for-humans-hand-to-agents.html` (SITREP) or `the-framework.html` (long-form) and edit. The required elements:

### `<head>`
- `<title>` — `Article Title | Theoretically Impossible Solutions` for long-form, `SITREP: Article Title | Theoretically Impossible Solutions` for SITREP
- `<meta name="description">` — 1–2 sentences, used as social share text
- Favicon and apple-touch-icon links (relative to `../`)
- Open Graph tags: `og:title`, `og:description`, `og:image`, `og:url`, `og:type=article`
- `twitter:card` summary
- `<link rel="canonical">` to the full URL
- Google Analytics snippet (`G-3ERT6LR74L`)
- RSS alternate link to `../feed.xml`
- `<link rel="stylesheet" href="../css/style.css">`

### Header nav
- Site brand block linking to `../index.html`
- Mobile nav toggle button
- Nav links: Home, About, Blog (active), RSS icon, Get in Touch button

### Post header section
```html
<section class="post-header">
  <div class="container--narrow">
    <p><span class="sitrep-tag">SITREP</span></p>   <!-- only for SITREPs -->
    <h1>Article Title</h1>
    <p class="post-meta"><span class="post-date">Month D, YYYY</span> &middot; Kevin Phifer</p>
  </div>
</section>
```

For long-form posts, omit the `<p><span class="sitrep-tag">SITREP</span></p>` line.

### Article body
- Wrap in `<article class="post-content"><div class="container--narrow">…</div></article>`
- Plain `<p>` paragraphs
- `<h2>` for section breaks
- Use `&mdash;` for em dashes (matches existing posts)
- Use `<em>` sparingly for emphasis; avoid `<strong>` in body prose

### Author footer (always include, exact copy)
```html
<div style="margin-top: 3rem; padding-top: 2rem; border-top: 1px solid var(--color-border);">
  <p style="color: var(--color-text-light); font-size: 0.9rem;">
    <strong>Kevin Phifer</strong> is the founder of <a href="../about.html">Theoretically Impossible Solutions LLC</a>, specializing in agentic AI development and consulting. You can reach him at <a href="mailto:kevin.phifer@theoreticallyimpossible.org">kevin.phifer@theoreticallyimpossible.org</a>.
  </p>
</div>

<p><a href="../blog.html">&larr; Back to Blog</a></p>
```

### Footer + cookie banner
Copy verbatim from any existing post. Don't modify.

---

## SITREP vs long-form: how to decide

**SITREP** — single moment, single insight, dispatch tone. The reader gets one scene and one takeaway. Examples: *No One Reads the Warnings*, *Add MCP If Adding an API*, *Number Five Is Alive*. Word count 800–1,200. Tagged with `sitrep-tag` in post header and on the blog card. Title in feed.xml prefixed with `SITREP: `.

**Long-form essay or story** — multiple scenes, sustained argument, narrative arc. Examples: *The Framework*, *Build for Humans Hand to Agents*, *Why "Theoretically Impossible"?*. Word count 1,200–1,800. No SITREP tag.

If the piece has more than one story, more than one act, or builds a case rather than reporting a moment, it's long-form. When in doubt, ask Kevin.

---

## Voice and source fidelity

### Source fidelity

The transcript is the source of truth. If Kevin said he used Blender to make a model, don't write that he needed something "for a Blender scene." If he said the conversation about a virtual version happened after COVID, don't move it to before. If he described a back-and-forth where he suggested an idea, don't flip it so Claude suggested it.

The rule is simple: if the transcript says X, the article says X. Don't embellish, don't recharacterize, don't invent details that aren't in the source. You can tighten language, cut dead weight, and restructure for flow — that's editing. Changing what actually happened is fabrication.

When in doubt about a detail, ask Kevin rather than filling in the gap with something plausible.

### AI voice patterns to avoid

Kevin's voice is conversational, direct, and grounded in concrete detail. The articles should read like a guy telling you a story, not like a model producing content. Watch for these patterns and kill them on sight:

- **Staccato fragment chains.** "It could do the rough work. It couldn't do the fine work. Not because it lacked the knowledge. Because it lacked the eyes." Four short dramatic fragments in a row is a dead giveaway. Combine them into actual sentences.
- **Inspirational closers.** "And you move faster together than either of you could alone." If it sounds like it belongs on a motivational poster, cut it.
- **Hyperbolic praise of simple ideas.** "Every Unity tutorial ever written" or "every Stack Overflow thread about shaders and modifiers and inverse normals." Kevin doesn't talk like this. Keep claims grounded.
- **The "nobody is talking about this" crutch.** Overused across articles. Find a different way in, or just state the point directly.
- **Parallel structure stacking.** "I bring the thing only I can bring. Claude brings the thing only it can bring." Once is fine. Two or three in a row reads like a model admiring its own symmetry.
- **Filler qualifiers.** "the one that actually changes how you work day to day" — if the sentence works without the qualifier, drop it.

Read the draft out loud. If a sentence sounds like it was written to impress rather than to communicate, rewrite it.

---

## What not to do

- Don't update `blog.html`, `index.html`, `feed.xml`, or `sitemap.xml` during the draft phase. **Ever.**
- Don't decide SITREP vs long-form unilaterally for borderline pieces. Ask.
- Don't add the SITREP tag to a post that hasn't been confirmed as a SITREP.
- Don't invent a publication date. Use today's date, verified via shell if needed.
- Don't write the article body in chat. Write it in the HTML file.
- Don't propose three articles when Kevin asked for one. Discuss angles for the one piece, draft once.
- Don't restructure or "improve" existing published posts during a publish step. Touch only the publish surfaces.
- Don't skip `AskUserQuestion`. The angle conversation is the most important step in the workflow.

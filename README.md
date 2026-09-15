# Notes

A personal library of revision documents for software engineers preparing for
technical interviews. Each document is a PDF covering one topic, and the whole
library is searchable from a single page.

Live at [notes.rohitshukla.net](https://notes.rohitshukla.net). Excluded from
search engines by `robots.txt` and a `noindex` meta tag, so the link is the only
way in.

## What the documents are for

Revision, not teaching.

Before an interview, an engineer has to refresh a wide range of topics in a
short time. Studying each one from the start, or working through books and full
documentation, is not practical. These documents restore concepts the reader
already knows, quickly.

The topics are the ones a working engineer is asked about: Java, C, Go, Docker,
Kubernetes, databases, system design, distributed systems, and other subjects
asked about in technical interviews.

### How deep each document goes

A document covers the concepts that are:

- important to the topic,
- relevant to a technical interview,
- commonly discussed with a developer of around five years of experience,
- needed to explain the topic clearly to an interviewer.

Detailed enough to build real understanding. Short enough to revise quickly.

### Why the depth stops there

Full coverage is not the goal. A Kubernetes document does not explain every
feature or every configuration option. It covers what a developer with five
years of experience should understand and could reasonably be asked.

The rule is: cover what is necessary for interview preparation, not everything
that exists about the topic.

When a section is hard to cut, the test is whether a five-year engineer gets
asked about it, not whether it is true or interesting.

### Tone and language

The content is interview-ready. It has to help the reader explain a topic
clearly, precisely and simply, without unnecessary complexity and without
ambiguous statements. Explanations are easy to understand and easy to recall
under pressure. Academic language, unnecessary jargon and long explanations are
replaced by simpler ones wherever a simpler one is enough.

### Writing standard

All content follows the principles of ASD-STE100, Simplified Technical English,
wherever practical:

- simple and direct sentences,
- common words instead of complex alternatives,
- short sentences,
- active voice instead of unnecessary passive voice,
- no ambiguity,
- consistent terminology,
- technical terms explained where they are needed,
- structured, easy to scan layout.

Two further rules apply. Documents are written in the first person plural,
using *we* rather than *you*, as set out in `.claude/CLAUDE.md`. Code examples
are Java by default, because that is the interview language; a document about a
specific language uses that language instead.

### The goal

Help an engineer revise important technical concepts quickly, then explain them
with confidence in an interview. The content optimizes for understanding,
recall and clear communication, rather than for completeness.

## What the site does

Type a topic and open the document. Search ranks by relevance across titles and
keywords, opens the topics that hold matches, and puts the strongest match
first. The topic rail filters the list, everything expands or collapses at once,
and the query and topic both live in the URL, so a result can be shared as a
link.

Every document sits in the markup, so the library browses and every link works
with JavaScript switched off. JavaScript adds search, filtering and the feedback
form on top of that.

Two themes. The choice persists, and the page follows the operating system until
it is overridden. Contrast is verified at WCAG 2.2 AA in both.

On phones the topic rail is hidden and the accordion carries the navigation.

## Topics

Java & Spring, Go, System design, Infra & ops, SQL & data, DSA, Web, Interview &
career, AI, Workplace, Personal.

The rail shows a live count for each. Those numbers are not repeated here,
because a number in a README goes stale the first time a document is added.

## Structure

```
notes/
├── index.html                 The library. Every document is in this markup.
├── myPersonalDocs.html        Workplace documents, reachable by URL only
├── robots.txt                 Disallow: /
├── site.webmanifest           Icons and colours for an installed shortcut
├── favicon.ico, *.png         Icon set, at the root where browsers look for it
├── CNAME                      Custom domain for GitHub Pages
│
├── assets/
│   ├── css/
│   │   ├── tokens.css         Design tokens, both themes, base styles
│   │   └── library.css        Components
│   ├── js/
│   │   └── library.js         Search, filtering, accordion, theme, feedback
│   └── fonts/                 Calibri, self-hosted (woff2 upright, ttf italics)
│
├── guide/                     Interview revision documents, and the stylesheet
│   └── pdf-style.css          Single source of truth for how a PDF looks
│
├── notes/                     Earlier documents and the architecture diagrams
│   └── go-cheat-sheet/        Go, one topic per file
│
├── docs/                      Roadmaps and workplace training
│
├── .internal/
│   └── DESIGN-PROMPT.md       Reusable design prompt for future pages
│
└── .github/workflows/         static.yml, deploys the repo as-is to Pages
```

`guide/` is where new revision documents are written. It is empty at the moment,
apart from the stylesheet, while the library is rebuilt.

## Adding a document

Add one `<li>` to `index.html` inside the right topic, following the rows
already there:

```html
<li data-keys="lowercase title, aliases, filename, topic id, kind">
  <a class="row" href="/guide/your-file.pdf" target="_blank" rel="noopener" data-row>
    <span class="row-title">Your title</span>
    <span class="row-kind">Cheat sheet</span>
  </a>
</li>
```

`data-keys` is the search haystack. Anything a reader might type belongs in it,
including the filename. Keep both halves in sync: a word that appears in the
title but not in `data-keys` makes the row unfindable.

`row-kind` is one of Cheat sheet, Notes, Roadmap or Link.

Then update that topic's two count numbers, one in the rail and one on the
section heading. Those are the fallback for readers without JavaScript. The
total above the list is calculated at runtime and needs no edit.

## How a PDF is produced

Markdown is converted to HTML, then printed to PDF by headless Chrome. Page
setup is A4 with `printBackground` on and margins of 11/10/12/10 mm. The look
comes entirely from `guide/pdf-style.css`, which is tracked in git. Calibri is
the font, and hyphenation is switched off, so words are never split across
lines.

One known gap: only the rendered PDFs are committed. The markdown sources are
not in the repository, so editing a document means recovering its text first.

## Running it

No build step and no dependencies. It needs `http://` rather than `file://`,
because the CSS, JavaScript and fonts are referenced from the site root:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Stack

Plain HTML, CSS and JavaScript. No framework, no bundler, no CDN. Calibri is
self-hosted, and the feedback form posts to the EmailJS REST API rather than
loading their SDK. What is in git is what ships.

That form needs a live EmailJS connection. A 412 response means the Gmail grant
has expired and has to be reconnected from the EmailJS dashboard.

## Author

Rohit Shukla, [github.com/rohitshukla001](https://github.com/rohitshukla001)

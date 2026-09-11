# netzfisch.github.io

Personal tech blog, built by Jekyll 4 and published by GitHub Pages on every
push to `master`. Posts are CC BY-SA 4.0, everything else MIT (see `README.md`).

## Build and preview

    $ bundle exec jekyll serve      # http://127.0.0.1:4000, rebuilds on save
    $ bundle exec jekyll build      # writes _site/ (gitignored)

`future: false` in `_config.yml` hides posts dated after today, so a post that
does not appear locally usually has tomorrow's date.

## Layout

| Path | What |
|------|------|
| `_posts/` | one file per post, `YYYY-MM-DD-slug.md` (hyphens, lowercase) |
| `_layouts/`, `_includes/` | `default` page frame, `post` layout, sidebar widgets |
| `assets/` | CSS, fonts, images |
| `static/` | cheat sheets (`git.md`, `rails.md`, ...) rendered with `layout: default` |
| `archive.html`, `tags.html` | generated lists; nothing to edit when adding a post |
| `README.md` | public repo description and license |

`.textile` posts from 2010 stay as they are; new posts are Markdown (kramdown).

## Post format

Frontmatter is exactly these four keys, values aligned with spaces after the
colon, and the body starts on the line after the closing `---`:

    ---
    layout:    post
    category:  linux
    tags:      ubuntu ruby asdf 2cents
    title:     Ruby segfaults after an Ubuntu update - blame the new install
    ---
    First paragraph ...

- `category` is one of `linux`, `ruby`, `tools`, `web`. It becomes the first
  URL segment (`permalink: date`).
- `tags` are space separated, lowercase. `2cents` marks a short opinion note,
  `howto` a step-by-step guide. Every tag gets an anchor on `tags.html`.
- No `date` key; the filename carries the date.
- The first paragraph is the excerpt on the index page
  (`excerpt_separator: "\n\n"`), so it has to stand alone.
- Shell code is a 4-space indented block with a `$ ` prompt; Ruby uses
  `{% highlight ruby %}` ... `{% endhighlight %}`. Fenced backtick blocks are
  not used anywhere in `_posts/`.
- Links are reference style: `[text][1]` in the body, the numbered list of
  URLs at the very end of the file.
- Section headings are `###` (the post title is already an `<h3>` in the
  layout). Short posts have none.
- Wrap lines at 80 columns.
- Gists embed with `{% gist user/id %}` (plugin `jekyll-gist`); prefer inline
  code blocks for new posts so the content stays in the repo.

Voice and structure of a post are the job of the `private-blog-post-writer`
skill; this file only defines the format.

## Checks before committing a post

    $ bundle exec jekyll build 2>&1 | grep -i -E 'error|warn' ; true
    $ head -6 _posts/<file>.md          # four keys, aligned, layout: post
    $ grep -c '```' _posts/<file>.md    # expect 0

## Commits

One post per commit, message `post(<category>): <short title>` in lowercase
imperative mood, 50 characters or fewer. Cheat sheet edits use `static: ...`.

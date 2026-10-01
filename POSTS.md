# Publishing Research Posts

Posts are Markdown files rendered by GitHub Pages. To publish one:

1. Copy `_templates/research-post.md` to a new file in `posts/`, for example `posts/2026-10-01-firmware-notes.md`.
2. Fill in the title, short description, category, date, reading time, and a unique permalink ending in `.html`.
3. Write the article below the front matter using Markdown. Keep `show_on_home: true` to list it on the homepage.
4. Run `jekyll serve` and preview at `http://localhost:4000`.
5. Publish the change to GitHub Pages.

The homepage research list is generated from post front matter, so there is no separate card to keep in sync. The existing DVR article remains a standalone HTML page; its homepage card uses front matter at the top of that file.
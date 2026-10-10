# LCY's Notes

Built with [Hugo](https://gohugo.io/) and [PaperMod](https://github.com/adityatelange/hugo-PaperMod) at <https://blog.lcy.one/>.

## Local development

Use the Hugo version in `.github/workflows/pages.yml`.

```sh
git submodule update --init --recursive
hugo server
```

## Writing posts

```sh
hugo new content post/my-post.md
```

Edit the post and set `draft` to `false` when ready. Keep the title in front matter; start the body with a paragraph or `##` heading.

Use `<!--more-->` on its own line to end the homepage preview. PaperMod displays plain-text summaries in two lines. Without a divider, Hugo generates the summary automatically.

## Markdown output

We provide Markdown versions to make this blog easier for LLM agents to read.

- `/markdown/post/my-post.md`: the full post with its title, canonical URL, dates, and tags.
- `/llms.txt`: an index linking directly to Markdown posts.

Article HTML includes a Markdown alternate link. Exports preserve the Markdown source; Hugo shortcodes are not expanded.

## Customizations

| File | Why it exists | Basis |
| --- | --- | --- |
| `layouts/_partials/extend_head.html` | Hugo's Markdown output format conflicts with legacy `.html` URLs specified in front matter. This hook works around the conflict by publishing Markdown at `/markdown/post/*.md`, preserving relative links, and adding HTML alternate links. | [Hugo output formats](https://gohugo.io/configuration/output-formats/), [URL conflict discussion](https://discourse.gohugo.io/t/output-formats-with-basename-and-url-parameter/33625), [resource publishing](https://gohugo.io/methods/resource/permalink/), [PaperMod head hook](https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#custom-head--footer) |
| `layouts/home.llms.txt` | PaperMod's llms.txt links to HTML. This template links directly to Markdown posts so agents can fetch them without first reading HTML. | PaperMod's [llms.txt template](https://github.com/adityatelange/hugo-PaperMod/blob/d3768854d00ad003b0a8dbdba254ce9224377a01/layouts/llms.txt) and the [llms.txt proposal](https://llmstxt.org/#proposal). |
| `layouts/robots.txt` | PaperMod has no setting for Content Signals. This template declares search, AI input, and model training allowed. | PaperMod's [robots template](https://github.com/adityatelange/hugo-PaperMod/blob/d3768854d00ad003b0a8dbdba254ce9224377a01/layouts/robots.txt) and Cloudflare's [Content Signals](https://developers.cloudflare.com/bots/additional-configurations/managed-robots-txt/#content-signals-policy). |
| `assets/css/extended/home-info.css` | Removes excess space around our short homepage introduction caused by PaperMod's minimum height and bottom margin. | PaperMod's [Home Info styles](https://github.com/adityatelange/hugo-PaperMod/blob/d3768854d00ad003b0a8dbdba254ce9224377a01/assets/css/common/post-entry.css#L1) and [custom CSS support](https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#bundling-custom-css-with-themes-assets). |

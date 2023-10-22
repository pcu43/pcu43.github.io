---
layout: page
title: "Linux"
permalink: /linux
---

Linux Notes

So I learned this while trying to get Jekyll/GitHub Pages to work. Jekyll doesn't like UTF-8 encoding so the YAML frontmatter of my `.md` pages kept showing up on the HTML page. To fix that, I set `LANG` to `C` and the problem went away. Use `locale -a` to list all the available things you can set `LANG` to. I chose `C` because it looked safe. Safe just like the C programming language.

---
layout: post
published: false
title: for each file cli macos
---
I had a need to cycle through some files to convert the encoding today. Had to lookup how to do this from the cli/terminal on a mac.

``` shell
for f in ./_posts/2007*.md; do iconv -sc -f UTF-16LE -t UTF-8 $f > $f.markdown ; done
```

This is just a one liner to look at all my posts from 2007 and convert them from utf-16le encoding to utf-8 and rename them with markdown after. This worked ok for me, but sadly I still had to go back and manually fix a bunch of the posts.

I'm still not sure exactly what caused this to happen, but I suspect it is because I have been machine hopping while I convert the old blog.

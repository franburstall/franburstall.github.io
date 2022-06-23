---
title: Blogging with emacs, revisited
description: Second try at this
date: 2022-06-23 08:41:53 +0100
tags: emacs jekyll
---

My [first attempt]({% post_url
2022-06-19-org-capture-for-jekyll %}) was not so
satisfactory:
* Repetition: I had to enter the title twice, once in
  slugified form.
* Irritating: the cursor ended up in the wrong place.
* Too many moving parts: `org-capture` is a *huge* functions
  and I was hardly using any of its functionality.
  
Better then to start again and, being unable to sleep this
morning, that is what I did.  This time, my approach was to
use [yasnippet](https://melpa.org/#/yasnippet) to harvest
the title and construct the righteous file from that.

Here is the snippet:
```
# -*- mode: snippet -*-
# name: new-post
# key: new-post
# --
---
title: ${1:title$(if yas-moving-away-p (blog-post-jekyll-create-file yas-text))}
description: $2
date: `(format-time-string "%Y-%m-%d %T %z")`
tags: $3
---

$0
```
We create a buffer with a temporary name and call the
  snippet:
```emacs-lisp
(defun blog-post-new-post ()
  "Create a new jekyll post."
  (interactive)
  (switch-to-buffer "*post*")
  (markdown-mode)
  (yas-expand-snippet (yas-lookup-snippet "new-post")))
```
Having entered the title of the post, hit `TAB` amd 
  `blog-post-jekyll-create-file` uses the title to make a file
  with a 
  jekyll-compliant name and associates our buffer with it.
  
Details:
```emacs-lisp
(defun blog-post-jekyll-generate-filename (title)
  "Create a Jekyll-compliant filename from TITLE."
  (let* ((slug (s-join "-"
		       (--map
			(downcase (s-replace-regexp "[^[:alnum:]]" "" it))
			(s-split-words title))))
	 (time (format-time-string "%Y-%m-%d"))
	 (filename (format "%s-%s.md" time slug))
	 (path (file-name-concat blog-post-jekyll-post-directory filename)))
    path))

(defun blog-post-jekyll-create-file (title)
  "Create a file for a post from TITLE using current buffer."
  (let ((path (blog-post-jekyll-generate-filename title)))
    (set-visited-file-name path)))
```

All of this is pretty comfortable.  I can see a couple of
things to do later:

* A `completing-read` way to insert links to other posts.
* A way to change time-stamps in file and filename at once:
  we are currently recording the time I started the post not
  when I finished it!

  

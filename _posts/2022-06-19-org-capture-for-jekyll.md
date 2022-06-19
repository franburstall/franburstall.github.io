---
layout: post
title: Org capture for Jekyll posts
tags: emacs jekyll
description: Org capture is your friend, even when writing markdown
---

Jekyll is kind of particular about the format of filenames
for blog posts.  It must be of the form:
```
YYYY-MM-DD-your-title-here.md
```

Much better to have the machine take care of that sort of
thing and, since I live in `emacs`, let us leverage the
`org-capture` mechanism to make it happen.

There are two things you need to know about `org-capture`:

1. The target file need not be an `org` file.
2. You can use a function to compute the target file-name.

First we declare the target directory where posts live:
```emacs-lisp
(defvar my/jekyll-post-directory
  "~/projects/franburstall.github.io/_posts"
  "Directory containing blog posts.")
```
Now a function to compute the name of a new post and find
(open) it:
```emacs-lisp
(defun my/jekyll-new-blog-entry ()
  "Create new blog post file."
  (let* ((time (format-time-string "%Y-%m-%d"))
	 (title (read-string "Title: "))
	 (filename (format "%s-%s.md" time title)))
    (find-file 
	  (file-name-concat my/jekyll-post-directory filename))))
```

Finally, make a template and add it to
`org-capture-templates`:
``` emacs-lisp
(push '("b"
	"Blog entry"
	plain
	(function my/jekyll-new-blog-entry)
	"---\nlayout: post\ntitle: %?\ntags: \n---\n"
	:immediate-finish t
	:jump-to-captured t)
      org-capture-templates)
```
* The `plain` at line 3 says that this is not an `org` file.
* Line 4 says to use `my/jekyll-new-blog-entry` to get the
  file.
* The last two lines of the template tell `org` to by-pass the usual
  capture buffer and just give me the file buffer to write
  in.
  
This is not perfect yet.  Point is left at the top of the
buffer despite the `%?` in the template, for example.  But
it will totally do for now!

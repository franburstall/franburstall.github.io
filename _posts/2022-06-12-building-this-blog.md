---
layout: post
title: Building this blog
excerpt: I thought this would be easy
usemathjax: true
---
Use `jekyll` they said; you'll be up and running in no time!
Well, four days later, I have a working blog and I will
celebrate the fact by describing some of what it took to get
this far.

## Ruby

`jekyll` is a ruby app so first we need a ruby installation:
``` fish
sudo apt-get install ruby-full
```
I quickly realised that I do not want to preface every
interaction with `sudo` so we had better keep gems somewhere
local.  So pop the following in `~/.zshenv`:
``` sh
export GEM_HOME=$HOME/.gems
export PATH=$HOME/.gems/bin:$PATH
```

## Jekyll

We can now install `jekyll` and friends:
``` fish
gem install jekyll bundler
```
This causes many gems to be installed.

We can now begin the job we came here to do:
```fish
jekyll new blog
cd blog
```
Yet more gems are installed and we are finally ready to go:
``` fish
bundle exec jekyll serve
```
We head for  <http://localhost:4000> and it works, by God!
We have a local copy of a blog to practice on.

## Changing theme

This is where the poo impacts the propeller.  The default
theme is [minima](https://github.com/jekyll/minima) which is
jolly nice but I want a **dark** theme.  I look around,
like
[midnight](https://github.com/pages-themes/midnight) and so
follow the instructions to install it.

There follows a period of chaos where nothing worked and I
enter gem dependency hell (an issue with the `faraday`
gem).  In the end, thanks to the
[internet](https://stackoverflow.com/questions/59553433/cannot-run-github-pages-locally), the relevant part of `_config.yaml` looks
like
```yaml
remote_theme: pages-themes/midnight@v0.2.0

plugins:
  - jekyll-feed
  - jekyll-remote-theme
```
and `Gemfile` like
```ruby
gem "github-pages"
gem 'faraday', '~> 0'
# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end
```
Now
```fish
bundle update
```
makes everything happy again.

We fire up the local server once more:
```fish
bundle exec jekyll serve --livereload
```
The `--livereload` makes pages in the browser auto-refresh
when you save your markdown source: lovely instant feedback!

### Where have all the layouts gone?

At this point, you realise that that any test posts you made
aren't working any more because they rely on layouts like
`post` and `page` that the `minima` theme provides but
`midnight` does not.  Time to ask the internet for help!

Two very helpful sources of information:

* [Surface
  Detail](https://surfacedetail.blogspot.com/2019/04/github-pages-and-jekyll-themes.html)
* [Natalya
  Osenko](https://www.natalyakosenko.com/2017-12-23-how-to-switch-jekyll-theme-on-github-pages-site)
  
From these I learn that I must steal the missing layouts
from `minima` and bend them to my will.  But where to steal
from?  `minima` is hidden away in some gem.  `bundle` to the
rescue:
```fish
bundle info minima
```
tells us the location of the gem and now we can loot to our
heart's content.

## How to get mathjax working

I am a mathematician and may want to talk maths here some
day.  For this we need [mathjax](https://www.mathjax.org)
and this does not work out of the box.  You need to inject
some `js` into the `head` of your pages.  For `midnight`,
you do this by adding to `_includes/custom_head.html`
thusly:
{% raw %}
```html
{% if page.usemathjax %}
<script type="text/x-mathjax-config">
MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript" async
src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
	</script>
{% endif %}
```
{% endraw %}
Now you can do in-line maths by wrapping it in single dollar signs
like this: $e^{\pi i}=-1$. Do display math with double
dollars in
a new paragraph:

$$
\int_0^x t\,dt=x^2/2.
$$

All this so long as you set
```yaml
usemathjax: true
```
in the `yaml` header of your post.

## Going live

I confess that I thought this was going to be a world of pain
but setting up GitHub Pages was a breeze and, once I had
pushed the local repo back to GitHub, everything Just
Worked.  GitHub automatically rebuilds the site every time
you push changes: very sweet.

## Resources

In this adventure, the internet was truly my friend.  Here
are some of the other things I found helpful:
* The official [Jekyll docs][jekyll-docs] are really very
  good.
* `sass` is super-charged `css` and was new to me.  Their
  [docs](https://sass-lang.com/guide) are excellent
  though.
* [Here](https://www.aleksandrhovhannisyan.com/blog/getting-started-with-jekyll-and-github-pages)
  is another blog on setting up `jekyll` which is pretty
  hard-core: he suggests wiping all the layouts and building
  your own from scratch.  Lots of useful information here
  for further perusal.
  


[jekyll-docs]: https://jekyllrb.com/docs/home

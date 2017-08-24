[![Build Status](https://travis-ci.org/savaslabs/savaslabs.github.io.svg?branch=source)](https://travis-ci.org/savaslabs/savaslabs.github.io)

This is the website for [Savas Labs](https://savaslabs.com).

The site is built using Jekyll and Gulp.

## Installation

### Install dependencies

1. [Bundler](http://bundler.io): `gem install bundler`
2. [Jekyll](http://jekyllrb.com/docs/installation/): `gem install jekyll`
3. [node.js and npm](https://docs.npmjs.com/getting-started/installing-node). For npm, you should be running at least major version 3. To update npm to the latest version, run `npm install npm@latest -g`.
4. [Gulp](https://github.com/gulpjs/gulp): `npm install -g gulp`
5. [ImageMagick](http://www.imagemagick.org/script/index.php): `brew install imagemagick`
6. [Proselint](https://github.com/amperser/proselint/): `pip install proselint`
7. [Aspell](http://aspell.net/): `_scripts/aspell.sh`. This script will install and configure Aspell.

### Site setup

1. Clone the repo (default branch is `source`; do not use master.)
2. Run `git config core.hooksPath hooks`. This configures git to use hooks inside the `hooks` directory instead of `.git/hooks`.
3. Run `bundle install` (or `bundle install --path vendor/bundle` if preferred)
4. Run `npm install` to install node modules. This takes a few minutes.

## Local development

To serve the site, run `gulp serve`. If you do not have Gulp installed locally
then you can install it globally via `npm install -g gulp`. This uses the test
and dev config files for local development.

If needed, run `npm install` to install any modules that were added since you last served the site.

Thanks to `gulp.watch` and BrowserSync, any changes you make will trigger Gulp
to either regenerate the Jekyll site and automatically refresh your browser or,
if they're changes to CSS or images, inject the updated file(s) so a refresh
isn't needed. It's pretty cool!

You can toggle some options in the gulpfile:

- In the `serve` task, change `ghostMode` to `true` if you want to mirror clicks,
reloads, etc. across browsers. Useful for testing, hard on performance.
- In the `serve` task, change `open` to `false` if you don't want BrowserSync to
automatically open a browser window for you when you serve the site.
- TODO: allow devs to use the `--drafts` tag to show works in progress. For now
you can update the build command in the `build:jekyll:local` task to include the
`--drafts` tag.

## Git workflow

### Branches

Because we're using Travis to build the site and push to master, which is then
deployed by GitHub Pages, the master branch contains the generated `_site`
directory. The main development branch you should pull from and open pull
requests against is `source`.

### Pre-commit hook

The pre-commit hook located in the `hooks` directory does the following on all staged HTML and markdown files:

1. Checks spelling with Aspell
2. Runs markdown-lint (mdl)
3. Runs proselint

To add words to the custom dictionary:

1. Add the words to `savas_wordlist.txt` (one word per line)
2. Run `_scripts/aspell.sh` to update the dictionary.

To ignore the tests and commit (only recommended for proselint errors):

1. Run `git commit --no-verify`

## Tests

To run the tests:

`$ bash _scripts/run-tests.sh`

## Writing blog posts

### Headings

Your post title (stored in the post's front matter) will be an H1. Your
top-level headings should be H2's (##), then H3's (###), etc.

### Images

You can include a featured image in the front matter using the `featured_image`
and `featured_image_alt` keys. This will work for our site and for Drupal Planet.
Please try to do this for every post!

[UPDATE THIS] Your image should be 1474px wide. Blog images should be placed in
`_/assets/img/blog`, but you should only include `/blog/[filename].jpg` in the
front matter of your post.

Lastly, please include `featured_image_width` and `featured_image_height` in
pixels to satisfy Google's structured data requirements.

#### Image placement and size within the post body

You can add images with the following markdown syntax:

```
![Image alt text here]({{ site.url }}/assets/img/blog/image-name.jpg){:class="blog-image-full-width"}
```

You can add following classes to contextual images:
- `class="blog-image-full-width"` - image will span the whole width of the post
- `class="blog-image-left"` - image will float left
- `class="blog-image-right"` - image will float right

Images without class will use default styling which is 100% post width.

Captions can be added to full width images by adding `<span class="caption">Caption</span>` right under the image tag.

To add caption to floated images wrap <img> tag in a div and give it a class. E.g.

``` html
<div class="blog-image-left">
 <img src="/assets/img/blog/{img}" alt="{alt}">
  <span class="caption">{caption}</span>
</div>
```

## Syntax Highlighting

Since updating to Jekyll 3.0.2 which uses Kramdown/Rouge, to use syntax
highlighting in a post you just need to use backticks (similar to GitHub or
Slack highlighting).

Special tips:

1. You can include the language name after the first set of backticks with no
space e.g. ```bash
2. The syntax block must be proceeded and followed by blank lines.
3. For php you must including an opening php tag to get proper highlighting.
4. To display raw Liquid code, you'll need to wrap your code in {% raw %} and
{% endraw %} tags.

## Tags

To add a new tag, complete the following:

1. Add the tag to _data/tags.yml.
2. Add a new markdown file for the tag in blog/tag. This creates a page for posts with that tag.
3. Add the tag to the front matter of your post.

## Twitter widget

We're using the jekyll-twitter-plugin to allow us to use a simple Liquid tag to
add Twitter widgets to blog posts or other pages. Use the following format
within HTML or markdown files:

```
{% twitter https://twitter.com/Savas_Labs/status/729504649233899521 %}
```

## Comments

Comments are handled by a Dockerized instance of [Squabble](https://github.com/savaslabs/squabble). Please refer to that repositories README for notes on usage, and to the [wiki page](https://pm.savaslabs.com/projects/savaslabs/wiki/Squabble) for production deployment information.

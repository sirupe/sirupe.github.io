# jekyll-toc

[![Build Status](https://travis-ci.org/toshimaru/jekyll-toc.svg?branch=master)](https://travis-ci.org/toshimaru/jekyll-toc)
[![Gem Version](https://badge.fury.io/rb/jekyll-toc.svg)](http://badge.fury.io/rb/jekyll-toc)
[![Dependency Status](https://gemnasium.com/toshimaru/jekyll-toc.svg)](https://gemnasium.com/toshimaru/jekyll-toc)
[![Code Climate](https://codeclimate.com/github/toshimaru/jekyll-toc/badges/gpa.svg)](https://codeclimate.com/github/toshimaru/jekyll-toc)
[![Test Coverage](https://codeclimate.com/github/toshimaru/jekyll-toc/badges/coverage.svg)](https://codeclimate.com/github/toshimaru/jekyll-toc/coverage)

# Installation

Add jekyll-toc plugin in your site's `Gemfile`, and run `bundle install`.

```ruby
gem 'jekyll-toc'
```

Add jekyll-toc to the `gems:` section in your site's `_config.yml`.

```yml
gems:
  - jekyll-toc
```

Set `toc: true` in posts for which you want the TOC to appear.

```yml
---
layout: post
title: "Welcome to Jekyll!"
toc: true
---
```

# Usage

There are three Liquid filters, which can be applied to HTML content,
e.g. the Liquid variable `content` available in Jekyll's templates.

## 1. Basic Usage

### `toc` filter

Add the `toc` filter to your site's `{{ content }}` (e.g. `_layouts/post.html`).

```liquid
{{ content | toc }}
```

This filter places the TOC directly above the content.

## 2. Advanced Usage

If you'd like separated TOC and content, you can use `toc_only` and `inject_anchors` filters.

### `toc_only` filter

Generates the TOC itself as described [below](#generated-table-of-contents-html).
Mostly useful in cases where the TOC should _not_ be placed immediately
above the content but at some other place of the page, i.e. an aside.

### `inject_anchors` filter

Injects HTML anchors into the content without actually outputing the
TOC itself. They are of the form:

```html
<a id="heading11" class="anchor" href="#heading1-1" aria-hidden="true">
  <span class="octicon octicon-link"></span>
</a>
```

This is only useful when the TOC itself should be placed at some other
location with the `toc_only` filter.

## Generated HTML

jekyll-toc generates an unordered list. The final output is as follows.

**Heads-up**: As of v0.3, nested toc has been supported. If you update jekyll-toc from 0.2 to 0.3, please check [upgrade guide](https://github.com/toshimaru/jekyll-toc/wiki/0.3-Upgrade-Guide)

```html
<ul class="section-nav">
  <li class="toc-entry toc-h1"><a href="#heading1">Heading.1</a>
    <ul>
      <li class="toc-entry toc-h2"><a href="#heading1-1">Heading.1-1</a></li>
      <li class="toc-entry toc-h2"><a href="#heading1-2">Heading.1-2</a></li>
    </ul>
  </li>
  <li class="toc-entry toc-h1"><a href="#heading2">Heading.2</a>
    <ul>
      <li class="toc-entry toc-h2"><a href="#heading2-1">Heading.2-1</a>
        <ul>
          <li class="toc-entry toc-h3"><a href="#heading2-1-1">Heading.2-1-1</a></li>
          <li class="toc-entry toc-h3"><a href="#heading2-1-2">Heading.2-1-2</a></li>
        </ul>
      </li>
      <li class="toc-entry toc-h2"><a href="#heading2-2">Heading.2-2</a></li>
    </ul>
  </li>
</ul>
```

It looks like the image below.

![screenshot](https://user-images.githubusercontent.com/803398/28401295-0dcfb7ca-6d54-11e7-892b-2f2e6ca755a7.png)

## CSS Styling

The toc can be modified with CSS. The sample CSS is the following.

```css
.section-nav {
  background-color: #FFF;
  margin: 5px 0;
  padding: 10px 30px;
  border: 1px solid #E8E8E8;
  border-radius: 3px;
}
```

![screen shot](https://user-images.githubusercontent.com/803398/28401455-0ba60868-6d55-11e7-8159-0ae7591aee66.png)

Each TOC `li` entry has two CSS classes for further styling.
The general `toc-entry` is applied to all `li` elements in the `ul.section-nav`.
Depending on the heading level each specific entry refers to, it has a second
CSS class `toc-XX`, where `XX` is the HTML heading tag name.

For example, the TOC entry linking to a heading `<h1>...</h1>` (a single
`#` in Markdown) will get the CSS class `toc-h1`.

That way, one can tune the depth of the TOC displayed on the site.  
The following CSS will display only the first two heading levels and hides
all other links:

```css
.toc-entry.toc-h1,
.toc-entry.toc-h2
{}
.toc-entry.toc-h3,
.toc-entry.toc-h4,
.toc-entry.toc-h5,
.toc-entry.toc-h6
{
  display: none;
}
```

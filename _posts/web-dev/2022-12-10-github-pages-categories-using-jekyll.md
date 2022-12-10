---
layout: post
title: Jeykyll을 이용해 Github Pages에 Categories 추가하기
subtitle: Liquid Template 빡세다
categories: [web-dev]
tags: [jekyll, liquid, blog, html]
comments: true
sitemap:
  changefreq: daily
---

{: .box-note}
**Note:** 제 Blog는 `Beatiful Jekyll`Theme를 기반으로 만들어졌습니다.

# Blog 우측 상단 Navbar에 Category를 누르면 Category에 속한 Posts들이 뜨도록 하고 싶다


생각을 해보자. 내 Blog Home화면에 들어가면 내가 이때까지 작성했던 Post들이 가장 최신 순부터 주르륵 뜬다. 그러면 이 기능을 이용하면 Category Page를 만들 수 있지 않을까?

# Github Pages에서는 Home화면에서만 Paginate를 지원한다

[stack overflow QnA](https://stackoverflow.com/questions/56065176/how-to-paginate-categories-in-jekyll-with-github-pages)
스택오버플로우 QnA에 다음과 같이 말한다.

> Paginate only paginates all posts and not by category or tag.

>Paginate V2 does this even for collections. But you cannot run this plugin on Github pages ([allowed plugins](https://pages.github.com/versions/)).
>
>Two solutions :
>
>1.  publish your code in a branch, generate your site locally (or with a service like [Travis Continuous Integration](https://github.com/marketplace/travis-ci) that is free for open source projects) and publish (or let your CI publish) your generated code in another branch.
>  
>2.  use a modern hosting provider like [Netlify](https://netlify.com/) that allows you to use any plugin.

그래서 Paginate는 포기했고, 그냥 Category Page를 클릭하면 Category에 속한 글들이 가장 최신 글부터 주르륵 뜨도록 했다.

# `/_layouts/`에 `category.html`파일을 작성하자

{% raw %}
```ruby
---
layout: page
---

{{ content }}

{% assign CATEGORY = page.title %}
{% assign posts = site.categories[CATEGORY] %}

<!-- role="list" needed so that `list-style: none` in Safari doesn't remove the list semantics -->
<ul class="posts-list list-unstyled" role="list">
	{% for post in posts %}
	<li class="post-preview">
		<article>

			{%- capture thumbnail -%}
			{% if post.thumbnail-img %}
			{{ post.thumbnail-img }}
			{% elsif post.cover-img %}
			{% if post.cover-img.first %}
			{{ post.cover-img[0].first.first }}
			{% else %}
			{{ post.cover-img }}
			{% endif %}
			{% else %}
			{% endif %}
			{% endcapture %}
			{% assign thumbnail=thumbnail | strip %}

			{% if site.feed_show_excerpt == false %}
			{% if thumbnail != "" %}
			<div class="post-image post-image-normal">
				<a href="{{ post.url | absolute_url }}" aria-label="Thumbnail">
					<img src="{{ thumbnail | absolute_url }}" alt="Post thumbnail">
				</a>
			</div>
			{% endif %}
			{% endif %}

			<a href="{{ post.url | absolute_url }}">
				<h2 class="post-title">{{ post.title | strip_html }}</h2>

				{% if post.subtitle %}
				<h3 class="post-subtitle">
					{{ post.subtitle | strip_html }}
				</h3>
				{% endif %}
			</a>

			<p class="post-meta">
				{% assign date_format = site.date_format | default: "%B %-d, %Y" %}
				Posted on {{ post.date | date: date_format }}
			</p>

			{% if thumbnail != "" %}
			<div class="post-image post-image-small">
				<a href="{{ post.url | absolute_url }}" aria-label="Thumbnail">
					<img src="{{ thumbnail | absolute_url }}" alt="Post thumbnail">
				</a>
			</div>
			{% endif %}

			{% unless site.feed_show_excerpt == false %}
			{% if thumbnail != "" %}
			<div class="post-image post-image-short">
				<a href="{{ post.url | absolute_url }}" aria-label="Thumbnail">
					<img src="{{ thumbnail | absolute_url }}" alt="Post thumbnail">
				</a>
			</div>
			{% endif %}

			<div class="post-entry">
				{% assign excerpt_length = site.excerpt_length | default: 50 %}
				{{ post.excerpt | strip_html | truncatewords: excerpt_length }}
				{% assign excerpt_word_count = post.excerpt | number_of_words %}
				{% if post.content != post.excerpt or excerpt_word_count > excerpt_length %}
				<a href="{{ post.url | absolute_url }}" class="post-read-more">[Read&nbsp;More]</a>
				{% endif %}
			</div>
			{% endunless %}

			<!-- Categories 삽입 -->
			{% if post.categories.size > 0 %}
			<div class="blog-categories">
				<span>Categories:</span>
				<!-- role="list" needed so that `list-style: none` in Safari doesn't remove the list semantics -->
				<ul class="d-inline list-inline" role="list">
					{% for category in post.categories %}
					<li class="list-inline-item">
						<a href="{{ '/categories/' | append: category | absolute_url}}">{{- category -}}</a>
					</li>
					{% endfor %}
				</ul>
			</div>
			{% endif %}
			<!-- Categories 삽입 -->

			{% if site.feed_show_tags != false and post.tags.size > 0 %}
			<div class="blog-tags">
				<span>Tags:</span>
				<!-- role="list" needed so that `list-style: none` in Safari doesn't remove the list semantics -->
				<ul class="d-inline list-inline" role="list">
					{% for tag in post.tags %}
					<li class="list-inline-item">
						<a href="{{ '/tags' | absolute_url }}#{{- tag -}}">{{- tag -}}</a>
					</li>
					{% endfor %}
				</ul>
			</div>
			{% endif %}

		</article>
	</li>
	{% endfor %}
</ul>
```
{% endraw %}
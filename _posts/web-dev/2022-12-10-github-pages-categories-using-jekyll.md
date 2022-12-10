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


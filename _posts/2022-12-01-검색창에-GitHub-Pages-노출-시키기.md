---
layout: post
title: 검색창에 GitHub Pages 노출 시키기
subtitle: 구글링할 때 내 블로그가 나오길...
tags: [blog]
comments: true
---

# Google Search Console

<https://search.google.com/search-console/about>에 접속을 한다.

![Google Search Console](../assets/img/2022-12-01-검색창에-GitHub-Pages-노출-시키기/google-search-console.png)

시작하기 버튼을 눌러준다.

![URL 접두어 선택](../assets/img/2022-12-01-검색창에-GitHub-Pages-노출-시키기/url-접두어.png)

필자는 `<GitHub Username>.github.io` URL을 쓰기 때문에 접두어를 선택했다. 

![html 다운로드](../assets/img/2022-12-01-검색창에-GitHub-Pages-노출-시키기/html-다운로드.png)

URL에 본인 블로그 URL을 입력하면, 다음과 같은 모달창이 뜬다. html파일을 다운로드하고, 본인 블로그 repository의 root directory(`/`)에 저장을 한다.

![html live-server 결과](../assets/img/2022-12-01-검색창에-GitHub-Pages-노출-시키기/html-live-server.png)

live-server로 방금 저장한 html을 열어보고, 다음과 같이 잘 뜨면 등록이 잘 된 것이다.

# `sitemap.xml` 생성하기

{% raw %}
```ruby
---
layout: null
---

<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
      {% endif %}

      {% if post.sitemap.changefreq == null %}
        <changefreq>weekly</changefreq>
      {% else %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
      {% endif %}

      {% if post.sitemap.priority == null %}
          <priority>0.5</priority>
      {% else %}
        <priority>{{ post.sitemap.priority }}</priority>
      {% endif %}

    </url>
  {% endfor %}
</urlset>
```
{% endraw %}

liquid template을 이용하여 위와 같이 `sitemap.xml`을 작성하여, root directory(`/`)에 저장하고, 원격 저장소에 push하자.




---
layout: post
title: Obsidian 사용 팁
subtitle: 생각날 때마다 노트하기
categories: [etc]
tags: [obsidian]
comments: true
sitemap:
  changefreq: daily
---
# Code 복붙 할 때 `Ctrl + Shift + V`

`Ctrl + V`로 붙여넣으면, 코드-코드 사이에 공백이 들어가서 코드 라인이 졸지에 2배가 돼 버린다.

# Obsidian Markdown에서 두 단어 이상 한글 Header ID 안됨

- [안녕](#안녕)
- [잘 가](#잘-가)

# 안녕

안녕

# 잘 가

잘 가

```
![안녕](#안녕)
![잘 가](#잘-가)

#안녕

안녕

#잘 가

잘 가
```

위의 경우 `#안녕` 한글 Header ID는 Obsidian에서 인식이 되지만, `#잘-가` 한글 Header ID는 Obsidian에서 인식이 안된다. (다른 Markdown 문서에서는 문제 없이 잘 된다. 심지어 이 블로그 포스팅에서도 잘 된다. 오직 Obsidian에서만 안된다.)
---
layout: post
title: Windows에서 miktex 설치하기
subtitle: 윈도우에서 pandoc으로 md to pdf 하려니 오류가 나서...
categories: [etc]
tags: [windows, miktex, pandoc, winget]
comments: true
sitemap:
  changefreq: daily
---

```powershell
winget install --exact --id MiKTeX.MiKTeX
```

![](/assets/img/2023-03-15-windows-miktex-install/package_installation.png)
<https://stackoverflow.com/questions/29240290/pandoc-for-windows-pdflatex-not-found>
<https://github.com/jgm/pandoc/issues/8183>

```powrshell
[WARNING] Missing character: There is no 蹂?pandoc.exe: <stderr>: hPutChar: invalid argument (Invalid argument)
```

<https://stackoverflow.com/questions/18178084/pandoc-and-foreign-characters>

```powershell
pandoc --pdf-engine=xelatex -V CJKmainfont="한글 지원 폰트" test.md -o test.pdf
```
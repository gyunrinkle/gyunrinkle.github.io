---
layout: post
title: nextjs 13 + content layer 조합으로 blog migration
subtitle: 멀고 먼 길 험난한 길
categories: [web-dev]
tags: [nextjs, content-layer]
comments: true
sitemap:
  changefreq: daily
---

# Next.js 13 Project 만들기
```powershell
npx create-next-app contentlayer-example
```

# 방금 만들었던 Next.js 13 Project 실행

```powershell
npm run dev
```

# Contentlayer 설치
```powershell
npm install contentlayer next-contentlayer
```

어 근데? 오류가 난다...
```
Conflicting peer dependency: @opentelemetry/api@1.4.1
node_modules/@opentelemetry/api
  peerOptional @opentelemetry/api@"^1.4.0" from next@13.2.4
  node_modules/next
    next@"13.2.4" from the root project
    peer next@"^12 || ^13" from next-contentlayer@0.3.0
    node_modules/next-contentlayer
      next-contentlayer@"*" from the root project

Fix the upstream dependency conflict, or retry
this command with --force or --legacy-peer-deps
to accept an incorrect (and potentially broken) dependency resolution.
```

구글링 해보니깐 `--legacy-peer-deps` 옵션을 `npm install`할 때 붙이라 해서, 붙이고 해결했다...
```powerhsell
 npm install contentlayer next-contentlayer --legacy-peer-deps
```

# `next.config.mjs` 파일 생성 후 하기와 같이 코드 입력
```mjs
// next.config.mjs

import { withContentlayer } from 'next-contentlayer'

export default withContentlayer({})
```

# `tsconfig.json` 변경
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "contentlayer/generated": ["./.contentlayer/generated"]
      // ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    }
  },
  "include": ["next-env.d.ts", "**/*.tsx", "**/*.ts", ".contentlayer/generated"]
  //                                                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
}
```
`^` 부분을 추가
```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./*"],
      "contentlayer/generated": ["./.contentlayer/generated"]
    }
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx",
    ".next/types/**/*.ts",
    ".contentlayer/generated"
  ],
  "exclude": ["node_modules"]
}

```
# 참고 자료

- <https://www.youtube.com/watch?v=Gh-AT4p52PI>
- <https://www.contentlayer.dev/docs/getting-started#2-install-contentlayer>
- <https://github.com/kfirfitousi/blog>

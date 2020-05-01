---
layout: post
title:  "Github page 사용하기"
date:   2020-05-01
categories: Github React
---
Github pages에 react 프로젝트 배포하기

1. gh-pages 설치
```
    npm install gh-pages
```


2. package.json 아래 내용 추가
```
    "homepage": "https://<username이름>.github.io/<repository 이름>/",
    "scripts": {
        ...
        "predeploy": "npm run build",
        "deploy": "gh-pages -d build"
    }
```


3. npm run deploy 실행


example)<https://spro1.github.io/study_react/>
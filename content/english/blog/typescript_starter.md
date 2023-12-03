---
title: "타입스크립트(Typescript) 스타터 코드"
images:
  - "images/post/ts_logo.png"
date: 2022-09-04T18:19:25+06:00
author: "Joone Hur"
tags: ["TypeScript"]
categories: ["Programming"]
draft: false

---
프로젝트가 반복되면 재사용되는 코드가 많아질 수 밖에 없다. 여러가지 툴을 타입스크립트를 작성하다 보니 그 동안 공통적으로 사용하고 있는 기능을 묶어서 프로젝트로 만들어서 github에 공개해봤다.

* https://github.com/joone/typescript-template

물론,  MS가 만든 비슷한 프로젝트도 있는데, 너무 기능이 많고, 내가 원하는 수준에서 만들었는데, 아마도 필요한 분이 있을것 같다.
일단,  프로젝트는 clone하면 기본적으로 아래 package를  사용할 수 있고, 간단한 웹서비스가 구현되어 있다.

* express.js
* eslint
* mocha
* chai
* prettier
* typeorm
* mongodb
* Docker

MongoDB를 사용하는 방법이 두가지 있는데, typeorm와 mongoose를 사용한 것 코드를 별도 branch로 분리해두었다. typeorem이 쓰기 편해도 MongDB에는 mongoose가 코드는 복잡해도 더 쉬운 것 같다.
Start your web application:

```bash
$ npm start
```

Run test cases:
```bash
$ npm test
$ npm run-script lint
```

Docker image를 만들어 실행하는 부분도 추가했고, prettier도 지원한다.
Run prettier to fix styles:
```bash
$ npm run style
```
Build a Docker image and run the container:
```bash
$ sudo ./build_docker.sh
$ sudo docker ps
$ curl -v http://localhost:3001
```
향후 계획은 게시판 정도 실행되도록 하는 것이 목표다.
		
			
출처: [주네의 열린 소프트웨어:티스토리](https://opensoftware.tistory.com/entry/타입스크립트Typescript-스타터-코드)
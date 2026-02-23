---
title: "My Contributions to Chrome and Edge M145: Long Animation Frames and Drag-and-Drop Fix"
date: 2026-02-23T21:30:00-08:00
author: "Joone Hur"
tags: ["chromium", "web performance", "drag-and-drop", "open source"]
categories: ["web"]
draft: false
---

최근 릴리스된 Chrome 및 Edge 브라우저 (M145)에 제가 기여한 기능 추가 및 버그 픽스가 포함되었습니다. 아마 이미 X에서 공유해서 이미 내용을 알고 있는 분들도 있겠지만, 잠깐 내용을 공유해보록 하겠습니다.

이번 릴리스가 의미가 있는 것이 제가 거의 2년전에 마이크로스프트 아웃룩팀에 조인하고 아마도 처음 제대로 된 결과물이 크롬/에지 브라우저에 포함되는 것 같습니다. 왜 이렇게 오랜 시간이 걸리는지 궁금할 수 있는데, 패치의 난이도에 따라 리뷰가 몇개월씩 걸리고 실제 프로덕션 환경에서 몇달씩 테스트하는 경우가 있어서 작은 버그 수정도 실제 릴리스에 반영되는데는 1년 넘게 걸릴 수도 있습니다.

## Long Animation Frame (Web Performance API)

첫번째는 Web Performance API(Long Animation Frame)에 관한 부분입니다. 크로미움 기반 브라우저에서 여러가지 Web Performance API가 있는데, 웹앱이 실행되면서 각종 성능 관련된 지표를 실시간으로 수집합니다. 이 정보는 실제로 성능이 지연된 코드 위치를 알려주는데, 특정 경우(Promise 처리), 이 위치가 누락된 경우가 많아서 실제 웹앱 UI 지연(업데이트, 응답) 관련해서 문제 해결에 어려움을 주었습니다.

사실 이 부분은 성능상 이유로 구현이 안되었는데, 동작하는 코드에서 실제 느리게 동작하는 자바스크립트 위치를 찾는다는 것은 당연히 브라우저에 동작에 큰 부담을 줍니다. 결국, 성능 최적화를 위해 Blink엔진 GC부터 V8 engine까지 수정하고 2개월 동안 필드 테스트끝에 성능 regression이 없다는 것이 확인되어 이제야 기능이 활성화되었습니다.

하여간 팀이 원하는 기능이 드디어 릴리스되었고 이 모든 것은 아웃룩 뿐만 아니라 웹 커뮤니티 및 모든 사용자가 누릴 수 있다는 것이 오픈소스에 매력이 아닌가 싶습니다.

## Drag-and-Drop 버그 수정

두번째는 Drag-and-drop 문제입니다. 웹표준에는 여러가지 Drag type이 정의되어 있는데, text, link, element등이 있습니다. link의 경우 URL을 drag-and-drop할 수 있는데, 표준에는 동시에 여러 URL을 처리하도록 되어 있지만, 크로미엄 계열 브라우저는 여러개를 drag-and-drop하는 경우 모든 URL이 하나로 합쳐지는 아주 오래된 버그가 있었습니다. 이 문제는 다른 drag-and-drop 이슈를 해결하다가 간단해 보여서 사이드로 버그를 수정했는데, 북마크 drag-and-drop과 엮기면서 나중에는 본일 보다 훨씬 더 난이도 커져버린 일이 되고 말았습니다. 이 버그를 수정하면서 오래된 버그가 안고쳐질때는 다 이유가 있다는 것을 새삼 깨닫게 되었습니다.

관심있는 분은 이 문서를 보세요: [Design Doc](https://docs.google.com/document/d/1zNvn2hQOfMCc9-L9_WtHBFW9BSgOwDzHJGYRjEY6Y3c/edit)

---
layout: post
title: "[Maven]Maven(package) Build"
date: 2021-03-17 09:43:23 +0900
category: maven
---

# [Maven]Maven(package) Build

- Maven에서는 clean, build, site의 세 가지 Lifecycle을 제공하고 있다.
- 컴파일(compile), 테스트(test), 패키지(package), 배포(depooy)등의 과정은 빌드 Lifecycle에 속한다.
- Maven은 모든 빌드 단위에 대한 Lifecycle이 예약되어 있어서 **개발자가 임의로 변경 할 수 없다.**
- 각 Lifecycle은 순서를 갖는 단계(phase)로 구성된다.
- Maven의 기본 Lifecycle을 이해하려면 Phase와 Goal의 개념을 이해해야 한다.

![alt text](/public/img/maven_image_file/maven_build1.png)

참고 URL : 

[Maven Lifecycle](http://wiki.gurubee.net/display/SWDEV/Maven+Lifecycle)
---
layout: post
title: 天鳳 Web版 Code injection (자바스크립트 코드 변경)
date: 2016-03-23T15:47:00+09:00
categories: [Javascript]
description: Javascript code injection
comments: false
tags: [javascript]
---
![image](https://user-images.githubusercontent.com/8151366/42277680-2c6f4c44-7fd3-11e8-9372-15206d9206cc.png)
![image](https://user-images.githubusercontent.com/8151366/42277629-fa16454a-7fd2-11e8-996b-e5a8247c0b7d.png)
완벽하게 작동하지 않는다. Global 변수를 정해서 몇가지 기본 기능을 제공하고 Object 내에 injc() 함수를 만들면 그것을 자동으로 함수 내에 삽입해주는 것인데, 더 괜찮은 방법을 찾지 못했다.

함수의 첫 부분, 마지막 부분, 함수의 Override 를 지원하고있다.

개발 당시에는 과거 천봉 윈도우 버전 plugin

![image](https://user-images.githubusercontent.com/8151366/42277712-3e50807c-7fd3-11e8-91d9-56224069b214.png)

이것과 비슷하게 개발할 수 있을 것이라 생각했었다. Windows 버전은 MFC 한계로 내가 원하는 기능의 구현이 어려워 포기한 상태였고, Flash가 2020년에는 완전히 사라지니 Web 버전의 대중화를 고려해본다면 괜찮은 선택이라 생각했다.

문제는, 도대체 어떻게 자바스크립트 코드 내에 코드를 삽입하는가? 포인터가 있는 것도 아닌데?

그렇게 찾은 것이 [http://esprima.org/](http://esprima.org/) 이다. Javascript Parser 인데, 매우 강력하다! 문제는 파싱만 해준다.. object로 뱉고 나머지는 알아서 조립하세요. 이러고 있다.

![image](https://user-images.githubusercontent.com/8151366/42277744-5a14644a-7fd3-11e8-9580-b0a370b56de6.png)

코드를 어떻게 찾느냐, 하면 Esprima 가 파싱한 코드의 object 결과로 찾는다. 1개 이상일 수도 있고, 1개일 수도 있다. 원하는 만큼 검색 조건을 늘릴 수 있다. (Key-value 가 동일한 것에 대해 true를 던지고, undefined는 무시한다)

rangeloc.. 대충 하느라 이름도 대충 만들었다. 삽입할 위치를 지정하는 것인데, 위 사진에는 range가 안나왔지만 위 뜻은 'value' 안에 'body' 안에 range object(array) 가 있다. 라는 뜻이다.

inject 는 뭐.. 대충 봐도 알 수 있을 것이다.

수시로 변하는 코드때문에 이런 기능을 구현했지만, 더 만들기 귀찮다.

그리고 코드를 찾고 수정하는 기능을 개선해야 한다. 아직 버그는 없지만, 삽입이 제대로 이루어지지 않는 버그가 분명 발생할 것이다. 장담한다. Parser는 매우 똑똑하게 분류하지만, 사실 코드를 함수 비스무리한 모든 것에 삽입해야 하는 입장에서는 이 함수가 함수 선언인지, 함수 표현 (Expressions) 인지 구분해야 할 이유가 없다. 그리고 위 message를 보면 알겠지만, 쟨 함수도 아니다 (..)

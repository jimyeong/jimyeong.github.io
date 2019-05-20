---
layout: post
title: "splice 와 slice 정리"
date: 2019-03-28
categories: jekyll
tag: jimyeong
---

## slice와 splice의 차이

slice 와 splice 정리

slice는 원본 배열을 바꾸지 않고 , 새로운 배열을 만든다.

```
// arg1 시작 인덱스 number, arg2 원소 개수
const animals = ["rabbit","lion","tiger","bear","fox"]
const edited = animals.slice(0,2)
// edited=["rabbit", "lion"] , animals = ["rabbit","lion","tiger","bear","fox"]
```

라는 결과를 얻을 수 있다. ()

그러나

splice는 원본 배열도 바꾸고, 새로운 배열도 만든다.
인자값으로 3개가 올 수 있다.
arg1 시작 index number를 지정할 값이다
arg2 는 배열 원소중 제거할 개수를 지정 할 값이다.
arg3 은 추가할 원소에 대한 값 이다.

```
const edited = animals.splice(1,1,"blue");
// edited = ["lion"], animals = ["rabbit","blue", tiger","bear","fox",]
```

이렇게 변화하게 된다.

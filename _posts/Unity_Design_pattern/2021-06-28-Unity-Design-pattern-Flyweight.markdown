---
layout: post
title: Unity Flyweight pattern
date: 2021-06-28 15:05:55 +0900
categories: Unity DesignPattern
---

# Unity Flyweight 패턴

## Unity에 쓰이는 Flyweight 패턴
Unity는 이미 Flyweight 패턴을 지원하고 있다. 예를 들면 Prefab를 들 수 있겠다. 한 화면의 무수히 많은 큐브를 만든다고 예를 들어보자 큐브를 다음과 같이 2가지 방법으로 큐브를 생성 해봤다
1. 직접 메쉬를 만들어서 큐브를 만든다.

2. 큐브를 Prefab으로 만들고 이를 이용한다.

먼저 첫번째 방법을 이용했을때 메모리 사용량을 확인해봤다.
![Ex1](https://user-images.githubusercontent.com/39051679/123587227-f079cf80-d820-11eb-83ae-8ce4c7f4c55b.jpg)

다음은 두번째 방법을 이용했을때 메모리 사용량을 확인해봤다.
![Ex2](https://user-images.githubusercontent.com/39051679/123587283-07b8bd00-d821-11eb-9e03-b8454205a707.jpg)

두 방법 모두 100 * 100개의 큐브를 생성했을때 유니티에서 제공하는 프로파일러로 메모리 사용량을 확인했을때 결과다. 이와같이 양이 많으으면 많을수록 확연한 결과 차이를 보인다. Unity는 또 다른 방법으로 Flyweight 패턴을 지원하는데 이는 "Scriptable Object"이다

![Ex3](https://user-images.githubusercontent.com/39051679/123585799-bf000480-d81e-11eb-949c-a88acf2e2f1a.jpg)

위와 같이 간단한 Scriptable object를 만든다.

![Ex4](https://user-images.githubusercontent.com/39051679/123585880-e3f47780-d81e-11eb-9a1d-71678b33389b.jpg)

만든 Scriptable Object를 이용하여 각각 다른 객체의 정보를 표현할 수 만큼 만들고 안에 데이터를 설정한다.

![Ex5](https://user-images.githubusercontent.com/39051679/123585618-7c3e2c80-d81e-11eb-8c7b-e46e008b6863.gif)

플레이어와 충돌할때 해당 객체가 Scriptable object를 가지고 있다면 그에 알맞게 UI가 업데이트가 되는것을 볼 수가 있다. 예제는 적은 객체를 이용했지만 만약 규모가 커지고 겹치는 요소가 많아진다면 Prefab과 Scriptable object를 이용하는 것이 많은 메모리를 아낄 수 있을 것이라 생각한다.
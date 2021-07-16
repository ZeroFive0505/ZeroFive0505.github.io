---
layout: post
title: Unity Singleton pattern
date: 2021-07-01 13:46:26 +0900
categories: Unity DesignPattern
---

# Unity Singleton 패턴
## 하나의 유일한 객체를 이용해 공유자원을 편리하게 관리하기

게임내에서 공유되는 자원을 관리할때 편하게 쓰이는 패턴이다. 예제에서는 공유되는 지점과 자원이 있다. 이러한 요소에 접근할때 싱글톤을 쓰면 간편하다.

```c#
    public static GameEnvironment Singleton
    {
        get
        {
            // 현재 인스턴스가 존재하지 않는다면 하나를 새로 만든다. 앞으로 계속 이 객체가 쓰일 것이다.
            if(instance == null)
            {
                instance = new GameEnvironment();
                instance.WayPoints.AddRange(GameObject.FindGameObjectsWithTag("WayPoint"));
            }

            return instance;
        }
    }
```

이제 이렇게 만들어진 싱글톤을 쓰고 싶은 곳에서 간단하게 `GameEviornment.Singleton`으로 접근하면 된다.

# 결과
![Image](https://user-images.githubusercontent.com/39051679/124065836-80628800-da72-11eb-8080-343bafe3d7c5.gif)

가장 가까운 공유자원으로 이동하는 모습을 볼 수 있다.
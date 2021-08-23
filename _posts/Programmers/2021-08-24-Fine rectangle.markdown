---
layout: post
title:  멀쩡한 사각형
date:  2021-08-24 07:45:28 +0900
categories: Programmers-Level2
---

# 멀쩡한 사각형
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/62048)

# 풀이
[이 곳](https://taesan94.tistory.com/55)에서 자세한 풀이를 참조할 수 있다. 먼저 못쓰는 사각형의 영역의 갯수를 세보면 두수의 최대 공약수만큼이 있다는 것을 알 수 있다. 또한 못쓰는 사각형의 가로 세로의 길이는 원래 사각형의 가로 세로 길이에 최대 공약수만큼 나눈 길이이다. 못쓰는 사각형의 대각선을 따로 보면 대각선은 그 사각형의 가로, 세로 길이만큼 움직여야하며 시작점은 같기에 넓이 + 높이 - 1을 한다.

# 코드
```c++
#include <iostream>

using namespace std;

long long GCD(int w, int h)
{
    while (w != 0 && h != 0)
    {
        if (w > h)
            w -= h;
        else
            h -= w;
    }

    if (w == 0)
        return h;
    else
        return w;
}

long long solution(int w, int h) 
{
    long long answer = 0;
    long long gcd = GCD(w, h);
    long long total = (long long) w * h;

    answer = total - ((long long)w / gcd + (long long)h / gcd - 1) * gcd;

    return answer;
}
```
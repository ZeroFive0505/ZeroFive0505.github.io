---
layout: post
title: 더 맵게
date:  2021-08-24 08:01:35 +0900
categories: Programmers-Level2
---

# 더 맵게
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42626)

# 풀이
최소힙의 특성으로 풀 수 있는 문제. 먼저 힙의 사이즈가 2 이상일때만 반복문을 돌며 두개의 값을 꺼내고 문제에 나온대로 식에 넣어서 그 값을 반환한다. 반환한 값을 그대로 힙에 넣고 만약 힙의 탑이 K 이상이라면 반복문을 멈추고 나간다. 반복문이 끝났을때 다시 한번 힙의 탑이 K 이상인지를 확인하고 만약 아니라면 -1을 반환한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <queue>
#include <vector>

using namespace std;

int Formular(int theLeast, int theSecond)
{
    return theLeast + (theSecond * 2);
}

int solution(vector<int> scoville, int K) 
{
    int answer = 0;
    priority_queue<int, vector<int>, greater<int>> pq;

    for (auto i : scoville)
        pq.push(i);

    int cnt = 1;

    while (pq.size() >= 2)
    {
        int first = pq.top();
        pq.pop();
        int second = pq.top();
        pq.pop();

        int mix = Formular(first, second);

        pq.push(mix);

        if (pq.top() >= K)
            break;

        cnt++;
    }

    if (pq.top() >= K)
        answer = cnt;
    else
        answer = -1;

    return answer;
}
```
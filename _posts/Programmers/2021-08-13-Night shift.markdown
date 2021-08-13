---
layout: post
title: 야근 지수
date:  2021-08-13 09:56:50 +0900
categories: Programmers-Level3
---

# 야근 지수
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12927)

# 풀이
n 시간동안 작업을 최소하 한다. 최대 힙을 이용해서 가장 오랜 시간이 걸리는 작업을 최상위로 놓고 그 작업을 빼고 시간을 -1을 해주고 다시 힙에 넣는다. 그리고 마지막에 힙이 빌때까지 제곱을 한 결과를 정답으로 반환한다.

# 코드
```c++
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

long long solution(int n, vector<int> works) 
{
    long long answer = 0;

    priority_queue<int> pq(works.begin(), works.end());

    for (int i = 0; i < n; i++)
    {
        if (pq.top() > 0)
        {
            int t = pq.top();
            t--;
            pq.pop();
            pq.push(t);
        }
    }

    while (!pq.empty())
    {
        answer += (long long)pq.top() * (long long)pq.top();
        pq.pop();
    }

    return answer;
}
```
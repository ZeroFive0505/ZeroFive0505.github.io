---
layout: post
title:  기능 개발
date:  2021-08-24 07:54:04 +0900
categories: Programmers-Level2
---

# 기능 개발
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42586)

# 풀이
큐를 이용해서 풀 수 있다. 큐에다 모든 작업과 해당 작업의 속도를 넣어놓고 가장 날이 하루하루 지날때마다 작업량과 속도가 100 이상이라면 큐에서 빼고 카운트를 늘려주고 정답에 넣는다.

# 코드
```c++
#include <iostream>
#include <string>
#include <queue>
#include <vector>

using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) 
{
    vector<int> answer;
    queue<pair<int, int>> q;
    int size = progresses.size();
    int day = 1;

    for (int i = 0; i < size; i++)
    {
        q.push({ progresses[i], speeds[i] });
    }


    while (!q.empty())
    {
        int cnt = 0;
        while (!q.empty() && q.front().first + q.front().second * day >= 100)
        {
            q.pop();
            cnt++;
        }

        if (cnt != 0)
            answer.push_back(cnt);

        day++;
    }

    return answer;
}
```
---
layout: post
title: 단속 카메라
date: 2021-08-09 09:17:58 +0900
categories: Programmers-Level3
---

# 단속 카메라
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42884)

# 풀이
먼저 시작점과 끝점을 짝으로해서 벡터에 넣는다. 그리고 만약 끝나는 지점이 같다면 시작점 기준으로 아니라면 끝점을 기준으로 정렬을 한다. 그리고 처음 기준이 되는 포인트를 잡고 벡터의 1부터 시작해서 끝까지 순회한다. 만약 기준이 되는 포인트의 끝나는 지점보다 지금 순회하고있는 포인트의 시작지점이 더 크다면 이 경우는 카메라를 더 설치해야하는 경우다. 따라서 정답을 늘리고 포인트를 이동한다. 또 다른 경우는 지금 포인트의 끝나는 지점이 현재 순회하고 있는 포인트의 끝나는 지점보다 크거나 같다면 이 경우 현재 범위가 지금 순회 포인트를 포함하고 있다는 의미가 되므로 간단히 범위를 새롭게 갱신한다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> routes) 
{
    int answer = 0;
    vector<pair<int, int>> v;

    for (int i = 0; i < routes.size(); i++)
    {
        int start = routes[i][0];
        int end = routes[i][1];

        v.push_back({ start, end });
    }

    sort(v.begin(), v.end(), [](const pair<int, int>& a, const pair<int, int>& b) {
        if (a.second == b.second)
            return a.first < b.first;
        else
            return a.second < b.second;
    });


    // 기준이 되는 포인트
    pair<int, int> p = v[0];
    // 감시 카메라는 최소 하나는 설치해야 함
    answer = 1;
    for (int i = 1; i < v.size(); i++)
    {
        // 만약 현재 범위의 끝나는 지점보다 더 크다면
        if (p.second < v[i].first)
        {
            // 카메라를 하나 더 설치하고
            answer++;
            // 범위 갱신
            p = v[i];
        }

        // 만약 지금 범위가 포함을 하고있다면 그냥 범위를 갱신
        if (p.second >= v[i].second)
            p = v[i];
    }

    return answer;
}
```
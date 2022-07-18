---
layout: post
title: 금과 은 운반하기
date: 2021-11-08 08:38:09 +0900
categories: Programmers-Level3
---

# 금과 은 운반하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/86053?language=cpp)

# 풀이
이분탐색을 이용해서 풀 수 있는 문제였다. 이분탐색을 이용해야 하는 문제인가? 라는 생각까지는 했지만 확실한 생각이 안 들어서 인터넷을 참조해본 결과 이분탐색을 이용해서 푸는 문제가 맞았었다.

# 코드
```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <limits.h>
#include <vector>

using namespace std;

const long long INF = LONG_MAX;

long long solution(int a, int b, vector<int> g, vector<int> s, vector<int> w, vector<int> t) 
{
    long long answer = INF;

    long long start = 0;
    // 최악의 시간 : 금과 은의 최대 무게는 10^9
    // 도시의 갯수 : 10^5
    // 왕복 2
    // 자원의 갯수 2
    long long end = 10e5 * 4 * 10e9;

    size_t size = g.size();

    while (start <= end)
    {
        long long mid = (start + end) / 2;

        // 금 운송량
        long long golds = 0;
        // 은 운송량
        long long silvers = 0;
        // 금과 은을 합쳐서 운송했을때
        long long sum = 0;


        for (size_t i = 0; i < size; i++)
        {
            long long currentGold = (long long)g[i];
            long long currentSilver = (long long)s[i];
            long long currentWeight = (long long)w[i];
            long long currentTime = (long long)t[i];

            // 현재 mid 시간에서 편도 시간 * 2를 나눈 몫을 횟수로 지정
            long long moveCnt = mid / (currentTime * 2);

            // 만약 나머지가 현재 편도 시간 이상만큼 있다면 한번 더 움직일 수 있다.
            if (mid % (currentTime * 2) >= t[i])
                moveCnt++;

            // 만약 현재 가지고 있는 금이 mid시간 만큼 운송했을때의 양보다 작다면
            // 가지고 있는 모든 금을 옮긴 것
            if (currentGold < moveCnt * currentWeight)
                golds += currentGold;
            // 아니라면 운송량과 횟수 만큼 금을 늘려준다.
            else
                golds += moveCnt * currentWeight;

            // 은도 마찬가지
            if (currentSilver < moveCnt * currentWeight)
                silvers += currentSilver;
            else
                silvers += moveCnt * currentWeight;

            // 금과 은을 섞어서 운반할 경우
            if (currentGold + currentSilver < moveCnt * currentWeight)
                sum += currentGold + currentSilver;
            else
                sum += moveCnt * currentWeight;
        }
        

        // 만약 모든 조건을 만족했을 경우는
        if (golds >= a && silvers >= b && sum >= a + b)
        {
            // 최대 한도선을 내린다.
            end = mid - 1;
            answer = min(mid, answer);
        }
        // 만약 불가능했을 경우 최저 한도선을 늘린다.
        else
            start = mid + 1;
    }

    return answer;
}
```
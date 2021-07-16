---
layout: post
title: 합승 택시 요금
date: 2021-07-09 10:04:42 +0900
categories: Programmers-Level3
---

# 합승 택시 요금
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/72413)

# 풀이
처음에는 다익스트라로 풀어보려 했지만.. 플로이드로 풀 게 되면 훨씬 간단하다. 모든 정점까지의 최단 거리를 구하고 초기값으로는 s -> a, s -> b의 값으로 각각 따로 갈때를 계산해두고 반복문으로 s -> i지점으로 가는 길이 있고 i -> a와 i -> b로 가는 길이 존재한다면 최솟값을 업데이트한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

const int INF = 987654321;

int solution(int n, int s, int a, int b, vector<vector<int>> fares) 
{
    int answer = 0;

    vector<vector<int>> dists(n + 1, vector<int>(n + 1, INF));

    for (int i = 0; i < fares.size(); i++)
    {
        int u = fares[i][0];
        int v = fares[i][1];
        int w = fares[i][2];

        dists[u][v] = w;
        dists[v][u] = w;
    }

    for (int i = 0; i <= n; i++)
        dists[i][i] = 0;

    for (int k = 1; k <= n; k++)
    {
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j <= n; j++)
            {
                if (dists[i][k] + dists[k][j] < dists[i][j])
                    dists[i][j] = dists[i][k] + dists[k][j];
            }
        }
    }


    answer = dists[s][a] + dists[s][b];

    for (int i = 1; i <= n; i++)
    {
        if (dists[s][i] != INF && dists[i][a] != INF && dists[i][b] != INF)
            answer = min(answer, dists[s][i] + dists[i][a] + dists[i][b]);
    }

    return answer;
}
```
---
layout: post
title: 가장 먼 노드
date: 2021-07-16 09:25:35 +0900
categories: Programmers-Level3
---

# 가장 먼 노드
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/49189)

# 풀이
다익스트라를 이용해서 1번 노드부터 모든 정점까지의 거리를 구한다. 거리를 구한 후, 최대 거리를 찾고나서 그 거리의 노드가 몇개 있는지를 센다.

# 코드
```c++
#include <iostream>
#include <queue>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

const int INF = 987654321;

int Dijkstra(const int n, int start, vector<vector<int>>& graph)
{
    vector<int> dists(n + 1, INF);
    dists[start] = 0;

    priority_queue<pair<int, int>> pq;

    pq.push({ 0, start });

    while (!pq.empty())
    {
        pair<int, int> current = pq.top();
        pq.pop();

        int u = current.second;
        int currentDist = -current.first;

        for (int v = 0; v < graph[u].size(); v++)
        {
            if (dists[graph[u][v]] > dists[u] + 1)
            {
                dists[graph[u][v]] = dists[u] + 1;
                pq.push({ -dists[graph[u][v]], graph[u][v] });
            }
        }
    }

    int maxDist = *max_element(dists.begin() + 1, dists.end());


    int cnt = 0;
    for (int i = 1; i < dists.size(); i++)
    {
        if (maxDist == dists[i])
            cnt++;
    }


    return cnt;
}

int solution(int n, vector<vector<int>> edge) 
{
    int answer = 0;

    vector<vector<int>> graph(n + 1);

    for (int i = 0; i < edge.size(); i++)
    {
        int u, v;
        u = edge[i][0];
        v = edge[i][1];

        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    answer = Dijkstra(n, 1, graph);

    return answer;
}
```
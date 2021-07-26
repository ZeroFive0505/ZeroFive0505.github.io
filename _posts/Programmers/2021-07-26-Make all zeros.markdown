---
layout: post
title: 모두 0으로 만들기
date: 2021-07-26 09:31:48 +0900
categories: Programmers-Level3
---

# 모두 0으로 만들기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/76503)

# 풀이
DFS를 이용해서 풀 수 있는 문제였다. 0번 정점을 시작으로 DFS를 시행한뒤에 부모에 자식노드의 합을 더한뒤에 정답에는 그것에 절댓값을 누적한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

void DFS(vector<vector<int>>& graph, vector<long long>& sum, long long& answer, int parrent, int current)
{
    for (int i = 0; i < graph[current].size(); i++)
    {
        if (graph[current][i] != parrent)
            DFS(graph, sum, answer, current, graph[current][i]);
    }

    sum[parrent] += sum[current];
    answer += abs(sum[current]);
}

long long solution(vector<int> a, vector<vector<int>> edges) 
{
    long long answer = 0;
    vector<long long> sum(a.begin(), a.end());

    vector<vector<int>> graph(a.size());

    for (int i = 0; i < edges.size(); i++)
    {
        int u = edges[i][0];
        int v = edges[i][1];

        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    DFS(graph, sum, answer, 0, 0);

    if (sum[0] != 0)
        answer = -1;

    return answer;
}
```
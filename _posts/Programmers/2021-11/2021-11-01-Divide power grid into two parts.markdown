---
layout: post
title: 전력망을 둘로 나누기
date: 2021-11-01 07:25:48 +0900
categories: Programmers-Leve2
---

# 전력망을 둘로 나누기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/86971?language=cpp)

# 풀이
n의 범위가 2 ~ 100이라서 이중 반복문으로 모든 경우의 수를 확인하면서 최소가되는 값을 시간안에 구할 수 있다. 또한 전력망의 형태는 항상 트리 형태라서 간선을 끊는 경우는 연결 요소가 무조건 늘어나게 되어있다. 이후는 DFS 또는 BFS를 돌면서 한 연결요소안에 포함된 노드의 수를 확인하고 그 차의 최소를 구하면된다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

const int INF = 987654321;

int cnt = 0;

void DFS(vector<vector<int>>& graph, vector<bool>& visited, int u)
{
    visited[u] = true;
    cnt++;
    for (int v = 0; v < graph[u].size(); v++)
    {
        if (!visited[graph[u][v]])
        {
            DFS(graph, visited, graph[u][v]);
        }
    }
}

int solution(int n, vector<vector<int>> wires) 
{
    int answer = INF;

    
    for (int i = 0; i < wires.size(); i++)
    {
        vector<vector<int>> graph(n + 1);
        vector<bool> visited(n + 1, false);
        for (int j = 0; j < wires.size(); j++)
        {
            // 만약 이번에 끊길 전력망이면
            if (i == j)
            {
                // 자기자신과 연결한다.
                graph[wires[j][0]].push_back(wires[j][0]);
                graph[wires[j][1]].push_back(wires[j][1]);
            }
            else
            {
                graph[wires[j][0]].push_back(wires[j][1]);
                graph[wires[j][1]].push_back(wires[j][0]);
            }
        }

        vector<int> cands;

        for (int j = 1; j <= n; j++)
        {
            for (int k = 0; k < graph[j].size(); k++)
            {
                if (!visited[graph[j][k]])
                {
                    cnt = 0;
                    DFS(graph, visited, graph[j][k]);
                    cands.push_back(cnt);
                }
            }
        }

        answer = min(abs(cands[0] - cands[1]), answer);
    }


    return answer;
}
```

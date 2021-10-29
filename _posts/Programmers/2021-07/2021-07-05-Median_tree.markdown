---
layout: post
title: 트리 트리오 중간 값
date: 2021-07-05 10:52:15 +0900
categories: Programmers-Level4
---

# 트리 트리오 중간 값
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/68937)

# 첫 시도
플로이드 알고리즘으로 모든 정점에서 모든 정점까지 거리를 구한 후에 최대 힙에다 모든 거리를 넣고 거기서 3개만 빼고 최대 값을 구했는데... 역시 n의 범위가 최대 250,000까지 가니까 테스트 7까지는 정답이었지만 그 이후부터는 전부 시간 초과가 떴다..

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <queue>
#include <string>
#include <vector>

using namespace std;
const int INF = 987654321;
int solution(int n, vector<vector<int>> edges) 
{
    int answer = 0;
    vector<vector<int>> graph(n + 1, vector<int>(n + 1, INF));

    for (int i = 0; i < edges.size(); i++)
    {
        int u = edges[i][0];
        int v = edges[i][1];
        int c = 1;
        graph[u][v] = c;
        graph[v][u] = c;
    }

    for (int i = 1; i <= n; i++)
        graph[i][i] = 0;

    for (int k = 1; k <= n; k++)
    {
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j <= n; j++)
            {
                if (graph[i][k] + graph[k][j] < graph[i][j])
                    graph[i][j] = graph[i][k] + graph[k][j];
            }
        }
    }

    answer = -INF;
    for (int i = 1; i <= n; i++)
    {
        int sum = 0;
        priority_queue<int> pq;
        for (int j = 1; j <= n; j++)
        {
            if (graph[i][j] == 0)
                continue;
            pq.push(graph[i][j]);
        }

        for (int j = 0; j < 3; j++)
        {
            sum += pq.top();
            pq.pop();
        }

        answer = max(answer, sum / 3);
    }

    return answer;
}
```

# 풀이
트리의 지름을 구해서 푸는 방법을 인터넷에서 찾을 수 있었다.. 자세한 풀이는 여기서 확인 할 수 있다. 
### [링크](https://yabmoons.tistory.com/597)

```C++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> Graph;
vector<bool> Visited;
int Leaf;
int Max_Len;
int Cnt;

void DFS(int cur, int len, bool terminate)
{
    if (Visited[cur])
        return;

    if (!terminate)
    {
        if (len > Max_Len)
        {
            Leaf = cur;
            Max_Len = len;
        }
    }
    else
    {
        if (len > Max_Len)
        {
            Leaf = cur;
            Cnt = 1;
            Max_Len = len;
        }
        else if (len == Max_Len)
            Cnt++;
    }

    Visited[cur] = true;

    for (int i = 0; i < Graph[cur].size(); i++)
    {
        int next = Graph[cur][i];
        DFS(next, len + 1, terminate);
    }
}

void Search(int start, int n, bool terminate)
{
    Cnt = 0;
    Max_Len = 0;
    Visited.assign(n + 1, false);

    DFS(start, 0, terminate);
}

int solution(int n, vector<vector<int>> edges) 
{
    Graph.resize(n + 1);
    Visited.resize(n + 1, false);

    for (int i = 0; i < edges.size(); i++)
    {
        int u = edges[i][0];
        int v = edges[i][1];
        Graph[u].push_back(v);
        Graph[v].push_back(u);
    }

    Search(1, n, false);
    Search(Leaf, n, true);

    if (Cnt >= 2)
        return Max_Len;
    else
    {
        Search(Leaf, n, true);
        if (Cnt >= 2)
            return Max_Len;
        else
            return Max_Len - 1;
    }
}
```
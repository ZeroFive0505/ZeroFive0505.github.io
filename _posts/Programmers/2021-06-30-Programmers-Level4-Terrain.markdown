---
layout: post
title: 지형 이동
date: 2021-06-30 08:14:13 +0900
categories: Programmers-Level4
---

# 지형 이동
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/62050)

# 풀이
일단 BFS로 영역을 나눌 수는 있겠다고 까지는 생각했지만 사다리가 여러개 놓일경우 어떻게 최소 높이 차의 사다리를 찾아낼 수 있을까를 너무 어렵게 생각했는데 간단하게 최소 힙을 써서 해결할 수 있었다. 영역을 나눴다면 이제 영역별로 간선이 여러 있다고 생각하고 MST 알고리즘을 이용하여 해결할 수 있었다.

# 코드
```C++
#include <iostream>
#include <queue>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

const int SIZE = 301;
int visited[SIZE][SIZE];
int parent[SIZE * SIZE];
int numSize[SIZE * SIZE];


struct sDelta
{
    int x;
    int y;
}Delta[] = {
    {-1, 0},
    {1, 0},
    {0, -1},
    {0, 1}
};

struct sNode
{
    int u;
    int v;
    int weight;

    sNode(int u, int v, int weight) : u(u), v(v), weight(weight) {}

    bool operator<(const sNode& rhs) const
    {
        return this->weight < rhs.weight;
    }

    bool operator>(const sNode& rhs) const
    {
        return this->weight > rhs.weight;
    }
};

void BFS(int y, int x, vector<vector<int>>& land, int height, int areaNum)
{
    queue<pair<int, int>> q;

    visited[y][x] = areaNum;

    q.push({ y, x });

    while (!q.empty())
    {
        pair<int, int> current = q.front();
        q.pop();

        int y = current.first;
        int x = current.second;

        for (int i = 0; i < 4; i++)
        {
            int ny = y + Delta[i].y;
            int nx = x + Delta[i].x;

            if (ny < 0 || nx < 0 || ny >= land.size() || nx >= land[0].size())
                continue;

            if (visited[ny][nx] == 0 && abs(land[y][x] - land[ny][nx]) <= height)
            {
                visited[ny][nx] = areaNum;
                q.push({ ny, nx });
            }
        }
    }
}

int GetParent(int v)
{
    int i;
    for (i = v; parent[i] != -1; i = parent[i]);
    return i;
}

void SetUnion(int u, int v)
{
    if (numSize[u] < numSize[v])
    {
        numSize[v] += numSize[u];
        parent[u] = v;
    }
    else
    {
        numSize[u] += numSize[v];
        parent[v] = u;
    }
}

int solution(vector<vector<int>> land, int height) 
{
    int answer = 0;

    int areaNum = 1;

    fill(parent, parent + (SIZE * SIZE), -1);
    fill(numSize, numSize + (SIZE * SIZE), 0);


    for (int i = 0; i < land.size(); i++)
    {
        for (int j = 0; j < land[0].size(); j++)
        {
            if (visited[i][j] == 0)
            {
                BFS(i, j, land, height, areaNum);
                areaNum++;
            }
        }
    }


    priority_queue<sNode, vector<sNode>, greater<sNode>> pq;


    for (int y = 0; y < land.size(); y++)
    {
        for (int x = 0; x < land[0].size(); x++)
        {
            for (int dir = 0; dir < 4; dir++)
            {
                int ny = y + Delta[dir].y;
                int nx = x + Delta[dir].x;

                if (ny < 0 || nx < 0 || ny >= land.size() || nx >= land[0].size())
                    continue;

                if (visited[y][x] != visited[ny][nx])
                {
                    pq.push(sNode(visited[y][x], visited[ny][nx], abs(land[y][x] - land[ny][nx])));
                }
            }
        }
    }


    areaNum--;

    int edgeAccepted = 0;

    while (edgeAccepted < areaNum - 1)
    {
        sNode top = pq.top();
        pq.pop();

        int u = top.u;
        int v = top.v;
        int w = top.weight;

        int uParent = GetParent(u);
        int vParent = GetParent(v);

        if (uParent != vParent)
        {
            answer += w;
            SetUnion(uParent, vParent);
            edgeAccepted++;
        }
    }
    

    return answer;
}
```
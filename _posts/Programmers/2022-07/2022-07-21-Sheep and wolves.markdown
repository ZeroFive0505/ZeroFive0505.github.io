---
layout: post
title: 양과 늑대
date: 2022-07-21 09:13:24 +0900
categories: Programmers-Level3
---

# 양과 늑대
## 문제 링크 -> [Programmers](https://school.programmers.co.kr/learn/courses/30/lessons/92343)

# 문제 풀이
[이 곳](https://yabmoons.tistory.com/727)의 문제 풀이를 참조하여 풀 수 있었다. 3차원 배열을 통해 양 몇마리 늑대 몇마리일 경우에 방문 여부를 체크하는 방식이 인상 깊었다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> nodes[20];
vector<int> g_info;
int MAX_SHEEP = 0;
bool visited[20][20][20];

void DFS(int node, int s, int w)
{
    // 최대치 갱신
    if (node == 0)
        MAX_SHEEP = max(s, MAX_SHEEP);

    for (size_t i = 0; i < nodes[node].size(); i++)
    {
        int next = nodes[node][i];

        // 만약 다음 노드에 양이 위치했다면
        if (g_info[next] == 0)
        {
            // 만약 양 한마리가 추가됬을때 방문한적이 없다면
            if (!visited[next][s + 1][w])
            {
                // 방문
                visited[next][s + 1][w] = true;
                // 빈 노드로 갱신한다.
                g_info[next] = -1;

                // 백트랙킹 시작
                DFS(next, s + 1, w);

                // 원상 복구
                g_info[next] = 0;
                visited[next][s + 1][w] = false;
            }
        }
        // 만약 늑대가 위치한 노드라면
        else if (g_info[next] == 1)
        {   
            // 만약 늑대가 한마리 추가됬을때 방문한적이 없으며
            // 양의 수가 늑대가 한마리 추가됬을때보다 많다면 방문이 가능하다.
            if (!visited[next][s][w + 1] && s > w + 1)
            {
                // 위와 마찬가지로 백트랙킹
                visited[next][s][w + 1] = true;
                g_info[next] = -1;

                DFS(next, s, w + 1);

                g_info[next] = 1;
                visited[next][s][w + 1] = false;
            }
        }
        // 빈 노드라면
        else
        {
            // 방문여부를 확인하고 백트랙킹
            if (!visited[next][s][w])
            {
                visited[next][s][w] = true;

                DFS(next, s, w);

                visited[next][s][w] = false;
            }
        }
    }
}

int solution(vector<int> info, vector<vector<int>> edges) 
{
    int answer = 0;

    for (size_t i = 0; i < edges.size(); i++)
    {
        int from = edges[i][0];
        int to = edges[i][1];

        nodes[from].push_back(to);
        nodes[to].push_back(from);
    }

    g_info = info;

    // 첫노드는 무조건 방문
    g_info[0] = -1;

    // 첫 노드는 양 한마리로 방문
    visited[0][1][0] = true;

    DFS(0, 1, 0);

    answer = MAX_SHEEP;

    return answer;
}
```
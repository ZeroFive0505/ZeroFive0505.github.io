---
layout: post
title: 매출 하락 최소화
date: 2021-07-05 09:48:55 +0900
categories: Programmers-Level4
---

# 매출 하락 최소화
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/72416)

# 풀이
어떻게 풀어야 할지 전혀 감이 안 와서 결국 다른 사람들의 풀이를 확인했다.
[풀이](https://yabmoons.tistory.com/625) DP를 이용한 방식으로 매출 최소합을 구해야하며, 말단 노드일 경우 간단하게 참가하는 경우와 불참하는 경우를 기록하고 말단 노드가 자식 노드들을 확인하면서 최소가되는 경우를 갱신한다..

# 코드
```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

const int MAX = 300001;


int cache[MAX][2]; // 0: 워크숍 참석 X, 1: 워크숍 참석
vector<int> Node[MAX];
vector<int> Cost;

void DFS(int cur)
{
    bool leaf_node = true;

    for (int i = 0; i < Node[cur].size(); i++)
    {
        int next = Node[cur][i];
        // 만약 자식이 하나라도 있으면 리프노드가 아니다.
        leaf_node = false;
        // 재귀 탐색
        DFS(next);
    }

    if (leaf_node)
    {
        // 리프노드일경우 참석을 안 했을때는 0, 참석했을시에는 그 비용만큼이 된다.
        cache[cur][0] = 0;
        cache[cur][1] = Cost[cur - 1];
        return;
    }

    // 리프노드가 아닐경우 즉, 팀장과 팀원의 역활을 동시에 하는 노드.
    int sum = 0;
    int min_cost = 987654321;
    bool attend = true;

    for (int i = 0; i < Node[cur].size(); i++)
    {
        int child = Node[cur][i];
        sum += min(cache[child][0], cache[child][1]);
        if (cache[child][0] >= cache[child][1])
            attend = false;
        min_cost = min(min_cost, cache[child][1] - cache[child][0]);
    }

    cache[cur][1] = Cost[cur - 1] + sum;
    if (attend == false)
        cache[cur][0] = sum;
    else
        cache[cur][0] = sum + min_cost;
}

int solution(vector<int> sales, vector<vector<int>> links) 
{
    int answer = 0;

    for (int i = 0; i < links.size(); i++)
    {
        int parent = links[i][0];
        int child = links[i][1];
        Node[parent].push_back(child);
    }

    Cost = sales;
    DFS(1);

    answer = min(cache[1][0], cache[1][1]);


    return answer;
}
```
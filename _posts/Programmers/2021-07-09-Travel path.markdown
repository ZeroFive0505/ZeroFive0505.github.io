---
layout: post
title: 여행경로
date: 2021-07-09 10:21:56 +0900
categories: Programmers-Level3
---

# 여행 경로
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/43164)

# 풀이
DFS 백트랙킹 문제였다. 모든 경우를 확인하고 가능한 경우를 반환하면 된다.

# 코드
```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool DFS(vector<vector<string>>& tickets, vector<bool>& visited, string current, int usedTickets, vector<string>& answer)
{
    // 먼저 현재 위치를 넣는다.
    answer.push_back(current);

    // 만약 티켓을 전부 썼다면 가능한 경로이다.
    if (tickets.size() == usedTickets)
        return true;


    for (int i = 0; i < tickets.size(); i++)
    {
        string from = tickets[i][0];
        string to = tickets[i][1];

        // 현재위치가 같으면서 아직 방문하지 않은 곳이 있다면.
        if (from == current && !visited[i])
        {
            // 방문한다.
            visited[i] = true;
            // 만약 성공적으로 순회가 가능했다면 참을 반환
            if(DFS(tickets, visited, to, usedTickets + 1, answer))
                return true;
            // 방문이 실패했으므로 다시 원상태로 되돌린다.
            visited[i] = false;
        }
    }

    // 반복문을 빠져나왔다면 현재 위치는 정답이 될 수 없다 따라서 뺀다.
    answer.pop_back();

    // 거짓을 반환
    return false;
}

vector<string> solution(vector<vector<string>> tickets) 
{
    vector<string> answer;
    // 티켓의 수만큼 만든다.
    vector<bool> visited(tickets.size(), false);
    // ABC 순서로 출력해야 하기에 정렬한다.
    sort(tickets.begin(), tickets.end());
    // 인천공항부터 시작한다.
    DFS(tickets, visited, "ICN", 0, answer);
    return answer;
}
```
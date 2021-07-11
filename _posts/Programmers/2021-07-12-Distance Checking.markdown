---
layout: post
title: 거리두기 확인하기
date: 2021-07-12 07:53:55 +0900
categories: Programmers-Level2
---

# 거리 두기 확인하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/81302)

# 풀이
맨하튼 거리를 어떻게 체크할지 좀 고민했던 문제였다.. 고민하다 간단하게 BFS를 상하좌우로 한번하고 다시한번 BFS를 하면서 만약 거리두기 조건에 만족하지 않는다면 바로 거짓을 반환하게 했다.

# 코드
```C++
#include <iostream>
#include <queue>
#include <string>
#include <vector>

using namespace std;

enum class Position
{
    EMPTY,
    PARTITION
};

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

struct sSit
{
    int x;
    int y;
    Position pos;

    sSit(int x, int y, Position pos) : x(x), y(y), pos(pos) {}
};

bool BFS(vector<string>& v, int y, int x)
{
    queue<sSit> q;
    for (int i = 0; i < 4; i++)
    {
        int ny = y + Delta[i].y;
        int nx = x + Delta[i].x;

        if (nx < 0 || ny < 0 || nx >= 5 || ny >= 5)
            continue;

        if (v[ny][nx] == 'P')
            return false;

        if (v[ny][nx] == 'O')
        {
            q.push(sSit(nx, ny, Position::EMPTY));
        }
        else if (v[ny][nx] == 'X')
        {
            q.push(sSit(nx, ny, Position::PARTITION));
        }
    }


    while (!q.empty())
    {
        sSit cur = q.front();
        q.pop();

        int cx = cur.x;
        int cy = cur.y;

        for (int i = 0; i < 4; i++)
        {
            int nx = cx + Delta[i].x;
            int ny = cy + Delta[i].y;

            if (nx == x && ny == y)
                continue;

            if (nx < 0 || ny < 0 || nx >= 5 || ny >= 5)
                continue;

            if (v[ny][nx] == 'P' && cur.pos == Position::EMPTY)
                return false;
        }
    }

    return true;
}

vector<int> solution(vector<vector<string>> places) 
{
    vector<int> answer;

    for (int i = 0; i < places.size(); i++)
    {
        vector<string> v = places[i];
        bool check = true;
        for (int j = 0; j < v.size(); j++)
        {
            for (int k = 0; k < v[j].size(); k++)
            {
                if (v[j][k] == 'P')
                {
                    check = BFS(v, j, k);
                }

                if (!check)
                    break;
            }

            if (!check)
                break;
        }

        if (check)
            answer.push_back(1);
        else
            answer.push_back(0);
    }

    return answer;
}
```
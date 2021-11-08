---
layout: post
title: 아이템 줍기
date: 2021-11-03 10:57:56 +0900
categories: Programmers-Leve3
---

# 아이템 줍기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/87694)

# 풀이
첫시도는 주어진 좌표값을 그대로 이용해서 외곽선만 남겨보려 했는데 잘 되지 않았다. 질문 글들을 참조해본 결과 좌표를 2배로 늘려서하면 외곽선을 간단하게 구할 수 있다는 것을 알게되고 그대로 구현을 한 이후 BFS를 이용해 최단경로를 구해준다. 단 정답을 반환할때는 2로 나눠줘야한다 2배로 모든 좌표를 늘렸으니

# 코드
```c++
#include <iostream>
#include <queue>
#include <string>
#include <vector>

using namespace std;

const int MAX = 103;

int arr[MAX][MAX] = {};
bool visited[MAX][MAX] = {};

struct sPos
{
    int x;
    int y;
    int cnt;

    sPos(int _x, int _y, int _cnt) : x(_x), y(_y), cnt(_cnt)
    {

    }
};

struct sDelta
{
    int x;
    int y;
}Delta[] = {
    {1, 0},
    {0, 1},
    {-1, 0},
    {0, -1}
};

const int INF = 987654321;


int solution(vector<vector<int>> rectangle, int characterX, int characterY, int itemX, int itemY) 
{
    int answer = INF;

    int startX = characterX * 2;
    int startY = characterY * 2;
    int endX = itemX * 2;
    int endY = itemY * 2;

    for (int i = 0; i < rectangle.size(); i++)
    {
        int ltX = rectangle[i][0];
        int ltY = rectangle[i][1];
        int btX = rectangle[i][2];
        int btY = rectangle[i][3];

        for (int y = ltY * 2; y <= btY * 2; y++)
        {
            for (int x = ltX * 2; x <= btX * 2; x++)
            {
                arr[y][x] = 1;
            }
        }
    }

    for (int i = 0; i < rectangle.size(); i++)
    {
        int ltX = rectangle[i][0];
        int ltY = rectangle[i][1];
        int btX = rectangle[i][2];
        int btY = rectangle[i][3];

        for (int y = ltY * 2 + 1; y < btY * 2; y++)
        {
            for (int x = ltX * 2 + 1; x < btX * 2; x++)
            {
                arr[y][x] = 0;
            }
        }
    }

    queue<sPos> q;

    q.push(sPos(startX, startY, 0));


    while (!q.empty())
    {
        sPos cur = q.front();
        q.pop();

        visited[cur.y][cur.x] = true;

        if (cur.x == endX && cur.y == endY)
            answer = min(answer, cur.cnt);

        for (int i = 0; i < 4; i++)
        {
            sPos temp = cur;

            temp.x += Delta[i].x;
            temp.y += Delta[i].y;

            if (temp.x < 0 || temp.x >= MAX || temp.y < 0 || temp.y >= MAX)
                continue;

            if (arr[temp.y][temp.x] == 0)
                continue;

            if (visited[temp.y][temp.x])
                continue;

            q.push(sPos(temp.x, temp.y, cur.cnt + 1));
        }
    }


    return answer / 2;
}
```
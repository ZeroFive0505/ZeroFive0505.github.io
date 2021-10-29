---
layout: post
title: 경주로 건설
date: 2021-07-26 08:49:16 +0900
categories: Programmers-Level3
---

# 보행자 천국
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/67259)

# 풀이
BFS를 응용해서 4방향으로 BFS를 하는 문제였다. 여기서 주의할 점은 왔던 곳으로 되돌아가지 않도록 하는 것인데 위, 오른쪽, 아래, 왼쪽 순서대로 0, 1, 2, 3의 값을 부여하면 그 차이가 절댓값으로 2가 되면 바로 그 전 방향으로 돌아가게 되는 것이다. 그리고 전역 3차원 배열에는 그 방향에서의 최소값을 기록해나가고 최종적으로 배열에서 최솟값을 구하면 된다. 여기서 또 주의할 점은 일반 큐가 아닌 우선순위 큐를 이용해야 하는데 최소 비용의 거리를 구해야하기에 이 비용을 기준으로 우선순위 큐를 이용해야 최소 비용을 구할 수 있다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>
#include <queue>

using namespace std;

const int SIZE = 26;

const int DIR = 4;

int DIST[SIZE][SIZE][DIR];

struct sDelta
{
    int x;
    int y;
} Delta[] = {
    {-1, 0}, {0, -1}, {1, 0}, {0, 1}
};

const int INF = 987654321;

struct sPoint
{
    int x;
    int y;
    int cost;
    int prevDir;

    sPoint(int x, int y, int cost, int prevDir)
    {
        this->x = x;
        this->y = y;
        this->cost = cost;
        this->prevDir = prevDir;
    }
};

struct sPointCmp
{
    bool operator()(const sPoint& a, const sPoint& b)
    {
        return a.cost > b.cost;
    }
};


int solution(vector<vector<int>> board) 
{
    int answer = INF;

    for (int i = 0; i < SIZE; i++)
    {
        for (int j = 0; j < SIZE; j++)
        {
            for (int k = 0; k < 4; k++)
            {
                DIST[i][j][k] = INF;
            }
        }
    }

    priority_queue<sPoint, vector<sPoint>, sPointCmp> pq;


    for (int i = 0; i < 4; i++)
    {
        DIST[0][0][i] = 0;
        pq.push(sPoint(0, 0, 0, i));
    }


    while (!pq.empty())
    {
        sPoint current = pq.top();
        pq.pop();

        int cx = current.x;
        int cy = current.y;
        int curCost = current.cost;
        int curDir = current.prevDir;

        for (int i = 0; i < DIR; i++)
        {
            int nx = cx + Delta[i].x;
            int ny = cy + Delta[i].y;
            int newDir = i;

            if (nx < 0 || ny < 0 || nx >= board[0].size() || ny >= board.size())
                continue;

            if (board[ny][nx] == 1)
                continue;

            if (abs(curDir - newDir) == 2)
                continue;

            int tempCost = 0;

            if (curDir == newDir)
                tempCost = 100;
            else if (curDir != newDir)
                tempCost = 600;


            if (DIST[ny][nx][newDir] > curCost + tempCost)
            {
                DIST[ny][nx][newDir] = curCost + tempCost;
                pq.push(sPoint(nx, ny, curCost + tempCost, newDir));
            }
        }
    }

    int row = board.size() - 1;
    int col = board[0].size() - 1;

    for (int i = 0; i < DIR; i++)
    {
        answer = min(answer, DIST[row][col][i]);
    }

    return answer;
}
```
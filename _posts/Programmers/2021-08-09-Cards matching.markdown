---
layout: post
title: 카드 짝 맞추기
date: 2021-08-09 10:16:17 +0900
categories: Programmers-Level3
---

# 카드 짝 맞추기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/72415)

# 풀이
[이 동영상](https://www.youtube.com/watch?v=4bwz9yOUGWM)을 참고하여 풀 수 있었던 문제였다. 
BFS + 백트랙킹 문제로 먼저 시작점에서 시작을 하여 카드의 짝을 찾고 그 카드를 어떤 순서로 짝을 찾느냐에 따라 이동 횟수가 달라 질 수 있는데 그 부분을 주의해야 했으며 또한 BFS시에는 간단하게 상하좌우로 한칸만 움직이는게 아니라 카드를 만날 때까지 쭉 일직선으로 이동이 가능하기에 이 경우도 확인을 해야한다. 또한 카드 짝을 맞추는 순서뿐만 아니라 몇번 카드부터 맞춰야 최소가 되는지를 확인해야하기에 백트랙킹으로 이 경우의 수를 전부 확인해야 하는 문제였다.

# 코드
```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>

using namespace std;

vector<vector<int>> BOARD;

const int INF = 987654321;

struct sPoint
{
    int row;
    int col;
    int cnt;

    bool operator==(const sPoint& rhs) const
    {
        return row == rhs.row && col == rhs.col;
    }
};

struct sDelta
{
    int row;
    int col;
}Delta[] = {
    {-1, 0},
    {1, 0},
    {0, -1},
    {0, 1}
};

int BFS(const sPoint& start, const sPoint& end)
{
    vector<vector<bool>> visited(BOARD.size(), vector<bool>(BOARD[0].size(), false));
    visited[start.row][start.col] = true;

    queue<sPoint> q;

    q.push(start);

    while (!q.empty())
    {
        sPoint current = q.front();
        q.pop();

        if (current == end)
            return current.cnt;

        for (int i = 0; i < 4; i++)
        {
            int nr = current.row + Delta[i].row;
            int nc = current.col + Delta[i].col;

            if (nr < 0 || nc < 0 || nr >= BOARD.size() || nc >= BOARD[0].size())
                continue;

            if (!visited[nr][nc])
            {
                visited[nr][nc] = true;
                q.push({ nr, nc, current.cnt + 1 });
            }

            // 일직선으로 쭉 움직이는 경우의 수
            for (int j = 0; ; j++)
            {   
                // 만약 0이아닌 수 즉, 카드를 만났을 경우 멈춘다.
                if (BOARD[nr][nc] != 0)
                    break;

                // 경계선 확인
                if (nr + Delta[i].row < 0 || nc + Delta[i].col < 0 || nr + Delta[i].row >= BOARD.size() ||
                    nc + Delta[i].col >= BOARD[0].size())
                    break;

                nr += Delta[i].row;
                nc += Delta[i].col;
            }

            if (!visited[nr][nc])
            {
                visited[nr][nc] = true;
                q.push({ nr, nc, current.cnt + 1 });
            }
        }
    }

    return INF;
}

int Permute(sPoint current)
{
    int ret = INF;

    for (int cardNum = 1; cardNum < 7; cardNum++)
    {
        vector<sPoint> cardPairs;

        for (int i = 0; i < BOARD.size(); i++)
        {
            for (int j = 0; j < BOARD[0].size(); j++)
            {
                if (cardNum == BOARD[i][j])
                    cardPairs.push_back({ i, j, 0 });
            }
        }

        // 만약 카드가 없다면 건너뛴다.
        if (cardPairs.empty())
            continue;

        // 현재 지점에서 0번째 카드와 1번째 카드 순으로 짝 맞췄을 경우와
        int first = BFS(current, cardPairs[0]) + BFS(cardPairs[0], cardPairs[1]) + 2;
        // 현재 지점에서 1번째 카드와 0번째 카드 순으로 짝 맞췄을 경우 
        int second = BFS(current, cardPairs[1]) + BFS(cardPairs[1], cardPairs[0]) + 2;

        // 먼저 카드 짝을 맞췄으니 지운다.
        for (int i = 0; i < cardPairs.size(); i++)
            BOARD[cardPairs[i].row][cardPairs[i].col] = 0;

        // 최솟값을 찾는다.
        // 첫번째, 0번째와 1번째 카드 순으로 맞췄을 경우 1번째 카드의 위치부터 시작.
        ret = min(ret, first + Permute(cardPairs[1]));
        // 두번째, 1번째와 0번째 카드 순으로 맞췄을 경우 0번째 카드의 위치부터 시작.
        ret = min(ret, second + Permute(cardPairs[0]));

        // 카드를 지웠으면 다시 원위치를 시킨다.
        for (int i = 0; i < cardPairs.size(); i++)
            BOARD[cardPairs[i].row][cardPairs[i].col] = cardNum;
    }

    if (ret == INF)
        return 0;
    return ret;
}

int solution(vector<vector<int>> board, int r, int c)
{
    BOARD = board;
    return Permute({ r, c, 0 });
}
```

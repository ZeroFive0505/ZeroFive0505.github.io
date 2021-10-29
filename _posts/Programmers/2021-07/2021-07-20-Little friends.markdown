---
layout: post
title: 리틀 프렌즈 사천성
date: 2021-07-20 11:18:18 +0900
categories: Programmers-Level3
---

# 리틀 프렌즈 사천성
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/1836)

# 풀이
BFS의 응용문제였다.. 처음에는 각 자리에서 4방향으로 뻗어나가고 한 번 양 방향으로 꺾으면서 같은 블록이 있는지 확인하는 방법으로 풀어봤는데 코드도 지저분해지고 알파벳 순서대로 출력하는게 잘 안되서 결국 다른 사람들의 풀이를 봐서 풀었다..
먼저 `map`에 블록의 위치들을 기록한다. 그리고 그 `map`을 순회하면서 짝을 찾을 수 있는지 찾는다 못 찾을 경우 `IMPOSSIBLE`을 반환한다.

BFS에서는 꺾은횟수를 기록하는 벡터를 만든다. 처음에는 방향이 정해지지 않았으니 -1로 초기화하고 큐에 넣고 빼고 4방향으로 확인을 한다. 현재 꺾은 횟수는 만약 방향이 -1이 아니고 현재 방향과 앞으로의 방향이 다르다면 횟수를 늘려주고 만약 횟수가 2 이상이라면 더 이상 못꺾는 경우 따라서 건너뛴다. 큐에 넣을때는 꺾은 횟수를 기록해준다.

# 코드
```c++
#include <iostream>
#include <string>
#include <map>
#include <queue>
#include <ctype.h>
#include <vector>

using namespace std;

struct sDelta
{
    int r;
    int c;
} Delta[] = { {-1, 0}, {0, 1}, {1, 0}, {0, -1} };

struct sPos
{
    int r;
    int c;
    int dir;
};

int M, N;

const int INF = 987654321;

vector<string> B;

map<char, sPos> hashMap;

sPos BFS(sPos src, char ch)
{
    vector<vector<int>> turns(M, vector<int>(N, INF));

    turns[src.r][src.c] = 0;

    src.dir = -1;

    queue<sPos> q;

    q.push(src);

    bool not_first = false;

    while (!q.empty())
    {
        sPos cur = q.front();
        q.pop();

        if (not_first && B[cur.r][cur.c] == ch)
            return cur;

        not_first = true;

        for (int i = 0; i < 4; i++)
        {
            int nr = cur.r + Delta[i].r;
            int nc = cur.c + Delta[i].c;

            int nextDir = i;

            int nextTurn = turns[cur.r][cur.c];

            if (cur.dir != -1 && cur.dir != nextDir)
                nextTurn++;

            if (nr < 0 || nr >= M || nc < 0 || nc >= N)
                continue;

            if (nextTurn >= 2)
                continue;

            if (B[nr][nc] != '.' && B[nr][nc] != ch)
                continue;

            if (turns[nr][nc] >= nextTurn)
            {
                q.push({ nr, nc, nextDir });
                turns[nr][nc] = nextTurn;
            }
        }
    }
    

    return { -1, -1 };
}

string solution(int m, int n, vector<string> board)
{
    string answer;

    M = m;
    N = n;
    B = board;


    for (int i = 0; i < board.size(); i++)
    {
        for (int j = 0; j < board[i].size(); j++)
        {
            if (isalpha(B[i][j]))
            {
                hashMap[B[i][j]] = { i, j };
            }
        }
    }


    while (1)
    {
        bool canRemove = false;
        for (const auto& iter : hashMap)
        {
            char ch = iter.first;
            sPos src = iter.second;
            sPos dest = BFS(src, ch);

            if (dest.r != -1 && dest.c != -1)
            {
                B[src.r][src.c] = '.';
                B[dest.r][dest.c] = '.';
                hashMap.erase(iter.first);
                answer += ch;
                canRemove = true;
                break;
            }
        }

        if (canRemove)
            continue;

        if (hashMap.empty())
            return answer;
        else
            return "IMPOSSIBLE";
    }
}
```
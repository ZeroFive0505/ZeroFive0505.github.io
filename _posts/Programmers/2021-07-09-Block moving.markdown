---
layout: post
title: 블록 이동하기
date: 2021-07-09 10:56:01 +0900
categories: Programmers-Level3
---

# 블록 이동하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/60063)

# 풀이
개인적으로 많이 어려웠던 문제였다.. 인터넷에 다른 사람들의 풀이를 보고 겨우 풀 수 있었다.. 기존 BFS에 블록이 2칸을 차지하며 회전까지도 가능한 문제였다.

# 코드
```c++
#include <iostream>
#include <string>
#include <queue>
#include <map>
#include <tuple>
#include <vector>

using namespace std;

vector<vector<int>> Board;

int N;

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

struct sRobot
{
    // 2 블록의 위치 정보
    int x1;
    int y1;
    int x2;
    int y2;

    sRobot(int x1, int y1, int x2, int y2) : x1(x1), y1(y1), x2(x2), y2(y2)
    {

    }

    // 이동이 가능한지 검사
    bool IsValid() const
    {
        if (x1 >= 0 && x1 < N && y1 >= 0 && y1 < N && x2 >= 0 && x2 < N && y2 >= 0 && y2 < N && !Board[y1][x1] && !Board[y2][x2])
            return true;
        else
            return false;
    }

    // 끝났는지 검사
    bool IsFinished() const
    {
        if (x1 == N - 1 && y1 == N - 1 || x2 == N - 1 && y2 == N - 1)
            return true;
        else
            return false;
    }

    // d 방향으로 움직인 로봇을 반환한다.
    sRobot Move(int d)
    {
        return sRobot(x1 + Delta[d].x, y1 + Delta[d].y, x2 + Delta[d].x, y2 + Delta[d].y);
    }

    // d 방향으로 움직일 수 있다면 회전은 총 2가지 경우가 있다.
    pair<sRobot, sRobot> Rotate(int d)
    {
        return make_pair(sRobot(x1, y1, x1 + Delta[d].x, y1 + Delta[d].y), sRobot(x2, y2, x2 + Delta[d].x, y2 + Delta[d].y));
    }

    // map을 이용하기에 필요한 연산자 오버로딩.
    bool operator==(const sRobot& rhs) const
    {
        return tie(x1, y1, x2, y2) == tie(rhs.x1, rhs.y1, rhs.x2, rhs.y2) ||
            tie(x1, y1, x2, y2) == tie(rhs.x2, rhs.y2, rhs.x1, rhs.y1);
    }

    // 비교 연산자.
    bool operator<(const sRobot& rhs) const
    {
        return tie(x1, y1, x2, y2) < tie(rhs.x1, rhs.y1, rhs.x2, rhs.y2);
    }
};

// 맵을 선언한다. hashMap[sRobot] = 거리
map<sRobot, int> hashMap;

// 4방향으로 움직임이 가능한 로봇들을 반환한다.
vector<sRobot> GetMoved(sRobot& current)
{
    vector<sRobot> ret;

    for (int i = 0; i < 4; i++)
    {
        if (current.Move(i).IsValid())
            ret.push_back(current.Move(i));
    }

    return ret;
}

// 4방향으로 움직임이 가능할때 회전 가능한 경우의 수를 반환한다.
vector<sRobot> GetRotated(sRobot& current)
{
    vector<sRobot> ret;

    for (int i = 0; i < 4; i++)
    {
        if (current.Move(i).IsValid())
        {
            pair<sRobot, sRobot> temp = current.Rotate(i);
            ret.push_back(temp.first);
            ret.push_back(temp.second);
        }
    }

    return ret;
}

int BFS(sRobot robot)
{
    // 초기 로봇의 움직인 거리는 0
    hashMap[robot] = 0;
    queue<sRobot> q;

    q.push(robot);

    while (!q.empty())
    {
        sRobot cur = q.front();
        q.pop();

        int curDist = hashMap[cur];
        // 만약 끝났다면 현재 거리를 반환.
        if (cur.IsFinished())
            return curDist;

        // 회전과 이동 총 2가지 경우의 수를 가져온다.
        vector<sRobot> next[2] = { GetMoved(cur), GetRotated(cur) };

        for (int i = 0; i < 2; i++)
        {
            for (sRobot& r: next[i])
            {
                // 만약 map에 존재 하지않고 가능한 경우의 수라면
                if (hashMap.find(r) == hashMap.end() && r.IsValid())
                {
                    // 움직인 거리를 하나 늘려주고 큐에 넣는다.
                    hashMap[r] = curDist + 1;
                    q.push(r);
                }
            }
        }
    }

    return -1;
}


int solution(vector<vector<int>> board) 
{
    int answer = 0;
    N = board.size();
    Board = board;
    answer = BFS(sRobot(0, 0, 1, 0));
    return answer;
}
```
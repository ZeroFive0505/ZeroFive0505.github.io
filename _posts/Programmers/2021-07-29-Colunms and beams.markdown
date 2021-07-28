---
layout: post
title: 기둥과 보
date: 2021-07-29 08:34:19 +0900
categories: Programmers-Level3
---

# 기둥과 보
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/60061)

# 풀이
인터넷에 다른 [사람](https://jinhyukoo.github.io/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4/2020/07/10/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4_%EA%B8%B0%EB%91%A5%EA%B3%BC%EB%B3%B4%EC%84%A4%EC%B9%98.html)의 풀이를 참고하여 푼 문제다. 기둥과 보의 설치와 제거 조건을 정확하게 설정해놓고 설치시에는 이 조건이 만족하면 설치하고 제거시에는 현재 기둥 또는 보의 설치조건을 만족하기 위한 조건들이 제거가 가능하다면 제거를하고 아니라면 다시 원상태로 되돌린다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

const int SIZE = 101;

const int COLUMN = 0;
const int BEAM = 1;

int BUILD[SIZE][SIZE][2];

// 경계 조건
bool CheckBoundary(const int x, const int y, const int n)
{
    if (y >= 0 && x >= 0 && x <= n && y <= n)
        return true;
    else
        return false;
}

// 보의 설치 조건
bool BeamCheck(const int x, const int y, const int n)
{
    // 양 끝에 다른 보가 있을때
    if (CheckBoundary(x - 1, y, n) && BUILD[y][x - 1][BEAM] && 
        CheckBoundary(x + 1, y, n) && BUILD[y][x + 1][BEAM])
        return true;

    // 또는 아래에 기둥이 있을때
    if (CheckBoundary(y - 1, x, n) && BUILD[y - 1][x][COLUMN])
        return true;

    // 아니면 기둥에 걸쳐있을때
    if (CheckBoundary(y - 1, x + 1, n) && BUILD[y - 1][x + 1][COLUMN])
        return true;

    return false;
}

bool ColCheck(const int x, const int y, const int n)
{
    // 맨 바닥에 설치
    if (y == 0)
        return true;

    // 아래에 기둥이 있을때
    if (CheckBoundary(x, y - 1, n) && BUILD[y - 1][x][COLUMN])
        return true;

    // 보에 걸쳐있을때
    if (CheckBoundary(x - 1, y, n) && BUILD[y][x - 1][BEAM])
        return true;

    // 보 위에 있을때
    if (CheckBoundary(x, y, n) && BUILD[y][x][BEAM])
        return true;

    return false;
}

vector<vector<int>> solution(int n, vector<vector<int>> build_frame)
{
    vector<vector<int>> answer;

    for (int i = 0; i < build_frame.size(); i++)
    {
        int x = build_frame[i][0];
        int y = build_frame[i][1];
        int a = build_frame[i][2];
        int b = build_frame[i][3];

        if (a == 0) // Column
        {
            if (b == 1)
            {
                // 기둥의 설치조건을 만족한다면 설치
                if (ColCheck(x, y, n))
                    BUILD[y][x][COLUMN] = 1;
            }
            else
            {
                // 일단 제거를 한다.
                BUILD[y][x][COLUMN] = 0;

                // 삭제된 기둥위에 기둥이 있다면
                if (BUILD[y + 1][x][COLUMN] == 1)
                {
                    // 위에있는 기둥이 다른 설치조건을 만족하는지 확인한다.
                    // 만약 만족한다면 현재 기둥은 지워도되고 아니라면 유지한다.
                    if (!ColCheck(x, y + 1, n))
                    {
                        BUILD[y][x][COLUMN] = 1;
                        continue;
                    }
                }

                // 만약 삭제할려는 기둥위에 보가 있다면
                if (BUILD[y + 1][x][BEAM] == 1)
                {
                    // 보가 이 기둥말고 다른 설치조건을 만족하는지 확인
                    if (!BeamCheck(x, y + 1, n))
                    {
                        BUILD[y][x][COLUMN] = 1;
                        continue;
                    }
                }

                // 만약 위에 걸쳐있는 보가 있다면.
                if (BUILD[y + 1][x - 1][BEAM] == 1)
                {   
                    // 보가 이 기둥말고 다른 설치조건을 만족하는지 확인
                    if (!BeamCheck(x - 1, y + 1, n))
                    {
                        BUILD[y][x][COLUMN] = 1;
                        continue;
                    }
                }
            }
        }
        else // Beam
        {
            if (b == 1)
            {
                // 보의 설치조건을 확인하고 만족한다면 설치
                if (BeamCheck(x, y, n))
                    BUILD[y][x][BEAM] = 1;
            }
            else
            {
                BUILD[y][x][BEAM] = 0;

                // 왼쪽에 보가 있다면
                if (BUILD[y][x - 1][BEAM] == 1)
                {
                    // 이 보가 다른 설치조건을 만족하는지 확인
                    if (!BeamCheck(x - 1, y, n))
                    {
                        BUILD[y][x][BEAM] = 1;
                        continue;
                    }
                }

                // 오른쪽에 보가 있다면
                if (BUILD[y][x + 1][BEAM] == 1)
                {
                    // 이 보가 다른 설치조건을 만족하는지 확인
                    if (!BeamCheck(x + 1, y, n))
                    {
                        BUILD[y][x][BEAM] = 1;
                        continue;
                    }
                }

                // 기둥이 있다면
                if (BUILD[y][x][COLUMN] == 1)
                {
                    // 기둥이 다른 설치조건을 만족하는지 확인한다.
                    if (!ColCheck(x, y, n))
                    {
                        BUILD[y][x][BEAM] = 1;
                        continue;
                    }
                }

                // 걸쳐있는 기둥이 있다면
                if (BUILD[y][x + 1][COLUMN] == 1)
                {
                    // 기둥이 다른 설치조건을 만족하는지 확인한다.
                    if (!ColCheck(x + 1, y, n))
                    {
                        BUILD[y][x][BEAM] = 1;
                        continue;
                    }
                }
            }
        }
    }

    // y좌표를 우선으로 넣는다.
    for (int i = 0; i <= n; i++)
    {
        for (int j = 0; j <= n; j++)
        {
            if (BUILD[j][i][COLUMN])
                answer.push_back({ i, j, COLUMN });

            if (BUILD[j][i][BEAM])
                answer.push_back({ i, j, BEAM });
        }
    }


    return answer;
}

```

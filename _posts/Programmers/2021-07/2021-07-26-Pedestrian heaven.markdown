---
layout: post
title: 보행자 천국
date: 2021-07-26 08:09:14 +0900
categories: Programmers-Level3
---

# 보행자 천국
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/1832)

# 풀이
등굣길과 유사하면서도 더 어려운 문제였다. 다른 사람들의 풀이를 참고해서 풀 수 있었는데 아래와 오른쪽 경우의 수를 따로 기록해가면서 풀 수 있었다. 일단 `RIGHT[1][1]`, `DOWN[1][1]`은 1로 초기화를 해둔다. 출발점과 도착점의 값은 `0`이라 명시되어 있기에 출발점에서 아래로 또는 오른쪽으로 가는 경우의 수는 하나 있을 것이다. 그 이후에는 1부터 m, n까지 순회하면서 바로 전에 왔던 곳이 0일 경우 경우의 수를 누적해주고 1일 경우 0으로 초기화 그리고 2일 경우는 바로 그 전 경로의 경우의 수를 가져온다.

# 코드
```c++
#include <iostream>
#include <cstring>
#include <vector>

using namespace std;

int MOD = 20170805;

const int SIZE = 501;

int RIGHT[SIZE][SIZE];
int DOWN[SIZE][SIZE];

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int solution(int m, int n, vector<vector<int>> city_map) 
{
    int answer = 0;
    memset(RIGHT, 0, sizeof(RIGHT));
    memset(DOWN, 0, sizeof(DOWN));

    RIGHT[1][1] = 1;
    DOWN[1][1] = 1;

    for (int i = 1; i <= m; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            if (city_map[i - 1][j - 1] == 0)
            {
                RIGHT[i][j] = (RIGHT[i][j] + RIGHT[i][j - 1] + DOWN[i - 1][j]) % MOD;
                DOWN[i][j] = (DOWN[i][j] + DOWN[i - 1][j] + RIGHT[i][j - 1]) % MOD;
            }
            else if (city_map[i - 1][j - 1] == 1)
            {
                RIGHT[i][j] = 0;
                DOWN[i][j] = 0;
            }
            else if (city_map[i - 1][j - 1] == 2)
            {
                RIGHT[i][j] = RIGHT[i][j - 1];
                DOWN[i][j] = DOWN[i - 1][j];
            }
        }
    }


    answer = (RIGHT[m][n - 1] + DOWN[m - 1][n]) % MOD;

    return answer;
}
```

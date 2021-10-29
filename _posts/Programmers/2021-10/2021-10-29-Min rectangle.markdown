---
layout: post
title: 최소 직사각형
date: 2021-10-29 11:33:54 +0900
categories: Programmers-Level1
---

# 최소 직사각형
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/86491)

# 풀이
처음에는 가로 세로를 바꿔가면서 최대 값을 찾는 방법으로 해봤는데 이렇게 하는 것이 아니라 각 최댓값의 집합과 최솟값의 집합을 만들어 거기서 최대값끼리 짝을 지어서 최소 직사각형을 만들어야 하는 문제였다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> sizes) 
{
    int answer = 0;

    vector<int> rows;
    vector<int> cols;

    for (int i = 0; i < sizes.size(); i++)
    {
        if (sizes[i][0] > sizes[i][1])
        {
            rows.push_back(sizes[i][0]);
            cols.push_back(sizes[i][1]);
        }
        else
        {
            rows.push_back(sizes[i][1]);
            cols.push_back(sizes[i][0]);
        }
    }

    int rowMax = -1;
    int colMax = -1;

    for (int i = 0; i < sizes.size(); i++)
    {
        rowMax = max(rowMax, rows[i]);
        colMax = max(colMax, cols[i]);
    }

    answer = rowMax * colMax;

    return answer;
}
```
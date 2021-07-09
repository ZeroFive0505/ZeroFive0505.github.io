---
layout: post
title: 최적의 행렬 곱셈
date: 2021-07-09 08:53:11 +0900
categories: Programmers-Level4
---


# 최적의 행렬 곱셈
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12942)

# 풀이
left 부터 right까지 곱셈의 최적의 해는 left 부터 k까지 더하기 k + 1부터 right까지의 합과 left, k + 1, right까지의 곱을 합한 값중에서 최소 값을 취하면 된다.


# 코드
```C++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

const int SIZE = 210;

int cache[SIZE][SIZE];

const int INF = 987654321;

int solution(vector<vector<int>> matrix_sizes)
{
    int answer = 0;

    for (int i = 0; i < SIZE; i++)
    {
        for (int j = 0; j < SIZE; j++)
        {
            if (i == j)
                cache[i][j] = 0;
            else
                cache[i][j] = INF;
        }
    }

    for (int i = 0; i < matrix_sizes.size(); i++)
    {
        for (int left = 0; left < matrix_sizes.size(); left++)
        {
            int right = left + i;

            if (right >= matrix_sizes.size())
                break;

            for (int k = left; k < right; k++)
            {
                cache[left][right] = min(cache[left][right], 
                    cache[left][k] + cache[k + 1][right] + matrix_sizes[left][0] * matrix_sizes[k + 1][0] * matrix_sizes[right][1]);
            }
        }
    }

    answer = cache[0][matrix_sizes.size() - 1];
    return answer;
}
```
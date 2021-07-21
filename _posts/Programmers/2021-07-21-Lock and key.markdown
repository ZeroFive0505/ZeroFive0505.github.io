---
layout: post
title: 자물쇠와 열쇠
date: 2021-07-21 09:18:43 +0900
categories: Programmers-Level3
---

# 자물쇠와 열쇠
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/60059)

# 풀이
먼저 정방행렬의 90도 회전을 구현해야 하며 코드는 다음과 같다.

```c++
void RotateMat(vector<vector<int>>& key)
{
    int m = key.size();

    vector<vector<int>> temp(m, vector<int>(m, 0));

    for (int i = 0; i < key.size(); i++)
    {
        for (int j = 0; j < key[i].size(); j++)
        {
            temp[i][j] = key[m - j - 1][i];
        }
    }

    key = temp;
}
```
회전 후의 행렬의 열은 회전 전 행렬의 행이고 회전 후의 행렬의 행은 `행렬의 크기 - 열 번호 - 1`로 나타낼 수 있다.

이렇게 회전을 최대 4번 반복하면서 자물쇠 행렬을 순회하는데 그 전에 패딩을 추가해야 한다. 자물쇠 행렬의 네개의 꼭짓점을 기준으로 열쇠의 크기만큼 추가한다. 그 이후에는 패딩이 추가된 자물쇠에 원래 자물쇠의 값을 넣고 그리고 열쇠 행렬과 XOR연산으로 토글 후에 정확한 자물쇠 시작 위치부터 카운트를 세고 만약 전부다 1이 되었다면 참을 반환한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

void RotateMat(vector<vector<int>>& key)
{
    int m = key.size();

    vector<vector<int>> temp(m, vector<int>(m, 0));

    for (int i = 0; i < key.size(); i++)
    {
        for (int j = 0; j < key[i].size(); j++)
        {
            temp[i][j] = key[m - j - 1][i];
        }
    }

    key = temp;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock) 
{
    int keySize = key.size();
    int lockSize = lock.size();

    for (int rot = 0; rot < 4; rot++)
    {
        RotateMat(key);

        for (int i = 0; i < lockSize + keySize - 1; i++)
        {
            for (int j = 0; j < lockSize + keySize - 1; j++)
            {
                vector<vector<int>> padding(lockSize + (keySize - 1) * 2, vector<int>(lockSize + (keySize - 1) * 2, 0));

                for (int y = 0; y < lockSize; y++)
                {
                    for (int x = 0; x < lockSize; x++)
                    {
                        padding[y + keySize - 1][x + keySize - 1] = lock[y][x];
                    }
                }

                for (int y = 0; y < keySize; y++)
                {
                    for (int x = 0; x < keySize; x++)
                    {
                        padding[i + y][j + x] ^= key[y][x];
                    }
                }

                int cnt = 0;

                for (int y = keySize - 1; y < lockSize + keySize - 1; y++)
                {
                    for (int x = keySize - 1; x < lockSize + keySize - 1; x++)
                    {
                        if (padding[y][x])
                            cnt++;
                    }
                }

                if (cnt == lockSize * lockSize)
                    return true;
            }
        }
    }
  
    return false;
}
```
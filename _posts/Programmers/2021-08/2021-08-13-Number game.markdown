---
layout: post
title: 숫자 게임
date:  2021-08-13 09:48:49 +0900
categories: Programmers-Level3
---

# 숫자 게임
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12987)

# 풀이
정렬을 이용하여 간단하게 풀 수 있는 문제였다. 먼제 A와 B를 오름차 순으로 정렬한뒤에 A와 B의 값을 비교해간다. 만약 B가 A보다 크다면 정답을 하나 늘려주고 인덱스를 현재 B의 인덱스에서 하나 더 한다.

# 코드
```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int solution(vector<int> A, vector<int> B) 
{
    int answer = 0;

    sort(A.begin(), A.end());
    sort(B.begin(), B.end());

    int index = 0;

    for (int i = 0; i < A.size(); i++)
    {
        for (int j = index; j < B.size(); j++)
        {
            if (A[i] < B[j])
            {
                answer++;
                index = j + 1;
                break;
            }
        }
    }

    return answer;
}
```
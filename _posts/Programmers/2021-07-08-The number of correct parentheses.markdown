---
layout: post
title: 올바른 괄호의 갯수
date: 2021-07-08 09:56:50 +0900
categories: Programmers-Level4
---

# 올바른 괄호의 갯수
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12929)

# 풀이
생각외로 간단한 문제였다... C++에서 제공하는 next_permutation으로 모든 경우의 수에서 괄호가 올바른지 체크를 하면 된다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;


int solution(int n) 
{
    int answer = 0;

    vector<int> v;

    for (int i = 0; i < n; i++)
    {
        // 여는 괄호는 1, 닫는 괄호는 -1
        v.push_back(1);
        v.push_back(-1);
    }

    do
    {
        int sum = 0;
        bool match = true;
        for (int i = 0; i < v.size(); i++)
        {
            sum += v[i];
            // 만약 괄호의 합이 0보다 작아지면 올바르지 못한 괄호가 된다.
            if (sum < 0)
            {
                match = false;
                break;
            }
        }

        if (match)
            answer++;

    } while (next_permutation(v.begin(), v.end()));

    return answer;
}
```
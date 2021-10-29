---
layout: post
title: 예상 대진표
date: 2021-09-04 07:37:26 +0900
categories: Programmers-Level2
---

# 예상 대진표
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12985)

# 풀이
2의 지수승이라는 것을 이용해서 간단하게 벡터로 풀었다. 먼저 반복분을 돌면서 a, b에 true를 넣고
최소 벡터의 크기가 2이상일 동안에 새로운 임시벡터에 승자를 넣는다. 만약 둘다 참일 경우는 원하는
정답이 되니 반복문을 나오고 아닐경우에는 조건에 맞게 임시벡터에 넣는다. 그리고 벡터를 스왑한다.

# 코드
```c++
#include <iostream>
#include <vector>
using namespace std;

int solution(int n, int a, int b)
{
    int answer = 0;
    vector<bool> v;

    for (int i = 1; i <= n; i++)
    {
        if (i == a || i == b)
            v.push_back(true);
        else
            v.push_back(false);
    }

    while (v.size() >= 2)
    {

        vector<bool> t;
        bool found = false;
        for (int i = 0; i < v.size(); i += 2)
        {
            // 만약 원하는 두 선수가 만났다면
            if (v[i] && v[i + 1])
            {
                found = true;
                break;
            }
            // 관심없는 두 선수가 만났다면 아무나 올린다.
            else if (v[i] == false && v[i + 1] == false)
                t.push_back(v[i]);
            // 두 선수중 한명이 관심있는 선수라면
            // 관심있는 선수를 올린다.
            else if (v[i] && v[i + 1] == false)
                t.push_back(v[i]);
            else if (v[i] == false && v[i + 1])
                t.push_back(v[i + 1]);
        }

        answer++;

        if (found)
            break;

        v = t;
    }

    return answer;
}
```
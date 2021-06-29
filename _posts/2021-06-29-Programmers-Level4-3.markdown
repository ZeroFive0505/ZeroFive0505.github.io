---
layout: post
title: 도둑질
date: 2021-06-29 10:17:15 +0900
categories: Programmers-Level4
---

# 도둑질
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42897)

# 풀이
레벨3에 있는 스티커 문제랑 완전히 똑같았다. 표현만 다를 뿐이었다.

```C++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

const int SIZE = 1000000;

int cache[SIZE];

int solution(vector<int> money) 
{
    int answer = 0;

    if (money.size() == 1)
        return money[0];

    cache[0] = money[0];
    cache[1] = cache[0];

    for (int i = 2; i < money.size() - 1; i++)
    {
        cache[i] = max(cache[i - 2] + money[i], cache[i - 1]);
    }

    answer = max(answer, cache[money.size() - 2]);

    cache[0] = 0;
    cache[1] = money[1];

    for (int i = 2; i < money.size(); i++)
    {
        cache[i] = max(cache[i - 2] + money[i], cache[i - 1]);
    }

    answer = max(answer, cache[money.size() - 1]);

    return answer;
}

```
---
layout: post
title: 숫자 블록
date: 2021-07-08 09:51:53 +0900
categories: Programmers-Level4
---

# 숫자 블록
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12923)

# 풀이
숫자의 범위도 크고 무슨 규칙성이 있는지를 찾기 힘들었다.. 결국 인터넷을 조사해보니 1과 자기 자신을 제외한 약수로 나눈 몫이 들어가는 법칙이 있었다. 만약 그 사이의 숫자로 나눠지는게 없다면 1은 반환하며 최대 범위를 잘 조절하면 풀 수 있는 문제였다.

# 코드
```C++
#include <iostream>
#include <string>
#include <cmath>
#include <vector>

using namespace std;

const int MAX = 10000000;

int Calc(int n)
{
    // 루트를 취한다.
    int num = sqrt(n);

    // 루트 값까지 순회.
    for (int i = 2; i <= num; i++)
    {
        // 만약 나누어 떨어지고 최대 값도 넘지 않는다면
        // 몫을 반환.
        if (n % i == 0 && n / i <= MAX)
            return n / i;
    }

    return 1;
}

vector<int> solution(long long begin, long long end) 
{
    vector<int> answer;

    for (long long i = begin; i <= end; i++)
        answer.push_back(Calc(i));

    if (begin == 1)
        answer[0] = 0;

    return answer;
}
```
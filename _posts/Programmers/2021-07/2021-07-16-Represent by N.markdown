---
layout: post
title: N으로 표현
date: 2021-07-16 09:49:55 +0900
categories: Programmers-Level3
---

# 표 편집
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/81303)

# 풀이
풀이는 [이곳](https://gurumee92.tistory.com/164)에서 참조를 했다... 먼저 벡터에 N이 5라면 5, 55, 555, 5555... 이런식으로 기초가 되는 값을 넣어 놓는다. 8번 반복하면서 모든 조합을 해당 벡터에 넣는다. op1 + op2, op1 - op2, op1 * op2, op1 / op2... 찾을 수 있다면 그 해당 인덱스에 1을 더해서 반환한다.

# 코드
```c++
#include <iostream>
#include <set>
#include <string>
#include <vector>
#include <cmath>

using namespace std;

int GetN_Number(int n, int power)
{
    int ret = 0;
    while (power)
    {
        power--;
        ret = ret + n * pow(10, power);
    }

    return ret;
}

int solution(int N, int number) 
{
    if (N == number)
        return 1;

    int answer = -1;

    vector<set<int>> v(8);
    
    int cnt = 1;
    for (auto& i : v)
    {
        i.insert(GetN_Number(N, cnt++));
    }

    for (int i = 1; i < v.size(); i++)
    {
        for (int j = 0; j < i; j++)
        {
            for (const auto& op1 : v[j])
            {
                for (const auto& op2 : v[i - j - 1])
                {
                    v[i].insert(op1 + op2);
                    v[i].insert(op1 - op2);
                    v[i].insert(op1 * op2);

                    if (op2 != 0)
                        v[i].insert(op1 / op2);
                }
            }
        }

        if (v[i].find(number) != v[i].end())
        {
            answer = i + 1;
            break;
        }
    }

    return answer;
}
```
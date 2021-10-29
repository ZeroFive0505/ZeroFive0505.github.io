---
layout: post
title: 소수 찾기
date: 2021-08-31 08:12:48 +0900
categories: Programmers-Level2
---

# 소수 찾기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42839)

# 풀이
next_permutation을 이용하여 모든 조합의 수를 만들고 set을 이용하여 중복되는 원소를 무시했다.

# 코드
```c++
#include <iostream>
#include <string>
#include <set>
#include <algorithm>
#include <vector>
#include <cmath>

using namespace std;

bool IsPrime(int num)
{
    if (num < 2)
        return false;

    if (num == 2)
        return true;

    int n = sqrt(num);
    
    for (int i = 2; i <= n; i++)
    {
        if (num % i == 0)
            return false;
    }

    return true;
}

int solution(string numbers) 
{
    int answer = 0;
    vector<int> perm;

    for (int i = 0; i < numbers.size(); i++)
        perm.push_back(i);

    vector<string> v;

    do
    {
        string t;
        for (int i = 0; i < numbers.size(); i++)
        {
            t += numbers[perm[i]];
            // 여기서 추가해준다. 만약 모든수를 다 써서 소수를 만들어야 한다면 이 반복문 밖에서
            // 추가해줘야한다.
            v.push_back(t);
        }


    } while (next_permutation(perm.begin(), perm.end()));

    set<int> s;

    for (int i = 0; i < v.size(); i++)
    {
        int n = stoi(v[i]);
        if (IsPrime(stoi(v[i])) && s.count(n) == 0)
        {
            answer++;
            s.insert(n);
        }
    }

    return answer;
}
```

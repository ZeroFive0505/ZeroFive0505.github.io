---
layout: post
title: 가장 큰 수
date: 2021-08-31 08:12:48 +0900
categories: Programmers-Level2
---

# 가장 큰 수
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42746)

# 풀이
정렬을할때 기준만 다르게 정하면 간단하게 풀 수 있는 문제였다. 문자열 2개를 합쳐서 큰 수를 만들 수 있는 순서대로 정렬을 하고 만약 첫번째 원소가 0이면 최댓값은 0이 되는 예외를 처리한다. 그 이후에는 벡터를 순회하면서 정답에 넣으면 된다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

string solution(vector<int> numbers) 
{
    string answer = "";

    vector<string> v;

    for (int i : numbers)
        v.push_back(to_string(i));

    sort(v.begin(), v.end(), [](const string& a, const string& b) {
        return a + b > b + a;
    });

    if (v[0] == "0")
        return "0";

    for (const string& s : v)
        answer += s;

    return answer;
}
```

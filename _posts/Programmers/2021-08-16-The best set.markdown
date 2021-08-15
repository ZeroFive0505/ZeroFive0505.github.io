---
layout: post
title: 최고의 집합
date:  2021-08-16 08:10:09 +0900
categories: Programmers-Level3
---

# 최고의 집합
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12938)

# 풀이
먼저 n과 s의 갯수를 비교한다. 만약 n이 s보다 더 크다면 집합은 만들어낼 수가 없다 따라서 벡터에 -1을 담아서 반환한다. 만약 아니라면 두수의 곱이 최대가 되려면 집합의 크기로 평균을 내는것이 좋다. 따라서 먼저 평균 값을 정답 벡터에 집어넣고 만약 나머지가 있다면 그 수만큼 벡터를 순회하면서 1씩 더해준다. 그리고 정렬해서 반환한다.

# 코드
```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> solution(int n, int s) 
{
    vector<int> answer;

    if (n > s)
        return vector<int>{-1};

    int num = s / n;

    for (int i = 0; i < n; i++)
        answer.push_back(num);

    int remainder = s % n;

    for (int i = 0; i < remainder; i++)
        answer[i]++;

    sort(answer.begin(), answer.end());

	return answer;
}
```
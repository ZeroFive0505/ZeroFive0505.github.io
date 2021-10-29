---
layout: post
title: 없는 숫자 더하기
date: 2021-10-29 10:52:01 +0900
categories: Programmers-Level
---

# 없는 숫자 더하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/86051?language=cpp)

# 풀이
맵을 이용해서 먼저 빈도수를 초기화하고 빈도수를 센 다음 빈도수가 0인 숫자의 값만 더했다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <map>
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> numbers) 
{
	int answer = 0;

	map<int, int> hahsMap;

	for (int i = 0; i < 10; i++)
	{
		hahsMap[i] = 0;
	}

	for (int i = 0; i < numbers.size(); i++)
	{
		hahsMap[numbers[i]]++;
	}

	for (const auto& kv : hahsMap)
	{
		if (kv.second == 0)
			answer += kv.first;
	}

	return answer;
}
```

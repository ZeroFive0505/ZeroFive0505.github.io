---
layout: post
title: 위장
date: 2021-09-13 10:33:09 +0900
categories: Programmers-Level2
---

# 위장
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42578)

# 풀이
먼저 의상의 카테고리별로 숫자를 기록해둔다. 그 이후 해쉬를 순회하면서 경우의 수를 곱해주는데 이때 1을 더해서 곱해준다. 왜냐하면 해당 의상을 입을 경우와 입고 있지 않을 경우을 포함해서 세야하기 떄문이다. 마지막에는 -1을 해주는데 하나도 입지않은 경우는 제외해야하기 때문이다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <unordered_map>

using namespace std;

int solution(vector<vector<string>> clothes)
{
	int answer = 0;
	vector<int> v;
	unordered_map<string, vector<string>> m;
	for (int i = 0; i < clothes.size(); i++)
	{
		vector<string> s = clothes[i];
		m[s[1]].push_back(s[0]);
	}


	unordered_map<string, vector<string>>::const_iterator it;
	int mul = 1;
	for (it = m.begin(); it != m.end(); it++)
	{
		string cat = (*it).first;
		mul *= (m[cat].size() + 1);
	}
	
	answer = mul - 1;

	return answer;
}
```
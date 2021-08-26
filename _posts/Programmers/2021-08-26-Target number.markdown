---
layout: post
title: 타겟 넘버
date: 2021-08-26 08:26:31 +0900
categories: Programmers-Level2
---

# 타겟 넘버
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/43165)

# 풀이
DFS를 이용해서 풀 수 있었다. 처음 초기값은 0을 전달해서 시작하면서 최대 깊이에 도달할때까지 더하거나 빼면서 만약 타겟 넘버랑 같다면 카운트를 늘려준다.

# 코드
```c++
#include <iostream>
#include <vector>

using namespace std;

int cnt = 0;
int N;
void DFS(int depth, vector<int>& numbers, int sum, int target)
{
	if (depth == N)
	{
		if(sum == target)	
			cnt++;
		return;
	}

	DFS(depth + 1, numbers, sum + numbers[depth], target);
	DFS(depth + 1, numbers, sum - numbers[depth], target);
}

int solution(vector<int> numbers, int target)
{
	int answer = 0;
	N = numbers.size();
	DFS(0, numbers, 0, target);
	answer = cnt;
	return answer;
}
```

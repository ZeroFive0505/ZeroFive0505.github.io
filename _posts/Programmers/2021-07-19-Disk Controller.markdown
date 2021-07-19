---
layout: post
title: 디스크 컨트롤러
date: 2021-07-19 09:07:01 +0900
categories: Programmers-Level3
---

# 디스크 컨트롤러
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42627)

# 풀이
먼저 들어온 작업들을 들어온 시간대로 정렬을 한다. 그 이후 남아있는 작업이 있거나 또는 우선순위 큐가 비어있지 않다면 계속해서 반복문을 돈다.
작업이 남았거나, 지난 시간이 현재 처리해야할 작업의 시간의 요청시간보다 같거나 크다면, 그 작업은 우선순위 큐에 넣는다. 그리고 `continue`로 그 다음의 코드는 스킵한다. 여기서 중요한 점은 작업은 순서대로 확인하지만 우선순위 큐에는 걸리는 시간이 작은 순서대로 들어간다. 그 이후에는 큐가 안 비어있다면, 시간에 해당 작업 시간을 누적시키고 정답 변수에는 `흐른 시간 - 작업이 들어온 시간`을 누적시킨다. 만약 큐가 비었다면 흐른 시간을 현재 작업의 요청시간으로 바꾼다.

# 코드
```c++
#include <iostream>
#include <queue>
#include <algorithm>
#include <vector>

using namespace std;

int solution(vector<vector<int>> jobs)
{
	int answer = 0;

	sort(jobs.begin(), jobs.end());

	priority_queue <pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

	int idx = 0;
	int elapsedTime = 0;

	while (idx < jobs.size() || !pq.empty())
	{
		if (idx < jobs.size() && elapsedTime >= jobs[idx][0])
		{
			pq.push({ jobs[idx][1], jobs[idx][0] });
			idx++;
			continue;
		}

		if (!pq.empty())
		{
			elapsedTime += pq.top().first;
			answer += elapsedTime - pq.top().second;
			pq.pop();
		}
		else
			elapsedTime = jobs[idx][0];
	}

	return answer / jobs.size();
}
```
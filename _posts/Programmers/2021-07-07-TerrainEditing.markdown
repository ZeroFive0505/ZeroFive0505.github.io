---
layout: post
title: 지형 편집
date: 2021-07-07 11:37:30 +0900
categories: Programmers-Level4
---

# 지형 편집
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12984)

# 첫 시도
뭔가 이분탐색으로 풀면 되지 않을까 싶었는데.. 어느때 왼쪽 경계선과 오른쪽 경계선을 수정해야 할 지 도저히 감이 안와서 결국 이분탐색으로 작성하는 코드를 포기하고 정말 간단하게 최소, 최대의 평균 값으로 풀어봤다..

# 코드
```C++
#include <iostream>
#include <algorithm>
#include <limits.h>
#include <vector>
using namespace std;

long long solution(vector<vector<int>> land, int P, int Q)
{
	long long answer = 0;

	long long minH = LLONG_MAX;
	long long maxH = -1;
	for (int i = 0; i < land.size(); i++)
	{
		for (int j = 0; j < land[i].size(); j++)
		{
			maxH = max(maxH, (long long)land[i][j]);
			minH = min(minH, (long long)land[i][j]);
		}
	}
	long long avg = (maxH + minH) / 2;
	int add = 0;
	int removal = 0;
	for (int i = 0; i < land.size(); i++)
	{
		for (int j = 0; j < land[i].size(); j++)
		{
			long long res = avg - (long long)land[i][j];

			if (res > 0)
				add += res;
			else if (res < 0)
				removal += abs(res);
		}
	}

	answer = P * add + Q * removal;

	return answer;
}
```

# 결과
역시나 정답은 아니였다... 결국 자료 조사를 해본 결과 생각했던대로 이분탐색으로도 풀 수 있었으며 특이하게도 중간 지점까지의 값과 중간 지점에서 하나 더 큰 값을 비교하여 그 결과값으로 왼쪽, 오른쪽의 경계선을 새롭게 지정했다.

# 코드
```C++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

long long Calc(vector<vector<int>>& land, long long height, int P, int Q)
{
	long long ret = 0;

	for (int i = 0; i < land.size(); i++)
	{
		for (int j = 0; j < land[i].size(); j++)
		{
			long long diff = height - land[i][j];

			if (diff > 0)
				ret += (long long)diff * P;
			else if (diff < 0)
				ret += (long long)abs(diff) * Q;
		}
	}

	return ret;
}

long long solution(vector<vector<int>> land, int P, int Q)
{
	long long answer = 0;

	long long minH = land[0][0];
	long long maxH = land[0][0];
	for (int i = 0; i < land.size(); i++)
	{
		for (int j = 0; j < land[i].size(); j++)
		{
			maxH = max(maxH, (long long)land[i][j]);
			minH = min(minH, (long long)land[i][j]);
		}
	}

	long long mid = 0;

	while (minH <= maxH)
	{
		mid = (minH + maxH) / 2;

		if (minH == maxH)
			break;

        // mid 까지
		long long result1 = Calc(land, mid, P, Q);
        // mid + 1 까지
		long long result2 = Calc(land, mid + 1, P, Q);

        // 만약 같다면 나간다.
		if (result1 == result2)
			break;
        // 만약 mid까지 한 것이 더 작다면 maxH를 조정한다.
		else if (result1 < result2)
			maxH = mid;
		else
        // 만약 mid까지 한 것이 더 크다면 minH를 더 높게 설정.
			minH = mid + 1;
	}

	return Calc(land, mid, P, Q);
}
```
---
layout: post
title: 후보키
date: 2021-09-08 10:46:27 +0900
categories: Programmers-Level2
---

# 후보키
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42890)

# 풀이
최소키인지를 어떻게 확인해야 할 지를 모르겠어서 인터넷의 다른 분들의 풀이를 참고하여 푼 문제다. 먼저 열의 수만큼의 후보키의 조합이 가능하기에 먼저 열의 크기를 구하고 그 만큼의 경우의 수를 반복한다.
그 다음에는 행과 열을 순회하면서 키를 만들어 `set`에 넣는다. 만약 set의 사이즈가 행의 사이즈와 같다면 일단 이 키는 유일성은 만족한다는 뜻이다. 여기서 최소성을 확인하기 위해서 함수를 호출해서 만약 둘다 만족한다면 새로운 키 후보로 넣는다.

# 코드
```c++
#include <iostream>
#include <set>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

bool IsMinimum(vector<int>& candidates, int key)
{
	for (int i = 0; i < candidates.size(); i++)
	{
        // 만약 현재 키가 전에 들어온 키들중에 있다면 추가하면 안된다.
		if ((candidates[i] & key) == candidates[i])
			return false;
	}

	return true;
}

int solution(vector<vector<string>> relation)
{
	int answer = 0;
	int rowSize = relation.size();
	int colSize = relation[0].size();
	vector<int> candidates;
	int combinations = 1 << colSize;

    // 총 조합의 갯수만큼 반복한다.
	for (int i = 1; i < combinations; i++)
	{
		set<string> s;
		for (int j = 0; j < rowSize; j++)
		{
			unsigned int mask = 1;
			string key;
			for (int k = 0; k < colSize; k++, mask <<= 1)
			{
                // 비트연산이 참이라면 키에 더한다.
				if (i & mask)
				{
					key += relation[j][k];
				}
			}
            
            // set에 추가한다.
			s.insert(key);
		}

        // 만약 헌재 최소 키를 만족하면서 모든 키를 구별할 수 있다면 추가한다.
		if (IsMinimum(candidates, i) && s.size() == rowSize)
			candidates.push_back(i);
	}
	answer = candidates.size();
	return answer;
}
```
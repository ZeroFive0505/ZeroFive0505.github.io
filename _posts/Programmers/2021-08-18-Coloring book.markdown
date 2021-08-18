---
layout: post
title:  카카오프렌즈 컬러링북
date:  2021-08-18 09:17:48 +0900
categories: Programmers-Level2
---

# 카카오프렌즈 컬러링북
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/1829)

# 풀이
BFS나 DFS로 풀 수 있었던 문제였다. DFS함수에 들어갈때 카운트를 0이 아닌 1로 설정을 해야한다. 그리고 그 상태에서 상하좌우로 DFS가 가능한지 체크하고 그 수를 누적해서 전체 값을 반환한다.

# 코드
```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

const int INF = 987654321;

int M, N;

bool BoundaryCheck(int y, int x)
{
	if (y < 0 || x < 0 || y >= M || x >= N)
		return true;
	else
		return false;
}

int DFS(vector<vector<int>>& picture, int y, int x, vector<vector<bool>>& visited, int color)
{
	visited[y][x] = true;
	int cnt = 1;

	if (!BoundaryCheck(y + 1, x) && !visited[y + 1][x] && picture[y + 1][x] == color)
		cnt += DFS(picture, y + 1, x, visited, color);

	if (!BoundaryCheck(y - 1, x) && !visited[y - 1][x] && picture[y - 1][x] == color)
		cnt += DFS(picture, y - 1, x, visited, color);

	if (!BoundaryCheck(y, x - 1) && !visited[y][x - 1] && picture[y][x - 1] == color)
		cnt += DFS(picture, y, x - 1, visited, color);

	if (!BoundaryCheck(y, x + 1) && !visited[y][x + 1] && picture[y][x + 1] == color)
		cnt += DFS(picture, y, x + 1, visited, color);

	return cnt;

}

vector<int> solution(int m, int n, vector<vector<int>> picture)
{
	M = m;
	N = n;
	vector<int> answer(2, 0);
	vector<vector<bool>> visited(m, vector<bool>(n, false));


	int areaCnt = 0;
	int maxCnt = -INF;
	for (int i = 0; i < picture.size(); i++)
	{
		for (int j = 0; j < picture[i].size(); j++)
		{
			if (!visited[i][j] && picture[i][j] != 0)
			{
				areaCnt++;
				int cnt = DFS(picture, i, j, visited, picture[i][j]);
				maxCnt = max(maxCnt, cnt);
			}
		}
	}

	answer[0] = areaCnt;
	answer[1] = maxCnt;
	return answer;
}
```
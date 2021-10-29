---
layout: post
title: 퍼즐 조각 채우기
date: 2021-09-10 09:06:20 +0900
categories: Programmers-Level3
---

# 퍼즐 조각 채우기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/84021)

# 풀이
퍼즐 조각이 한 판에 모여있고 그걸 어떻게 떼어놓고 회전해야할지 전혀 감이 안 와서 인터넷에서 다른 분들의 풀이를 참조하였고 그 중 [여기서](https://jaimemin.tistory.com/1905) 좋은 풀이를 찾아서 참조하여 풀었다.
일단 퍼즐조각, 그리고 빈 곳은 단순 BFS로 구분을 한다. 그렇게 나눠진 조각의 기준점을 다시 잡아줘야하는데 여기서 `minTop`, `minLeft`를 구해서 빼는 방식으로 모든 퍼즐조각과 빈 곳의 좌표를 (0, 0)을 시작기준으로 옮겨버린다. 그 이후에는 퍼즐 조각들을 순회하면서 정확하게 딱 맞출 수 있는지를 확인하는데 이는 `map`을 이용했다. 키로는 현재의 좌표 그리고 값으로는 bool타입으로 선언하여 모든 퍼즐 좌표에 대해서 순회하고 만약 매칭이 된다면 참을 반환한다. 매칭은 총 4번 반복하는데 이는 회전이 총 4번이 가능하기 떄문이다. 퍼즐을 90도 회전할때는 기존 좌표 (y, x)에서 (x * -1, y)로 바뀌는데 이때 마찬가지로 `minTop`과 `minLeft`를 갱신해서 (0, 0)으로 옮겨준다. 이때 주의할점은 `minTop`의 경우에는 이제 x * -1과 비교를 해야하며 `minLeft`의 경우에는 y의 값과 비교한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <queue>
#include <map>
#include <cstring>

using namespace std;

const int MAX = 50;

struct sCoord
{
	int y;
	int x;

	bool operator<(const sCoord& rhs) const
	{
		if (y < rhs.y)
			return true;

		if (y == rhs.y && x < rhs.x)
			return true;

		return false;
	}
};

class Block
{
public:
	vector<sCoord> coords;

	void addCoords(sCoord coord)
	{
		coords.push_back(coord);
	}

	void Rotate90()
	{
		int minTop = MAX;
		int minLeft = MAX;

		vector<sCoord> temp;

		for (sCoord coord : coords)
		{
			temp.push_back({ coord.x * -1, coord.y });

			minTop = min(minTop, coord.x * -1);
			minLeft = min(minLeft, coord.y);
		}

		coords.clear();

		for (sCoord coord : temp)
		{
			coords.push_back({ coord.y - minTop, coord.x - minLeft });
		}
	}
};

struct sDelta
{
	int x;
	int y;
}Delta[] = {
	{-1, 0},
	{0, 1},
	{1, 0},
	{0, -1}
};

int result = 0;
int BOARDSIZE = 0;
int GAMEBOARD[MAX][MAX];
int TABLE[MAX][MAX];

bool VISITED[MAX][MAX];
vector<Block> candidateBlocks;
vector<Block> emptyBlocks;
vector<bool> candidateBlocksVistied;

void Init(vector<vector<int>> game_board, vector<vector<int>> table)
{
	BOARDSIZE = game_board.size();

	for (int i = 0; i < game_board.size(); i++)
	{
		for (int j = 0; j < game_board[i].size(); j++)
		{
			GAMEBOARD[i][j] = game_board[i][j];
		}
	}

	for (int i = 0; i < table.size(); i++)
	{
		for (int j = 0; j < table[i].size(); j++)
		{
			TABLE[i][j] = table[i][j];
		}
	}
}

vector<sCoord> GetArrangedCoords(vector<sCoord> temp)
{
	int minTop = MAX;
	int minLeft = MAX;

	for (sCoord coord : temp)
	{
		minTop = min(minTop, coord.y);
		minLeft = min(minLeft, coord.x);
	}

	vector<sCoord> coords;

	for (sCoord coord : temp)
	{
		coords.push_back({ coord.y - minTop, coord.x - minLeft });
	}

	return coords;
}

Block GetCandidateBlock(int y, int x)
{
	queue<sCoord> q;

	q.push({ y, x });

	VISITED[y][x] = true;

	Block block;

	vector<sCoord> temp;

	while (!q.empty())
	{
		sCoord cur = q.front();
		q.pop();

		for (int i = 0; i < 4; i++)
		{
			int nx = cur.x + Delta[i].x;
			int ny = cur.y + Delta[i].y;

			if (nx < 0 || ny < 0 || nx >= BOARDSIZE || ny >= BOARDSIZE)
				continue;

			if (VISITED[ny][nx] || TABLE[ny][nx] == 0)
				continue;

			VISITED[ny][nx] = true;
			q.push({ ny, nx });
		}

		temp.push_back({ cur.y, cur.x });
	}

	vector<sCoord> arrangedCoords = GetArrangedCoords(temp);

	for (sCoord coord : arrangedCoords)
	{
		block.addCoords(coord);
	}

	return block;
}

Block GetEmptyBlock(int y, int x)
{
	queue<sCoord> q;
	q.push({ y, x });
	VISITED[y][x] = true;

	Block block;

	vector<sCoord> temp;

	while (!q.empty())
	{
		sCoord cur = q.front();
		q.pop();

		for (int i = 0; i < 4; i++)
		{
			int nx = cur.x + Delta[i].x;
			int ny = cur.y + Delta[i].y;

			if (nx < 0 || ny < 0 || nx >= BOARDSIZE || ny >= BOARDSIZE)
				continue;

			if (VISITED[ny][nx] || GAMEBOARD[ny][nx])
				continue;

			VISITED[ny][nx] = true;
			q.push({ ny, nx });
		}

		temp.push_back({ cur.y, cur.x });
	}

	vector<sCoord> arragendCoords = GetArrangedCoords(temp);

	for (sCoord coord : arragendCoords)
	{
		block.addCoords(coord);
	}


	return block;
}

void GetCandidates()
{
	for (int y = 0; y < BOARDSIZE; y++)
	{
		for (int x = 0; x < BOARDSIZE; x++)
		{
			if (VISITED[y][x] || TABLE[y][x] == 0)
				continue;

			candidateBlocks.push_back(GetCandidateBlock(y, x));
		}
	}
}

void GetEmptyBlocks()
{
	memset(VISITED, false, sizeof(VISITED));

	for (int y = 0; y < BOARDSIZE; y++)
	{
		for (int x = 0; x < BOARDSIZE; x++)
		{
			if (VISITED[y][x] || GAMEBOARD[y][x])
				continue;

			emptyBlocks.push_back(GetEmptyBlock(y, x));
		}
	}
}

bool Match(Block empty, Block piece)
{
	if (empty.coords.size() != piece.coords.size())
		return false;

	map<sCoord, bool> check;

	for (sCoord coord : empty.coords)
	{
		check[coord] = true;
	}

	for (sCoord coord : piece.coords)
	{
		if (check[coord] == false)
			return false;
	}

	return true;
}

void solve(int depth, int cnt)
{
	if (depth == emptyBlocks.size())
	{
		result = max(result, cnt);

		return;
	}

	for (int i = 0; i < candidateBlocks.size(); i++)
	{
		if (candidateBlocksVistied[i])
			continue;

		for (int j = 0; j < 4; j++)
		{
			if (Match(emptyBlocks[depth], candidateBlocks[i]))
			{
				candidateBlocksVistied[i] = true;
				solve(depth + 1, cnt + candidateBlocks[i].coords.size());

				break;
			}

			candidateBlocks[i].Rotate90();
		}
	}

	solve(depth + 1, cnt);
}


int solution(vector<vector<int>> game_board, vector<vector<int>> table)
{
	Init(game_board, table);
	GetCandidates();
	GetEmptyBlocks();

	candidateBlocksVistied.resize(candidateBlocks.size(), false);

	solve(0, 0);

	return result;
}
```
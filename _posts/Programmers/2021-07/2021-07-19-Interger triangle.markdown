---
layout: post
title: 정수 삼각형
date: 2021-07-19 08:39:26 +0900
categories: Programmers-Level3
---

# 정수 삼각형
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/43105)

# 풀이 1
먼저 맨위에서 한 단계 내려와 맨 윗점의 정수와 합을 구한다. 그 이후에는
2번부터 반복문을 돌면서 j가 0이면 바로 위에서밖에 숫자를 못 가져오며, j가 바로 마지막 원소를 가리키고 있으면 대각선 위의 숫자를 가져올 수 있다. 이 두가지 경우를 제외한 나머지의 경우는 두가지 경로로 숫자를 가져올 수 있으므로 이런식으로 마지막까지 숫자를 더 하고 최대 값을 구하면 된다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int solution(vector<vector<int>> triangle) 
{
    int answer = 0;

    if (triangle.size() > 2)
    {
        triangle[1][0] += triangle[0][0];
        triangle[1][1] += triangle[0][0];

        for (int i = 2; i < triangle.size(); i++)
        {
            for (int j = 0; j < triangle[i].size(); j++)
            {
                if (j == 0)
                    triangle[i][j] += triangle[i - 1][j];
                else if (j == triangle[i].size() - 1)
                    triangle[i][j] += triangle[i - 1][j - 1];
                else
                    triangle[i][j] += max(triangle[i - 1][j - 1], triangle[i - 1][j]);
            }
        }

        for (int i = 0; i < triangle[triangle.size() - 1].size(); i++)
        {
            answer = max(answer, triangle[triangle.size() - 1][i]);
        }
    }
    else
    {
        triangle[1][0] += triangle[0][0];
        triangle[1][1] += triangle[0][0];

        answer = max(triangle[1][1], triangle[1][0]);
    }

    return answer;
}
```

# 풀이 2
이번에는 메모이제이션을 이용해서 풀어보았다. 만약 행이 다 끝났다면 바로 삼각형의 값을 반환하고 아니라면 캐시에 있는 값을 확인하고 -1이 아니라면 다음 줄로 내려가서 최댓값을 찾고 현재 삼각형의 값을 더해나간다.

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

const int SIZE = 501;

int cache[SIZE][SIZE];

int n;

vector<vector<int>> t;

int dp(int r, int c)
{
    if (r == n - 1)
        return t[r][c];

    int& ret = cache[r][c];

    if (ret != -1)
        return ret;

    return ret = max(dp(r + 1, c), dp(r + 1, c + 1)) + t[r][c];
}

int solution(vector<vector<int>> triangle) 
{
    int answer = 0;
    t = triangle;
    n = triangle.size();
    memset(cache, -1, sizeof(cache));
    answer = dp(0, 0);
    return answer;
}
```
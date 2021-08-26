---
layout: post
title: 행렬 테두리 회전하기
date: 2021-08-26 08:26:31 +0900
categories: Programmers-Level2
---

# 행렬 테두리 회전하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/77485)

# 풀이
먼저 행렬에 값을 채운뒤에 주어진 명령들을 수행한다. 두개의 좌표가 주어지므로 (x1, y1)과 (x2, y2)를 구한다. 그리고 그 좌표들로 순회를 하면서 값과 좌표를 기록한다. 가장작은 값을 배열에 담아야하므로 최솟값을 간단하게 값을 담은 배열을 순회하여 담고 이제 행렬을 회전시킨다. 처음부터 계속 값을 밀게되면 마지막값은 덮어쓰이기에 먼저 마지막 값을 가지고 있고 스택을 이용해서 값을 하나씩 민다. 그리고 좌표를 기록한 배열을 순회하면서 값을 갱신한다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <stack>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int rows, int columns, vector<vector<int>> queries) 
{
    vector<int> answer;
    vector<vector<int>> m(rows, vector<int>(columns, 0));

    int cnt = 1;
    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            m[i][j] = cnt++;
        }
    }

    for (int i = 0; i < queries.size(); i++)
    {
        vector<int> q = queries[i];
        vector<int> v;
        vector<pair<int, int>> coords;
        // x1, y1, x2, y2
        int x1 = q[0] - 1;
        int y1 = q[1] - 1;
        int x2 = q[2] - 1;
        int y2 = q[3] - 1;

        // 순서대로 왼쪽상단에서 오른쪽상단으로 넣는다.
        for (int j = y1; j < y2; j++)
        {
            v.push_back(m[x1][j]);
            coords.push_back({ x1, j });
        }

        // 오른쪽 상단에서 오른쪽하단으로
        for (int j = x1; j < x2; j++)
        {
            v.push_back(m[j][y2]);
            coords.push_back({ j, y2 });
        }

        // 오른쪽하단에서 왼쪽하단으로
        for (int j = y2; j > y1; j--)
        {
            v.push_back(m[x2][j]);
            coords.push_back({ x2, j });
        }

        // 왼쪽하단에서 왼쪽상단으로
        for (int j = x2; j > x1; j--)
        {
            v.push_back(m[j][y1]);
            coords.push_back({ j, y1 });
        }

        // 최솟값을 넣는다.
        answer.push_back(*min_element(v.begin(), v.end()));

        int last = v.back();
        int size = v.size();

        stack<int> s;
        s.push(v[0]);
        
        // 스택을 이용해서 값을 하나씩미룬다.
        for (int i = 1; i < size; i++)
        {
            int top = s.top();
            s.push(v[i]);
            v[i] = top;
        }

        v[0] = last;
    
        for (const auto& kv : coords)
        {
            m[kv.first][kv.second] = v[0];
            v.erase(v.begin());
        }

    }

    return answer;
}
```
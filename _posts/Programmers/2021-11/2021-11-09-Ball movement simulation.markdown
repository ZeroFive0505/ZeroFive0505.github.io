---
layout: post
title: 공 이동 시뮬레이션
date: 2021-11-09 08:21:48 +0900
categories: Programmers-Leve3
---

# 공 이동 시뮬레이션
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/87391?language=cpp)

# 풀이
행과 열의 범위가 엄청나게 커서 단순하게 공을 직접 모든 좌표를 순회하면서 시뮬레이션을 하면 무조건 시간초과가 뜰 것이라 생각했다. 그래서 혹시 도착점에서 반대로 시작하면 되지 않을까 싶었지만 이렇게 할 경우 어떻게 정확하게 해당 하는 모든 좌표의 갯수를 구할지 도무지 감이 오질 않았다. 결국에는 인터넷을 [참조](https://velog.io/@gkak1121/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EA%B3%B5-%EC%9D%B4%EB%8F%99-%EC%8B%9C%EB%AE%AC%EB%A0%88%EC%9D%B4%EC%85%98)하였다. 풀이 방법은 도착점에서 쿼리가 들어온 순서의 반대로 그리고 그 쿼리에 대한 이동도 반대로해서 갯수를 구한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;


long long solution(int n, int m, int x, int y, vector<vector<int>> queries) 
{
    long long answer = 0;

    long long colStart = y, colEnd = y;
    long long rowStart = x, rowEnd = x;


    int size = queries.size();
    
    for (int i = size - 1; i >= 0; i--)
    {
        int direction = queries[i][0];
        int move = queries[i][1];

        switch (direction)
        {
        // 원래는 열 번호가 감소하지만
        // 반대로 늘린다.
        case 0:
        {
            // 만약 시작 지점이 0이 아니라면 늘린다.
            if (colStart != 0)
                colStart = colStart + move;

            // 끝 지점도 늘리고
            colEnd = colEnd + move;

            // 만약 경계선을 넘어갔다면 고정
            if (colEnd > m - 1)
                colEnd = m - 1;
        }
            break;
        // 열 번호 증가지만 이것도 반대로
        case 1:
        {
            // 시작 지점은 감소하고
            colStart = colStart - move;

            // 경계선 확인
            if (colStart < 0)
                colStart = 0;

            // 최대치는 그대로 유지
            if (colEnd != m - 1)
                colEnd = colEnd - move;
        }
            break;
        case 2:
        {
            if (rowStart != 0)
                rowStart = rowStart + move;

            rowEnd = rowEnd + move;

            if (rowEnd > n - 1)
                rowEnd = n - 1;
        }
            break;
        case 3:
        {
            rowStart = rowStart - move;

            if (rowStart < 0)
                rowStart = 0;

            if (rowEnd != n - 1)
                rowEnd = rowEnd - move;
        }
            break;
        }

        if (rowStart > n - 1 || rowEnd < 0 || colStart > m - 1 || colEnd < 0)
            return answer;
    }

    answer = (rowEnd - rowStart + 1) * (colEnd - colStart + 1);

    return answer;
}
```
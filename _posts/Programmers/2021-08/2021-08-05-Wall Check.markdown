---
layout: post
title: 외벽 점검
date: 2021-08-05 09:15:24 +0900
categories: Programmers-Level3
---

# 외벽 점검
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/60062)

# 풀이
인터넷을 참고하여 원래 배열을 2배의 크기로 늘려서 푸는 방법으로 풀 수 있었던 문제였다. 먼제 원래 배열을 2배로 늘리고 원래 사이즈에 맞을 경우는 원래의 값을 넣어주고 넘을 경우 벽의 길이 만큼 더해서 넣어준다. 그 이후에는 검사할 수 있는 거리를 정렬하고 순열을 이용해서 푼다. 순열을 이용하는 이유는 검사하는 순서에 따라서 더 최적의 결과가 나올 수 있기에 그 경우의 수를 전부 체크해야 한다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

const int INF = 987654321;

int solution(int n, vector<int> weak, vector<int> dist) 
{
    int answer = INF;

    vector<int> temp(weak.size() * 2, 0);

    for (int i = 0; i < temp.size(); i++)
    {
        if (i < weak.size())
            temp[i] = weak[i];
        else
            temp[i] = weak[i - weak.size()] + n;
    }

    sort(dist.begin(), dist.end());

    do
    {
        for (int i = 0; i < weak.size(); i++)
        {
            int start = temp[i];
            int end = temp[i + weak.size() - 1];

            for (int j = 0; j < dist.size(); j++)
            {
                start += dist[j];

                if (start >= end)
                {
                    answer = min(answer, j + 1);
                    break;
                }

                int next = upper_bound(temp.begin(), temp.end(), start) - temp.begin();

                start = temp[next];
            }
        }
    } while (next_permutation(dist.begin(), dist.end()));

    if (answer == INF)
        return -1;

    return answer;
}
```
---
layout: post
title: 메뉴 리뉴얼
date: 2021-08-26 09:18:27 +0900
categories: Programmers-Level2
---

# 메뉴 리뉴얼
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/72411)

# 풀이
자세한 풀이는 [이 곳](https://hini7.tistory.com/166)을 참조했다. 맵을 이용해서 해당 코스의 크기에 가장 최대의 크기와 최대 크기가 될때 코스의 구성을 저장할 수 있게한다. 그리고 순열을 통해서 해당 주문에서 모든 조합을 따져서 다른 주문에도 이러한 조합이 있는지 확인하고 카운트를 늘려준다. 카운트가 만약 1을 초과한다면 새롭게 갱신하고 똑같다면 그대로 넣는다.

# 코드
```c++
#include <iostream>
#include <string>
#include <map>
#include <algorithm>
#include <vector>

using namespace std;

vector<string> solution(vector<string> orders, vector<int> course)
{
    vector<string> answer;
    map<int, int> maxCourseSize;
    map<int, vector<string>> maxResult;

    for (int i = 0; i < orders.size(); i++)
    {
        sort(orders[i].begin(), orders[i].end());
    }
    
    for (int i = 0; i < course.size(); i++)
    {
        maxCourseSize[course[i]] = 1;
        vector<string> v;
        maxResult[course[i]] = v;
    }

    while (orders.size())
    {
        string order = orders.front();
        int orderSize = order.size();
        vector<int> perm(orderSize, 0);
        orders.erase(orders.begin());

        for (int i = 0; i < course.size(); i++)
        {
            if (orderSize < course[i])
                continue;

            for (int j = orderSize - 1; j >= orderSize - course[i]; j--)
                perm[j] = 1;

            do
            {
                string courseComb;

                for (int j = 0; j < perm.size(); j++)
                {
                    if (perm[j] == 1)
                        courseComb.push_back(order[j]);
                }

                int match = 1;

                for (int j = 0; j < orders.size(); j++)
                {
                    if (includes(orders[j].begin(), orders[j].end(), courseComb.begin(), courseComb.end()))
                        match++;
                }

                if (match == 1)
                    continue;

                if (match > maxCourseSize[course[i]])
                {
                    maxCourseSize[course[i]] = match;
                    maxResult[course[i]].clear();
                    maxResult[course[i]].push_back(courseComb);
                }
                else if (match == maxCourseSize[course[i]])
                {
                    maxResult[course[i]].push_back(courseComb);
                }

            } while (next_permutation(perm.begin(), perm.end()));
        }
    }

    for (int i = 0; i < maxResult.size(); i++)
    {
        for (int j = 0; j < maxResult[i].size(); j++)
        {
            answer.push_back(maxResult[i][j]);
        }
    }

    sort(answer.begin(), answer.end());

    return answer;
}
```
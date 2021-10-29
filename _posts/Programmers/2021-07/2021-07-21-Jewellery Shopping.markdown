---
layout: post
title: 보석 쇼핑
date: 2021-07-21 08:18:44 +0900
categories: Programmers-Level3
---

# 보석 쇼핑
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/67258)

# 풀이
투 포인터 알고리즘을 이용해서 푸는 문제였다. 투 포인터 문제 유형은 많이 익숙하지 않아서 인터넷을 참고하여 풀었다. 먼제 `set`에 모든 보석 종류들을 넣는다. `set`에 특성상 중복되는 문자열은 제외하고 정확히 보석의 종류만큼의 크기를 가질 것이다. 그 이후 `map`에 보석들을 하나씩 넣어보면서 `map`의 크기가 `set`과 크기와 같아지면 빠져나온다. 이때 `end` 포인터를 하나씩 미리 증가시켜둔다. `MIN`에는 가장 최소 구간을 찾아야하므로 처음에는 `start` 부터 `end`까지의 범위를 저장해놓고 반복문을 돌면서 `start`에 있는 보석을 하나 빼보고 만약 그 보석이 하나밖에 없는 보석이었다면 `end`포인터를 하나 증가시키고 추가시키면서 해당 보석의 숫자가 1이 될 때까지 보석을 추가한다. 그리고 만약 최소 범위가 더 작아졌다면 새롭게 갱신을 한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <unordered_map>
#include <set>

using namespace std;

vector<int> solution(vector<string> gems) 
{
    vector<int> answer(2);
    unordered_map<string, int> hashMap;
    set<string> s(gems.begin(), gems.end());

    int start = 0;
    int end = 0;

    for (int i = 0; i < gems.size(); i++)
    {
        string key = gems[i];
        hashMap[key]++;

        if (hashMap.size() == s.size())
            break;
        end++;
    }

    int MIN = end - start;

    answer[0] = start + 1;
    answer[1] = end + 1;

    while (end < gems.size())
    {
        string key = gems[start];
        start++;
        hashMap[key]--;

        if (hashMap[key] == 0)
        {
            end++;
            for (; end < gems.size(); end++)
            {
                hashMap[gems[end]]++;

                if (hashMap[key] != 0)
                    break;
            }

            if (end == gems.size())
                break;
        }

        if (MIN > end - start)
        {
            MIN = end - start;
            answer[0] = start + 1;
            answer[1] = end + 1;
        }
    }

    return answer;
}
```
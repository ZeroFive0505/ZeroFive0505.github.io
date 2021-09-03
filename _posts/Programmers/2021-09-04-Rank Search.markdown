---
layout: post
title: 순위 검색
date: 2021-09-04 07:57:42 +0900
categories: Programmers-Level2
---

# 순위 검색
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/72412)

# 풀이
인터넷에 다른 사람들의 풀이를 참조하여 풀 수 있는 문제였다. 모든 조합을 만들어서 그것을 키값으로 하여 점수를 조회해서 풀어야하는 문제였다. 16번 만큼 반복하며 비트 연산을 통해 해당 키를 포함시킬지 말지를 정하여 점수를 넣는다. 조회하기전에 정렬을 하고나서 조회를 할때는 and를 무시하고 키를 만들어 조회후에 lower_bound를 통해 해당 점수보다 낮은 사람의 수를 세고 그 차를 정답에 넣는다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <algorithm>
#include <sstream>

using namespace std;


vector<int> solution(vector<string> info, vector<string> query) 
{
    vector<int> answer;
    map<string, vector<int>> hashMap;

    for (int i = 0; i < info.size(); i++)
    {
        stringstream ss(info[i]);
        vector<string> v;
        for (int j = 0; j < 4; j++)
        {
            string temp;
            ss >> temp;
            v.push_back(temp);
        }

        int score;

        ss >> score;

        for (int j = 0; j < 16; j++)
        {
            unsigned int mask = 1;
            string key;
            for (int k = 0; k < 4; k++, mask <<= 1)
            {
                if (j & mask)
                    key += v[k];
                else
                    key += "-";
            }

            hashMap[key].push_back(score);
        }
    }

    for (auto& kv : hashMap)
    {
        sort(kv.second.begin(), kv.second.end());
    }

    for (int i = 0; i < query.size(); i++)
    {
        stringstream ss(query[i]);
        string key;
        for (int j = 0; j < 7; j++)
        {
            string temp;

            ss >> temp;

            if (temp != "and")
                key += temp;
        }

        int score;

        ss >> score;

        int idx = lower_bound(hashMap[key].begin(), hashMap[key].end(), score) - hashMap[key].begin();

        answer.push_back(hashMap[key].size() - idx);
    }

    return answer;
}
```
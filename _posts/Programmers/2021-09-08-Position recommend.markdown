---
layout: post
title: 직업군 추천하기
date: 2021-09-08 10:15:47 +0900
categories: Programmers-Level1
---

# 직업군 추천하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/84325)

# 풀이
문자열을 계속 쪼개가면서 해당 문자열이 찾고자 하는 문자열안에 속해있는지 찾았다. 먼저 임시 문자열에 공백을 마지막에 넣었는데 이는 공백단위로 쪼개가다보면 마지막 문자열은 탐색을 하지않는 경우가 생겨서 이렇게 했다. 모든 태그에 대해서 점수를 구한다음에 정렬해서 첫번째 값을 취한다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

string solution(vector<string> table, vector<string> languages, vector<int> preference) 
{
    string answer = "";

    vector<pair<string, int>> v;

    for (int i = 0; i < table.size(); i++)
    {
        string temp = table[i];

        size_t pos;

        pos = temp.find(" ");

        // 먼재 현재 태그를 가져온다.
        string tag = temp.substr(0, pos);

        // 마지막 문자열까지 확인하기 위해서 마지막에 공백을 넣는다.
        temp.push_back(' ');

        // 가장 처음 배수는 5
        int index = 5; 

        int score = 0;

        temp = temp.substr(pos + 1, temp.size());

        while ((pos = temp.find(" ")) != string::npos)
        {
            string lang = temp.substr(0, pos);

            for (int j = 0; j < languages.size(); j++)
            {
                if (languages[j] == lang)
                {
                    score += preference[j] * index;
                    break;
                }
            }

            temp = temp.substr(pos + 1, temp.size());

            index--;
        }


        v.push_back({ tag, score });
    }
    

    // 점수 기준으로 정렬한다. 하지만 점수가 같다면 사전순으로 정렬한다.
    sort(v.begin(), v.end(), [](const pair<string, int>& a, const pair<string, int>& b) {
        if (a.first != b.first && a.second != b.second)
            return a.second > b.second;
        else
            return a.first < b.first;
    });

    // 가장 처음 값을 취한다.
    answer = v[0].first;

    return answer;
}
```

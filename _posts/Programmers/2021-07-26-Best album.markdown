---
layout: post
title: 베스트 앨범
date: 2021-07-26 08:19:37 +0900
categories: Programmers-Level3
---

# 베스트 앨범
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42579)

# 풀이
조금 복잡했던 해쉬 문제였다. 일단 장르의 아이디들을 기록할 해쉬와 몇번 재생되었는지의 해쉬를 준비한다. 그리고 마지막으로 랭킹을 기록할 해쉬를 만든다. 먼저 순회하면서 초기화를 해주고 그 이후에 해당 장르의 최종 재생 횟수를 구한 다음에 랭킹에 기록한다.
랭킹을 순회할때는 거꾸로 순회하는데 `map`의 경우 자동으로 정렬이 오름차순으로 되어 있어서 거꾸로 순회하면 가장 많은 재생 횟수를 가지는 장르가 나온다. 이제 그 장르를 키로 해당 장르와 동일한 아이디를 가져오고 순회하면서 임시 벡터에 넣고 재생횟수 순으로 정렬하고 2개만 정답에 집어 넣으면 된다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <map>

using namespace std;


vector<int> solution(vector<string> genres, vector<int> plays) 
{
    vector<int> answer;
    int id = 0;
    map<string, vector<int>> hashGens;
    map<int, int> playTimes;
    map<int, string> topChart;

    // 아이디를 이용해서 기록한다.
    for (int i = 0; i < genres.size(); i++)
    {
        string gen = genres[i];
        hashGens[gen].push_back(id);
        playTimes[id] = plays[i];
        id++;
    }

    // hashGens에는 아이디들이 벡터로 담겨 있다. 그걸 순회하면서 횟수를 누적한다.
    for (int i = 0; i < genres.size(); i++)
    {
        string gen = genres[i];
        vector<int> v = hashGens[gen];
        int sum = 0;
        for (auto i : v)
            sum += playTimes[i];
        topChart[sum] = gen;
    }

    // 장르를 가져오고 해당 장르들을 순회하면서 임시 벡터에 넣는다.
    for (auto i = topChart.rbegin(); i != topChart.rend(); i++)
    {
        string gen = (*i).second;
        
        vector<int> v = hashGens[gen];
        vector<pair<int, int>> temp;

        for (auto i : v)
        {
            int p = playTimes[i];
            temp.push_back({ i, p });
        }


        // 재생 횟수순으로 정렬한다.
        sort(temp.begin(), temp.end(), [](const pair<int, int>& a, const pair<int, int>& b) {
            return a.second > b.second;
        });

        int cnt = 0;

        for (int i = 0; i < temp.size() && cnt != 2; i++, cnt++)
        {
            answer.push_back(temp[i].first);
        }

    }


    return answer;
}
```
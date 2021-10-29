---
layout: post
title: 복서 정렬하기
date: 2021-09-08 09:27:47 +0900
categories: Programmers-Level1
---

# 복서 정렬하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/85002)

# 풀이
먼저 문제에 주어진 기준대로 정렬하기 위해서 구조체를 하나 만들었다. 거기에 반복문을 돌면서 값을 알맞게 구해서 벡터에 넣어준다. 주의할 점은 승률을 구할때 전체 인원수에서 자기 자신을 뺀 인원수로 나누는게 아니라 경기 갯수를 따로 구해줘야한다. 그리고 그냥 `sort`를 쓰면 11번 12번 테스트에서 오답판정을 받는다. 따라서 `stable_sort`를 해줘서 똑같은 수치가 나온 선수가 멀리떨어져 있어도 그 상대적인 위치는 바뀌지 않도록 한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

struct sBoxer
{
    int id;
    int nHeavier;
    int weight;
    double winRate;
};

vector<int> solution(vector<int> weights, vector<string> head2head) 
{
    vector<int> answer;

    vector<sBoxer> v;

    for (int i = 0; i < weights.size(); i++)
    {
        sBoxer boxer;
        boxer.id = i + 1;
        boxer.weight = weights[i];
        int heavierCnt = 0;
        int winCnt = 0;
        int nMatchCount = 0;
        for (int j = 0; j < head2head[i].size(); j++)
        {
            if (head2head[i][j] == 'W')
            {
                winCnt++;
                if (weights[j] > weights[i])
                    heavierCnt++;
                nMatchCount++;
            }
            else if (head2head[i][j] == 'L')
                nMatchCount++;
        }

        double winRate;

        if (nMatchCount == 0)
            winRate = 0.0;
        else
            winRate = winCnt / ((double)nMatchCount);

        boxer.nHeavier = heavierCnt;
        boxer.winRate = winRate;

        v.push_back(boxer);
    }

    stable_sort(v.begin(), v.end(), [](const sBoxer& a, const sBoxer& b) {
        if (a.winRate != b.winRate)
            return a.winRate > b.winRate;
        else if (a.winRate == b.winRate && a.nHeavier != b.nHeavier)
            return a.nHeavier > b.nHeavier;
        else if (a.nHeavier == b.nHeavier)
            return a.weight > b.weight;
        else
            return a.id < b.id;
    });

    for (int i = 0; i < v.size(); i++)
    {
        answer.push_back(v[i].id);
    }

    return answer;
}
```
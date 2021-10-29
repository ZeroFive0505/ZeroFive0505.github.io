---
layout: post
title: 뉴스 클러스터링
date: 2021-08-30 07:54:59 +0900
categories: Programmers-Level2
---

# 뉴스 클러스터링
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/17677)

# 풀이
문제에 예시에서 다중집합 예기가 나오길래 multiset으로 풀어보려고 했는데 잘 안되서 결국 인터넷을 참조하여 풀었다. 간단하게 벡터를 이용해서 풀 수 있었다. 먼저 문자열 1, 2를 부분문자열 2개로 나눈다. 그리고 알파벳인지를 확인하고 벡터에 넣는다. 이렇게 모든 문자열에 대해서 집합을 구하고 합집합은 간단하게 그 두 벡터의 합으로 나타낸다. 그리고 집합의 크기가 더 큰쪽에서 작은쪽의 집합의 원소를 빼고 교집합의 수를 늘린다. 마지막으로 합집합에서 교집합의 수만큼 빼고 유사도를 반환한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

const int NUM = 65536;

int solution(string str1, string str2) 
{
    int answer = 0;
    int nIntersection = 0;
    int nUnion = 0;

    transform(str1.begin(), str1.end(), str1.begin(), ::toupper);
    transform(str2.begin(), str2.end(), str2.begin(), ::toupper);

    vector<string> set1, set2;

    for (int i = 0; i < str1.size() - 1; i++)
    {
        string sub = str1.substr(i, 2);
        if (isalpha(sub[0]) && isalpha(sub[1]))
            set1.push_back(sub);
    }

    for (int i = 0; i < str2.size() - 1; i++)
    {
        string sub = str2.substr(i, 2);
        if (isalpha(sub[0]) && isalpha(sub[1]))
            set2.push_back(sub);
    }

    // 만약 두 집합 다 비어있다면 유사도는 최대
    if (set1.empty() && set2.empty())
        return NUM;

    nUnion = set1.size() + set2.size();

    if (set1.size() > set2.size())
    {
        for (int i = 0; i < set2.size(); i++)
        {
            auto iter = find(set1.begin(), set1.end(), set2[i]);

            if (iter != set1.end())
            {
                set1.erase(iter);
                nIntersection++;
            }
        }
    }
    else
    {
        for (int i = 0; i < set1.size(); i++)
        {
            auto iter = find(set2.begin(), set2.end(), set1[i]);

            if (iter != set2.end())
            {
                set2.erase(iter);
                nIntersection++;
            }
        }
    }


    nUnion -= nIntersection;

    // 0으로 나눌수는 없으니 이때도 최대치가된다.
    if (nUnion == 0)
        return NUM;

    answer = (double)nIntersection / nUnion * NUM;

    return answer;
}
```
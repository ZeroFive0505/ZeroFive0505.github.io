---
layout: post
title:  단체사진 찍기
date:  2021-08-20 09:34:45 +0900
categories: Programmers-Level2
---

# 단체사진 찍기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/1835)

# 풀이
순열을 이용해서 풀 수 있는 문제였다. 순열을 돌리면서 해당 순열이 조건을 만족하는지 확인하고 만족할 경우 갯수를 늘려준다.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool Check(char comp, int dist, int realDist)
{
    switch (comp)
    {
    case '=':
        return dist == realDist;
    case '>':
        return realDist > dist;
    case '<':
        return realDist < dist;
    }
}

int solution(int n, vector<string> data) 
{
    int answer = 0;
    string friends = "ACFJMNRT";
    sort(friends.begin(), friends.end());
  
    do
    {
        bool match = true;
        for (int i = 0; i < data.size(); i++)
        {
            string s = data[i];

            char friendA = s[0];
            char friendB = s[2];

            char comp = s[3];
            int desireDist = s[4] - '0';

            int distA = -1;
            int distB = -1;

            // 현재 순열에서 거리를 구한다.
            for (int i = 0; i < friends.size(); i++)
            {
                if (friends[i] == friendA)
                    distA = i;

                if (friends[i] == friendB)
                    distB = i;

                if (distA != -1 && distB != -1)
                    break;
            }
            
            // 1을 빼준다. 왜냐하면 바로 옆에 붙어있을 경우 거리는 0이다 1이아니라
            int currentDist = abs(distB - distA) - 1;

            // 확인해본다.
            if (Check(comp, desireDist, currentDist))
                continue;

            // 아니라면 반복문을 빠져나간다.
            match = false;  
            break;
        }

        if (match)
            answer++;

    } while (next_permutation(friends.begin(), friends.end()));

    return answer;
}
```

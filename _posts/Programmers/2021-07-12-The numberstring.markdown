---
layout: post
title: 숫자 문자열과 영단어
date: 2021-07-12 07:13:09 +0900
categories: Programmers-Level1
---

# 숫자 문자열과 영단어
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/43164)

# 풀이
map을 이용해서 일단 각 문자열과 매칭되는 정수 값을 할당하고 정답 변수에 자릿수에 알맞게 더해주면 된다.

# 코드
```C++
#include <iostream>
#include <ctype.h>
#include <map>
#include <string>
#include <vector>

using namespace std;

int solution(string s)
{
    int answer = 0;
    map<string, int> hashMap;

    hashMap["zero"] = 0;
    hashMap["one"] = 1;
    hashMap["two"] = 2;
    hashMap["three"] = 3;
    hashMap["four"] = 4;
    hashMap["five"] = 5;
    hashMap["six"] = 6;
    hashMap["seven"] = 7;
    hashMap["eight"] = 8;
    hashMap["nine"] = 9;

    for (int i = 0; i < s.size();)
    {
        if (isdigit(s[i]))
        {
            int num = s[i] - '0';
            answer = answer * 10 + num;
            i++;
        }
        else
        {
            int j;
            string temp;
            for (j = i; j < s.size(); j++)
            {
                temp.push_back(s[j]);

                if (hashMap.find(temp) != hashMap.end())
                {
                    int num = hashMap[temp];
                    answer = answer * 10 + num;
                    break;
                }
            }

            i += j - i;
            i++;
        }
    }

    return answer;
}
```
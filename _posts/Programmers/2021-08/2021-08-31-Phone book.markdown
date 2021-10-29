---
layout: post
title: 전화번호 목록
date: 2021-08-31 08:28:33 +0900
categories: Programmers-Level2
---

# 전화번호 목록
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42577)

# 풀이
해쉬를 이용해서 풀 수 있었다. 먼제 해쉬를 이용해서 카운트를 세주고 전화번호 목록을 순회하면서 임시 번호를 만든다. 그 만든 번호로 해쉬에서 조회를 하고 만약 0이아닌 수가 나오고 또 자기자신과 다른 번호라면 거짓을 반환한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>
#include <unordered_map>

using namespace std;

bool solution(vector<string> phone_book) 
{

    unordered_map<string, int> m;
    
    for (const string& s : phone_book)
        m[s] = 1;

    for (int i = 0; i < phone_book.size(); i++)
    {
        string temp;
        for (int j = 0; j < phone_book[i].size(); j++)
        {
            temp += phone_book[i][j];

            if (m.count(temp) && temp != phone_book[i])
                return false;
        }
    }

    return true;
}
```
---
layout: post
title: 모음 사전
date: 2021-09-06 10:28:10 +0900
categories: Programmers-Level2
---

# 모음 사전
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/84512)

# 풀이
DFS를 이용해서 일단 가능한 모든 경우의 수를 구했다. 처음에는 깊이만큼 도달했을때 벡터에 추가를 하는 식으로 했는데 이런식으로는 5글자들로 꽉꽉 채운 경우만 구할 수 있었다. 그래서 반복문을 돌면서 문자열에 추가되는 순간에 같이 벡터에 추가시켰다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

const char vowel[] = { 'A', 'E', 'I', 'O', 'U' };
vector<string> v;

void recur(int depth, string temp)
{
    if (depth == 5)
        return;

    for (int i = 0; i < 5; i++)
    {
        temp.push_back(vowel[i]);
        // 문자가 추가되는 순간 같이 넣어준다.
        v.push_back(temp);
        recur(depth + 1, temp);
        temp.pop_back();
    }
}

int solution(string word) 
{
    int answer = 0;

    // 가장 처음에는 빈문자열을 넣어준다.
    v.push_back("");
    recur(0, "");

    sort(v.begin(), v.end());

    for (int i = 0; i < v.size(); i++)
    {
        if (word == v[i])
        {
            answer = i;
            break;
        }
    }

    return answer;
}
```
---
layout: post
title: 자동 완성
date: 2021-07-07 10:13:45 +0900
categories: Programmers-Level4
---

# 자동 완성
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/17685)

# 풀이
보자마자 트라이가 생각나는 문제였다. 가사 검색도 그렇고 레벨4에는 트라이 문제가 자주 출체되는 모양이다.. 푸는 도중에 자식의 숫자를 잘못된 방법으로 세서 도저히 정답이 안 나와서 결국에는 다른 사람의 답을 참조했는데 새롭게 노드를 할당할때 자식의 수를 늘리는 것이 아니라 무조건 삽입되는 순간에 자식의 수를 늘렸어야 했다.. 따라서 자식의 수가 1이거나 또는 종료된 문자일 경우의 그 수를 세서 반환한다. 트라이를 제대로 익혀놔야겠다.

# 코드
```C++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

struct sTrie
{
    bool terminate;
    int childCount;
    sTrie* next[26];

    sTrie() : terminate(false), childCount(0)
    {
        fill(next, next + 26, nullptr);
    }

    ~sTrie()
    {
        for (int i = 0; i < 26; i++)
        {
            if (next[i])
                delete next[i];
        }
    }

    void Insert(const char* key)
    {
        childCount++;
        if (*key == '\0')
            terminate = true;
        else
        {
            int cur = *key - 'a';

            if (next[cur] == nullptr)
                next[cur] = new sTrie();
            next[cur]->Insert(key + 1);
        }
    }

    int find(const char* key, int cnt)
    {
        if (*key == '\0' || childCount == 1)
            return cnt;

        int cur = *key - 'a';

        return next[cur]->find(key + 1, cnt + 1);
    }
};

int solution(vector<string> words) 
{
    int answer = 0;
    sTrie trie;

    for (int i = 0; i < words.size(); i++)
    {
        transform(words[i].begin(), words[i].end(), words[i].begin(), ::tolower);
        trie.Insert(words[i].c_str());
    }


    for (int i = 0; i < words.size(); i++)
    {
        answer = answer + trie.find(words[i].c_str(), 0);
    }


    return answer;
}
```
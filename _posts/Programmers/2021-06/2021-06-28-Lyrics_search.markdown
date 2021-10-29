---
layout: post
title: 가사 검색
date: 2021-06-28 13:53:25 +0900
categories: Programmers-Level4
---

# 가사 검색  
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/60060)

# 풀이
형식이 프로그래머스 Level 3의 불량 유저랑 비슷해서 그거랑 비슷한 느낌으로 풀어봤다.. 코드는 아래와 같다.
```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

int match(vector<string>& words, string& q)
{
    int cnt = 0;
    for (int i = 0; i < words.size(); i++)
    {
        if (words[i].size() != q.size())
            continue;

        int j;
        for (j = 0; j < q.size(); j++)
        {
            if (q[j] == '?')
                continue;

            if (words[i][j] != q[j])
                break;
        }

        if (j == words[i].size())
            cnt++;
    }

    return cnt;
}

vector<int> solution(vector<string> words, vector<string> queries) 
{
    vector<int> answer;

    for (int i = 0; i < queries.size(); i++)
    {
        string q = queries[i];
        int cnt = match(words, q);
        answer.push_back(cnt);
    }

    return answer;
}
```
# 실패
효율성 2개 빼고는 전부 다 맞았지만 효율성을 상승시킬 방법이 마땅히 떠오르지 않아 인터넷을 검색해봤다.

# 해결책
Trie라는 자료구조를 이용하는 것이다. 예를들어 알파벳을 저장하는 Trie라고 한다면 26개의 배열이 하나의 노드 형태가 되고 또 각 배열의 요소 마다 또 26개의 알파벳을 자식으로 가지는 형태이다. Trie를 각 문장의 길이별로 저장하고 일반 Trie뿐만 아니라 역 문자열을 저장하기 위한 역 Trie를 만들어 쿼리를 처리할때 끝문자가 '?'로 끝나면 일반 Trie를 아닐 경우에는 역 Trie를 이용해서 검색한다. 코드는 아래와 같다.

```C++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

struct sTrie
{
    sTrie* node[26];
    int count;
    bool terminate;

    sTrie() : terminate(false), count(1)
    {
        for (int i = 0; i < 26; i++)
            node[i] = nullptr;
    }

    ~sTrie()
    {
        for (int i = 0; i < 26; i++)
        {
            if (node[i])
                delete node[i];
        }
    }

    void Insert(const char* key)
    {
        if (*key == '\0')
            terminate = true;
        else
        {
            int currIdx = *key - 'a';
            if (node[currIdx] == nullptr)
                node[currIdx] = new sTrie();
            else
                node[currIdx]->count++;
            node[currIdx]->Insert(key + 1);
        }
    }

    int Find(const char* key)
    {
        int currIdx = *key - 'a';

        if (*key == '?')
        {
            int temp = 0;

            for (int i = 0; i < 26; i++)
            {
                if (node[i] != nullptr)
                    temp += node[i]->count;
            }

            return temp;
        }

        if (node[currIdx] == nullptr)
            return 0;

        if (*(key + 1) == '?')
            return node[currIdx]->count;

        return node[currIdx]->Find(key + 1);
    }
};

const int SIZE = 10001;

sTrie* trie[SIZE];
sTrie* reverse_trie[SIZE];

vector<int> solution(vector<string> words, vector<string> queries) 
{
    int wSize = words.size();
    int qSize = queries.size();
    vector<int> answer(qSize, 0);

    for (int i = 0; i < wSize; i++)
    {
        int size = words[i].size();

        const char* key = words[i].c_str();

        if (trie[size] == nullptr)
            trie[size] = new sTrie();

        trie[size]->Insert(key);

        string r_str = words[i];

        reverse(r_str.begin(), r_str.end());

        const char* r_key = r_str.c_str();

        if (reverse_trie[size] == nullptr)
            reverse_trie[size] = new sTrie();

        reverse_trie[size]->Insert(r_key);
    }

    int idx = 0;

    for (auto& s : queries)
    {
        int size = s.size();

        if (s[size - 1] == '?')
        {
            const char* c = s.c_str();

            if (trie[size] == nullptr)
            {
                idx++;
                continue;
            }
            else
                answer[idx] = trie[size]->Find(c);
        }
        else
        {
            string rev = s;
            reverse(rev.begin(), rev.end());

            const char* c = rev.c_str();

            if (reverse_trie[size] == nullptr)
            {
                idx++;
                continue;
            }
            else
                answer[idx] = reverse_trie[size]->Find(c);
        }

        idx++;
    }

    return answer;
}
```
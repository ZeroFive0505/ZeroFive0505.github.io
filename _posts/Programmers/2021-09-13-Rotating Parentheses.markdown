---
layout: post
title: 괄호 회전하기
date: 2021-09-13 10:24:03 +0900
categories: Programmers-Level2
---

# 괄호 회전하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/76502)

# 풀이
덱을 이용해서 문자열을 회전시키고 스택을 이용하여 짝이 맞는지 확인을 해서 풀 수 있었다.

# 코드
```c++
#include <iostream>
#include <string>
#include <queue>
#include <stack>
#include <vector>

using namespace std;

bool Check(string s)
{
    stack<char> st;
    int size = s.size();

    for (int i = 0; i < size; i++)
    {
        char ch = s[i];

        if (ch == '(' || ch == '[' || ch == '{')
            st.push(ch);
        else
        {
            if (!st.empty())
            {
                if ((st.top() == '(' && ch == ')'
                    || st.top() == '{' && ch == '}'
                    || st.top() == '[' && ch == ']'))
                    st.pop();
            }
            else
                return false;
        }
    }

    if (st.empty())
        return true;
    else
        return false;
}

string Rotate(string s, int cnt)
{
    deque<char> dq;
    int size = s.size();

    // 사이즈만큼 덱에 넣고
    for (int i = 0; i < size; i++)
        dq.push_back(s[i]);

    // 카운트만큼 덱 앞에서 꺼내서 뒤에 넣는다.
    for (int i = 0; i < cnt; i++)
    {
        char front = dq.front();
        dq.pop_front();
        dq.push_back(front);
    }

    
    string ret(dq.begin(), dq.end());

    return ret;
}

int solution(string s) 
{
    int answer = 0;
    int size = s.size();

    // 총 길이 -1 만큼 반복한다. 길이만큼 반복하게되면 결국 한번도 돌리지 않게되는 것이랑 마찬가지이기에
    for (int i = 0; i < size - 1; i++)
    {
        if(Check(Rotate(s, i)))
            answer++;
    }

    return answer;
}
```

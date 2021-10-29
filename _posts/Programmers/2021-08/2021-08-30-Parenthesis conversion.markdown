---
layout: post
title: 괄호 변환
date: 2021-08-30 07:36:08 +0900
categories: Programmers-Level2
---

# 괄호 변환
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/60058)

# 풀이
문제가 잘 이해가 되지 않아 인터넷을 참고하여 풀었던 문제다. 문자열을 올바른 괄호 문자열인지를 확인한다.
그 다음에는 문자열을 균형잡힌 문자열과 아닌 문자열 u, v로 분리하는데 균형잡힌 문자열의 경우 여는 괄호의 수와 닫는 괄호의 수가 동일하면 거기까지를 부분문자열 u로 만들고 그 이후를 v로 만든다. 그리고 만약 u가 균형잡힌 문자열이라면 v를 다시 u, v로 나누는 작업을 반복하고 아니라면 문제에 나온대로 임시문자열에 여는 괄호와 v를 재귀적으로 호출한 다음 붙이고 닫는 괄호를 넣고 u의 첫문자와 마지막 문자를 빼고 반대로 넣어서 반환한다.

# 코드
```c++
#include <iostream>
#include <stack>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

bool Check(const string& s)
{
    stack<char> st;

    for (int i = 0; i < s.size(); i++)
    {
        if (st.empty())
        {
            st.push(s[i]);
        }
        else
        {
            if (s[i] == '(')
                st.push(s[i]);
            else if (!st.empty() && s[i] == ')')
                st.pop();
        }
    }

    if (st.empty())
        return true;
    else
        return false;
}

string recur(string p)
{
    if (p.empty())
        return "";
    int openCount = 0;
    int closeCount = 0;

    string u, v;

    for (int i = 0; i < p.size(); i++)
    {
        if (p[i] == '(')
            openCount++;
        else if (p[i] == ')')
            closeCount++;

        if (openCount == closeCount)
        {
            u = p.substr(0, i + 1);
            v = p.substr(i + 1);
            break;
        }
    }

    if (Check(u))
        return u + recur(v);

    string temp = "(";

    temp += recur(v);

    temp += ")";

    u.erase(u.begin());
    u.pop_back();

    for (int i = 0; i < u.size(); i++)
    {
        if (u[i] == '(')
            temp += ')';
        else
            temp += '(';
    }

    return temp;
}

string solution(string p)
{
    if (Check(p))
        return p;

    return recur(p);
}
```

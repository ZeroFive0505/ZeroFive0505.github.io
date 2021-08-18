---
layout: post
title:  짝지어 제거하기
date:  2021-08-18 09:18:17 +0900
categories: Programmers-Level2
---

# 짝지어 제거하기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/1829)

# 풀이
스택을 이용해서 풀 수 있었다. 간단하게 스택이 비어있으면 푸시하고 아니라면 탑과 현재의 문자를 비교하고 같다면 빼내고 아니라면 푸시한다. 그리고 마지막으로 스택이 비어있다면 1을 반환 아니라면 0을 반환한다.

# 코드
```c++
#include <iostream>
#include <stack>
#include <string>

using namespace std;

int solution(string s)
{
    int answer = 0;
    stack<char> st;
    
    for (int i = 0; i < s.size(); i++)
    {
        if (st.empty())
            st.push(s[i]);
        else
        {
            if (st.top() == s[i])
                st.pop();
            else
                st.push(s[i]);
        }
    }

    if (st.empty())
        answer = 1;
    else
        answer = 0;

    return answer;
}
```
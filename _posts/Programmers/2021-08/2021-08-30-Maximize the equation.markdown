---
layout: post
title: 수식 최대화
date: 2021-08-30 08:24:49 +0900
categories: Programmers-Level2
---

# 수식 최대화
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/67257)

# 풀이
순열과 후위표기식으로 풀었다. 순열로 각 연산자가 최대 우선순위일때의 후위표기식을 구한다음 최댓값을 취했다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <stack>
#include <ctype.h>
#include <string>
#include <vector>

using namespace std;

int GetPriority(string& ops, char op)
{
    if (ops[0] == op)
        return 3;
    else if (ops[1] == op)
        return 2;
    else if (ops[2] == op)
        return 1;
}

long long solution(string expression) 
{
    long long answer = 0;
    bool minus = false;
    bool plus = false;
    bool mul = false;
    string ops;
    for (int i = 0; i < expression.size(); i++)
    {
        if (!minus && expression[i] == '-')
        {
            ops.push_back('-');
            minus = true;
        }
        else if (expression[i] == '+')
        {
            ops.push_back('+');
            plus = true;
        }
        else if (expression[i] == '*')
        {
            ops.push_back('*');
            mul = true;
        }
    }

    sort(ops.begin(), ops.end());

    do
    {
        string eq;
        string temp;
        stack<char> op_st;
        for (int i = 0; i < expression.size(); i++)
        {
            // 숫자라면 일단 문자열에 추가한다.
            if (isdigit(expression[i]))
                temp.push_back(expression[i]);
            else if (expression[i] == '-' || expression[i] == '*' || expression[i] == '+')
            {
                eq += temp;
                // 공백을 이용해서 숫자간 구별을 한다.
                eq += " ";
                temp.clear();
                if (op_st.empty())
                    op_st.push(expression[i]);
                else
                {
                    while (!op_st.empty() && GetPriority(ops, op_st.top()) <= GetPriority(ops, expression[i]))
                    {
                        char op = op_st.top();
                        op_st.pop();
                        temp.push_back(op);
                        temp.push_back(' ');
                    }
                    op_st.push(expression[i]);
                    eq += temp;

                    temp.clear();
                }
            }
        }
        
        eq += temp;
        eq += " ";
        temp.clear();

        while (!op_st.empty())
        {
            char op = op_st.top();
            op_st.pop();
            temp.push_back(op);
            temp.push_back(' ');
        }

        eq += temp;

        long long result = 0;

        size_t pos;

        stack<long long> num_st;

        // 공백단위로 끊는다.
        while ((pos = eq.find(" ")) != string::npos)
        {
            string token = eq.substr(0, pos);

            // 숫자라면 스택에 넣는다.
            if (isdigit(token[0]))
                num_st.push(stoi(token));
            else
            {
                long long a = num_st.top();
                num_st.pop();

                long long b = num_st.top();
                num_st.pop();

                switch (token[0])
                {
                case '+':
                    num_st.push(b + a);
                    break;
                case '-':
                    num_st.push(b - a);
                    break;
                case '*':
                    num_st.push(b * a);
                    break;
                }
            }

            eq.erase(0, pos + 1);
        }


        result = abs(num_st.top());

        answer = max(answer, result);

    } while (next_permutation(ops.begin(), ops.end()));



    return answer;
}
```
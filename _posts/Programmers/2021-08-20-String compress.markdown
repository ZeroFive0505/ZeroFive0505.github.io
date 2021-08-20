---
layout: post
title:  문자열 압축
date:  2021-08-20 08:42:57 +0900
categories: Programmers-Level2
---

# 문자열 압축
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/60057)

# 풀이
반복문은 총 1번부터 최대 길이의 절반까지만 반복한다. 최대로 압축을 할 수 있다는 것은 똑같은 문자열이 2번 반복된다는 것이고 따라서 절반까지만 반복하면 된다. 1번부터 하는 이유는 토큰을 만들기위해서 이다. 먼저 처음 문자열에서 1씩 부분 문자열을 만들어 토큰들을 만들어 그걸로 비교를 하고 다음에는 2씩 부분 문자열을 만들고 최종적으로는 길이의 절반만큼 부분 문자열을 만들 것이다. 만든 부분 문자열이 전 문자열과 매칭이 되면 카운트를 늘려주고 1이 아니었다면 그 숫자를 문자열로 바꿔서 문자열에 더해준다.

# 코드
```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

using namespace std;

// n만큼의 부분 문자열을 만들어준다.
vector<string> Convert(const string& s, int n)
{
    vector<string> v;

    for (int i = 0; i < s.size(); i += n)
    {
        v.push_back(s.substr(i, n));
    }

    return v;
}

int solution(string s) 
{

    // 정답은 초기에는 문자열의 길이로 잡아준다.
    // 만약 문자열이 길이가 1인 문자열이 들어올 시에 반복문 자체를 건너뛰게된다.
    int answer = s.size();

    for (int i = 1; i <= s.size() / 2; i++)
    {   
        // 초기 카운트는 1 자기 자신을 포함하기에
        int cnt = 1;
        vector<string> tokens = Convert(s, i);
        // 가장 첫 토큰은 비교대상으로 쓴다.
        string before = tokens[0];
        string compressed;
        // 토큰의 길이만큼 반복한다. 1부터 시작한다. 첫 토큰은 위에 비교대상으로 썼기에
        for (int j = 1; j < tokens.size(); j++)
        {
            // 만약 비교 대상 토큰과 비교후 같다면 카운트를 늘려준다.
            if (before == tokens[j])
                cnt++;
            else
            {
                // 카운트가 1이 아니라면 압축 문자열에 숫자로 바꾼후 넣어준다.
                if (cnt != 1)
                    compressed += to_string(cnt);
                
                // 그리고 비교대상이었던 문자열을 넣고
                compressed += before;
                // 새롭게 갱신한다.
                before = tokens[j];
                cnt = 1;
            }
        }

        if (cnt != 1)
            compressed += to_string(cnt);
        compressed += before;

        // 최소값을 취한다.
        answer = min(answer, (int)compressed.size());
    }

    return answer;
}
```

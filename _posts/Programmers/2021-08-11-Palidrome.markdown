---
layout: post
title: 가장 긴 팰린드롬
date: 2021-08-11 08:58:48 +0900
categories: Programmers-Level3
---

# 가장 긴 팰린드롬
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12904)

# 풀이
첫 시도는 벡터를 이용해서 가능한 모든 부분 문자열을 만들고 나서 그게 회문인지를 확인하는 방식으로 했는데 효율성에서 시간이 초과가 나와서 다른 사람들의 풀이를 참조하여 풀 수 있었다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;


int solution(string s)
{
    int answer = 1;
    int size = s.size();

    while (size >= 2)
    {
        // 처음에는 전체 문자열 그 다음에는 길이가 1나 줄어든 부분문자열을 확인하고 한칸 
        // 옮겨서 확인한다.
        for (int i = 0; i <= s.size() - size; i++)
        {
            int left = i;
            int right = size + i - 1;
            bool palindrome = true;
            while (left <= right)
            {
                if (s[left] != s[right])
                {
                    palindrome = false;
                    break;
                }
                left++;
                right--;
            }

            if (palindrome)
                return size;
        }
        size--;
    }

    return answer;
}
```
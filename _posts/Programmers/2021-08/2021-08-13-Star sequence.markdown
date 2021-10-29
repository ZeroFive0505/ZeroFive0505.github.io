---
layout: post
title: 스타 수열
date:  2021-08-13 09:20:00 +0900
categories: Programmers-Level3
---

# 스타 수열
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/70130)


# 풀이
정확한 풀이는 [이 곳](https://yabmoons.tistory.com/610)에서 확인 할 수 있다. 

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> a) 
{
    int answer = -1;

    vector<int> cnt(a.size(), 0);

    // 먼저 나온 숫자들의 등장 횟수를 기록한다.
    for (int i = 0; i < a.size(); i++)
        cnt[a[i]]++;

    for (int i = 0; i < cnt.size(); i++)
    {
        // 만약 한번도 나오지 않은 숫자라면 건너뛴다.
        if (cnt[i] == 0)
            continue;
        // 또는 지금 현재 나온 답보다 적은 등장횟수라면 건너 뛴다.
        if (cnt[i] <= answer)
            continue;

        int result = 0;

        for (int j = 0; j < a.size() - 1; j++)
        {
            // 현재 i번째 수로 수열을 만드는 과정에서
            // 만약 j번째 또는 j+1번째 수가 i랑 같지 않다면 스타 수열은 만들 수 없다. 
            if (a[j] != i && a[j + 1] != i)
                continue;

            // 또는 j번째의 수가 j + 1번째 수와 같다면 수열은 만들 수 없다.
            if (a[j] == a[j + 1])
                continue;

            // 만약 위 두 조건이 성립하지 않았다면
            // 결과 값을 늘려주고
            // j값을 늘려준다 결과적으로 j는 2개 건너뛸것이다.
            result++;
            j++;
        }

        // 최댓값을 취한다.
        answer = max(result, answer);
    }


    // 현재 answer 공통된 원소가 사용된 횟수를 의미한다 따라서 2를 곱해서 최종적인 길이를 반환한다.
    return answer * 2;
}
```
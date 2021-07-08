---
layout: post
title: 선입선출 스케쥴링
date: 2021-07-08 08:31:38 +0900
categories: Programmers-Level4
---

# 선입 선출 스케쥴링
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/12920)

# 첫 시도
덱을 이용해서 가장 속도가 빠른 코어를 앞에 넣고 시간을 확인해서 빼고 다시 넣는 식으로 했는데 예제는 통과했지만, 답은 아니었다.

# 코드
```C++
#include <iostream>
#include <queue>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<int> cores) 
{
    int answer = 0;
    int time = 0;
    vector<pair<int, int>> v;
    deque<pair<int, int>> workingQ;
    deque<pair<int, int>> watingQ;
    
    if (cores.size() >= n)
    {
        answer = n % cores.size();
        if (answer == 0)
            return cores.size();
        else
            return answer;
    }


    n -= cores.size();
    int maxDuration = *max_element(cores.begin(), cores.end()) + 1;

    for (int i = 0; i < cores.size(); i++)
    {
        v.push_back({ i + 1, cores[i] });
    }

    sort(v.begin(), v.end(), [](const pair<int, int>& a, const pair<int, int>& b) {
        return a.second < b.second;
    });

    for (int i = 0; i < v.size(); i++)
    {
        workingQ.push_back(v[i]);
    }

    while (n)
    {
        time++;
        time %= maxDuration;

        while (!workingQ.empty() && workingQ.front().second <= time)
        {
            pair<int, int> f = workingQ.front();
            workingQ.pop_front();
            n--;
            watingQ.push_back(f);

            if (n == 0)
            {
                answer = f.first;
                break;
            }
        }

        while (!watingQ.empty())
        {
            workingQ.push_front(watingQ.front());
            watingQ.pop_front();
        }
    }

    return answer;
}
```

# 풀이
인터넷에서 풀이를 참조한 결과 이진탐색을 이용한 Parametric search 문제였다.
최소 시간과 최대 시간의 중간 값을 찾아 코어의 작업 시간으로 나누고 만약 0이랑 같다면 바로 지금 새로 작업을 할당 받은 코어가 된다. 만약 처리한 작업의 양이 n보다 크다면 상한선을 낮추고 만약 더 낮다면 상한선을 높이면서 같아지는 지점을 찾는다.

# 코드
```C++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

const int INF = 987654321;

int solution(int n, vector<int> cores) 
{
    int answer = 0;
    
    if(n <= cores.size())
        return n;

    int m = *min_element(cores.begin(), cores.end());

    int left = n / cores.size() * m;
    int right = n * m;

    while (left <= right)
    {
        int mid = (left + right) / 2;
        int cnt = cores.size();

        int currentCnt = 0;

        for (int i = 0; i < cores.size(); i++)
        {
            cnt += mid / cores[i];
            if (mid % cores[i] == 0)
            {
                cnt--;
                currentCnt++;
            }
        }

        if (cnt >= n)
        {
            right = mid - 1;
        }
        else if ((cnt + currentCnt) < n)
            left = mid + 1;
        else
        {
            for (int i = 0; i < cores.size(); i++)
            {
                if (mid % cores[i] == 0)
                    cnt++;

                if (cnt == n)
                    return i + 1;
            }
        }
    }


    return answer;
}
```
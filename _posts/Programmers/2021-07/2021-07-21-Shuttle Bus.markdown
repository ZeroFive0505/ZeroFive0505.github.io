---
layout: post
title: 셔틀 버스
date: 2021-07-21 08:38:52 +0900
categories: Programmers-Level3
---

# 셔틀 버스
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/17678)

# 풀이
문자열로 이루어진 테이블에서 시간을 초 분 단위로 바꿔서 그것을 벡터에 넣는다. 그리고 정렬를 한 이후에 버스 운행 횟수 만큼 반복문을 돌면서 만약 도착 시간이 버스의 운행시간보다 크거나 같다면 탈 수 있으니 그 결과를 반영해주고 만약 이 버스가 마지막이고 이제 아무도 못 탄다면 마지막에 탄 손님보다 무조건 1분 빠르게 와야한다는 뜻이다. 아니라면 버스의 운행 정각 시간에 도착해도 된다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

string solution(int n, int t, int m, vector<string> timetable) 
{
    string answer;

    vector<int> times;

    for (int i = 0; i < timetable.size(); i++)
    {
        string hour = timetable[i].substr(0, 2);
        string minutes = timetable[i].substr(3, 2);

        int total = stoi(hour) * 60 + stoi(minutes);

        times.push_back(total);
    }


    sort(times.begin(), times.end());

    // 버스는 오전 9시부터 운행을 시작.
    int start = 540;

    int idx = 0;

    int busTime = 0;

    for (int i = 0; i < n; i++)
    {
        int capacity = m;

        for (int j = idx; j < times.size(); j++)
        {
            if (times[j] <= start)
            {
                capacity--;
                idx++;

                if (capacity == 0)
                    break;
            }
        }

        // 만약 마지막 버스라면
        if (i == n - 1)
        {   
            // 더이상 아무도 탈 수 없을때는 마지막 손님보다 1분 빠르게
            if (capacity == 0)
                busTime = times[idx - 1] - 1;
            else
                busTime = start;
        }

        start += t;

        if (start >= (60 * 24))
            break;
    }

    
    int hour = busTime / 60;

    if (hour < 10)
        answer += "0" + to_string(hour) + ":";
    else
        answer += to_string(hour) + ":";

    int minutes = busTime % 60;

    if (minutes < 10)
        answer += "0" + to_string(minutes);
    else
        answer += to_string(minutes);

    return answer;
}

```
---
layout: post
title:  오픈 채팅방
date:  2021-08-20 09:02:20 +0900
categories: Programmers-Level2
---

# 오픈 채팅방
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42888)

# 풀이
원래는 unordered_map을 이용해서 로그를 순서대로 기록을 하려했는데 IDE에서는 원하는 결과가 잘 나왔지만 이상하게도 프로그래머스에서는 잘못된 결과를 출력을 했다. 결국에는 map을 또 따로 만들어서 로그 순서대로 기록을 했다.

# 코드
```c++
#include <iostream>
#include <unordered_map>
#include <map>
#include <string>
#include <vector>

using namespace std;

vector<string> solution(vector<string> record) 
{
    vector<string> answer;
    int size = record.size();
    map<int, string> logHash;
    //unordered_map<string, string> logHash;
    map<string, vector<int>> m;
    map<string, string> nameHash;
    int logNumber = 1;
    for (int i = 0; i < size; i++)
    {
        // 메세지와 UID 그리고 이름을 나눈다.
        string s = record[i];
        size_t pos;
        pos = s.find(" ");
        string msg = s.substr(0, s.find(" "));
        s.erase(0, pos + 1);

        pos = s.find(" ");
        string uid = s.substr(0, s.find(" "));
        s.erase(0, pos + 1);

        string name = s.substr(0, s.find(" "));

        // 메세지가 입장일 경우
        if (msg == "Enter")
        {
            // 만약 한번도 등장하지않은 UID일 경우
            // 해당 로그 번호에 그대로 기록한다.
            if (m.count(uid) == 0)
                logHash[logNumber] = name + "님이 들어왔습니다.";
            else
            {
                // 만약 uid에 기록된 이름과 현재 이름이 다르다면 닉네임을 바꾼거다.
                if (name != nameHash[uid])
                {
                    // 해당 UID의 모든 로그들을 가져온다.
                    vector<int> logs = m[uid];
                    for (int i = 0; i < logs.size(); i++)
                    {
                        // 로그들을 순회하면서 메세지 부분과 이름 부분을 가져온다.
                        string t = logHash[logs[i]];
                        string token = t.substr(nameHash[uid].length(), t.length());

                        // 메세지부분만 따로 떼고 이름과 붙인다.
                        t = name + token;

                        // 그리고 그걸 그대로 해당 로그 번호에 다시 기록한다.
                        logHash[logs[i]] = t;
                    }
                }

                // 그리고 마지막으로 해당 유저의 입장을 기록
                logHash[logNumber] = name + "님이 들어왔습니다.";
            }

            // UID와 이름을 기록
            nameHash[uid] = name;
            // 해당 UID의 로그 번호를 모은다.
            m[uid].push_back(logNumber);
        }
        // 채팅방을 떠날시
        else if (msg == "Leave")
        {
            logHash[logNumber] = nameHash[uid] + "님이 나갔습니다.";

            // 로그를 기록한다.
            m[uid].push_back(logNumber);
        }
        else
        {   

            // 닉네임을 바꿀경우 닉네임을 바꾸고 입장하는경우와 동일하다.
            vector<int> logs = m[uid];
            for (int i = 0; i < logs.size(); i++)
            {
                string t = logHash[logs[i]];
                string token = t.substr(nameHash[uid].length(), t.length());

                t = name + token;

                logHash[logs[i]] = t;
            }

            // 채팅방안에서 닉네임을 바꿀경우 메세지를 띄울 필요는 없다. 따라서 간단하게 닉네임만 바꿔준다.
            nameHash[uid] = name;
        }

        // 로그 번호를 순서대로 늘려간다.
        logNumber++;
    }

    map<int, string>::const_iterator it;
    // 로그들을 순회하면서 메세지들을 넣는다.
    for (it = logHash.begin(); it != logHash.end(); it++)
    {
        answer.push_back((*it).second);
    }

    return answer;
}
```
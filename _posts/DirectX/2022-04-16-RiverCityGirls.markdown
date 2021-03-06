---
layout: post
title: 리버 시티 걸즈 개발 일지
date: 2022-04-16 09:01:32 +0900
categories: DirectX
---

# DirectX로 River City Girls 모작하기
DirectX로는 어떤 게임을 모작해볼까 고민해보다가 벨트스크롤, Beat'em Up 장르의 게임을 좋아했기에 최근에 플레이 해본 River City Girls를 모작해보기로 결정했다. 

아래는 그 개발의 과정과 영상이다.

# 기본적인 플레이어 움직임 만들기

[![기본적인 플레이어 움직임](https://imgur.com/5kqgACI.png)](https://www.youtube.com/watch?v=OOGDMbHtU9Q "움직임")

플레이어 바닥의 충돌체를 만들어서 벨트스크롤 장르의 x, y, z축을 만들었다. 플레이어가 땅바닥에서 움직일경우 x, z축 움직임 점프를 할 경우는 y축

점프의 경우는 간단하게 위로 플레이어의 몸만 가속도를 줘서 날리고 바닥 충돌체는 그 위치를 그대로 유지하게 만들어서 점프를 하더라도 원하는 위치에 착지가 가능하게 그리고 항상 똑같은 거리만큼을 유지하게 만들 수 있었다.

# 히트박스 테스트와 플레이어 좌우반전

[![히트박스 테스트, 스프라이트 좌우 반전](https://img.youtube.com/vi/SePe0i6TOWo/0.jpg)](https://www.youtube.com/watch?v=SePe0i6TOWo "히트박스")

기본적인 움직임이 끝이나서 플레이어와 적과의 충돌처리를 만들었다. 이때는 위에서 언급한 3가지 축이 일치할 경우에만 히트박스 생성시에 히트가 됬다고 판단했다. 예를 들어 내 바닥이 적의 바닥과 충돌하고 몸이 충돌했을 경우 히트박스는 유요한 히트박스가 된다. 아직 좌우반전 처리는 불안정했다.

# 넉백과 연쇄 콤보

[![넉백, 연쇄 콤보](https://img.youtube.com/vi/cckQwv7hYww/0.jpg)](https://www.youtube.com/watch?v=cckQwv7hYww "히트박스")

원작에서는 경우 약공격의 경우 적에게 히트했을시에 바로 연계가 가능하고 히트가 되지 않으면 다음 단계로 넘어갈 수가 없는 없다. 또한 몇몇 공격은 특정 방향으로 적을 날릴 수 있도록 만들었다.

# 스테이지 제작

[![타일, 스테이지 제작](https://img.youtube.com/vi/mCthuuKL6Io/0.jpg)](https://www.youtube.com/watch?v=mCthuuKL6Io "스테이지")

원작의 경우 스테이지가 어떤식으로 되어있나 툴로 분석해서 리소스를 찾아보니 하나의 큰 배경으로 이루어져 있었다. 몇몇 스테이지는 사용하고 싶어도 제목은 BG라고 되어있지만 앞에 가장 앞에 출력이 되어야할 기둥, 나무등이 그냥 뒷배경에 같이 붙어있어서 원작 게임의 초반 스테이지중에 뒷배경만 깔끔하게 되어있는 스테이지를 특정 크기의 타일로 나눠 플레이어의 행동을 제한했다.

# 테스트 시작

[![플레이 테스트](https://imgur.com/Jyunmu5.png)](https://www.youtube.com/watch?v=u5rWFtRQ6oc
 "테스트")

 기본적인 움직임과 공격 그리고 스테이지가 완성이 되서 테스트를 시작했다. 잡기와 다운된 적을 공격하는 기능 그리고 적의 넉백의 경우 이전과 달리 가속도 속도를 만들어서 속도의 변화를 위치의 변화로 해서 더욱 자연스럽게 바뀐 모습을 볼 수 있다. 또한 강한 충격으로 날아갈 경우 벽과 충돌시에 반대 방향으로 또 튕겨나오게 했다.

# 추적 AI

[![추적 AI](https://imgur.com/JpA8I6E.png)](https://www.youtube.com/watch?v=ZdhAXtPvfYo
 "추적")

 이제 플레이와 적과의 상호작용은 어느정도 완성이 되어서 내가 적을 때리는 상황뿐만 아니라 적이 나를 인지하고 공격하는 상황을 만들어야 했다. 따라서 일단 간단하게 특정 시간이 지나면 내 위치로 오도록 만들었다.

 # 공격 AI

 [![공격 AI](https://imgur.com/y1elG2t.png)](https://www.youtube.com/watch?v=_27L9zqgaLQ
 "공격")

 추적이 완성된 이후에는 공격을 만들었다. 축이 일치할 경우 바로 공격상태로 전환을 하게 만들었는데 적이 너무 공격을 잘해서 확률을 설정해야 할 것 같다.

 # 적 추가와 스테이지 전환

  [![적 추가 1](https://imgur.com/SArWCLx.png)](https://www.youtube.com/watch?v=c_R6UgyieXU
 "적 추가 1")


[![스테이지 전환](https://imgur.com/MtTRGNT.png)](https://www.youtube.com/watch?v=Gmg6Z22MR_M
 "스테이지 전환")

 만들어둔 적 클래스를 기반으로 비교적 간단하게 새로운 적을 추가 할 수 있었다. 다만 적마다 공격 패턴이 다르고 이 공격패턴마다 행동하는게 달라서 이 부분만 좀 신경을 써야는데 이 부분이 생각 보다 까다로웠다. 특히 공중 패턴의 경우 신경써야할 부분이 많았다. 애니메이션 연계도 그렇고.. AI는 기본적으로 Idle, Pursue, Attack, Fallback, Block 상태(State)로 구현했다.

# 보스

 [![스테이지 전환, 보스](https://imgur.com/tKIf1wl.png)](https://www.youtube.com/watch?v=oqou9V0ZJss
 "스테이지 전환, 보스")

 최종적으로 UI와 사운드 그리고 스테이지 전환과 보스전을 테스트 해봤다. 확실히 사운드가 추가가되니 타격감이 많이 살아난 느낌이다. 보스전의 경우 기둥의 충돌 활성화가 내 바닥이 기둥바닥에 충돌됬을 경우만 활성화되어 활성화 되기 전까지는 충돌체가 있어도 통과가 가능하다.
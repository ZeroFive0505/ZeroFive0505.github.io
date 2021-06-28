---
layout: post
title: Unity Command pattern
date: 2021-06-28 14:04:41 +0900
catergories: Unity DesignPattern
---

# Unity 커맨드 패턴

# 게임에 쓰이는 커맨드 패턴
예를들어 패드로 게임을 한다고 생각해보자 유저는 패드 각 버튼의 입력을 설정에 가서 바꿀 수 있다. 이때 커맨드 패턴을 이용하면 간단하게 각 키에 새로운 액션을 할당 할 수 있다. 

![https://gameprogrammingpatterns.com/command.html](https://gameprogrammingpatterns.com/images/command-buttons-one.png)

# 과정
먼저 커맨드 클래스를 가상화로 구현한다.
```C#
public abstract class Command
{
    public abstract void Execute(Animator anim);
}
```
그리고 이를 상속받아서 실제로 해당 애니메이션을 실행할 클래스들을 만든다.

```C#
public class Punch : Command
{
    public override void Execute(Animator anim)
    {
        anim.SetTrigger("isPunching");
    }
}

public class Kick : Command
{
    public override void Execute(Animator anim)
    {
        anim.SetTrigger("isKicking");
    }
}
....
```
이 다음 유저의 입력을 관리할 클래스를 만들고 거기에 Command 형식의 변수를 선언해 자유롭게 커맨드 클래스를 상속받는 클래스를 할당 받을 수 있게 한다.

```C#
Command keyP, keyK;
Command combo;
List<Command> commands = new List<Command>();
Dictionary<Command, int> moveSet = new Dictionary<Command, int>();

keyP = new Punch();
keyK = new Kick();
combo = new Combo();
....
```
만약 펀치와 킥을 바꾸고 싶다면 간단하게 keyK에 펀치를 할당하고 keyP에 킥을 할당하면 된다. 또한 커맨드 형태의 리스트를 준비해 모든 커맨드들을 기록한다. 이를 이용해 격투게임에서 사용자가 최근에 입력한 입력 커맨드를 볼 수 있을뿐만 아니라 특별한 순서대로 일정 시간내로 입력되면 발동되는 특수 무브셋도 간단하게 구현 할 수 있었다.

# 결과

![Result](https://user-images.githubusercontent.com/39051679/122658507-17912b00-d1a9-11eb-9200-00d8a5e9c918.gif)

![Result](https://user-images.githubusercontent.com/39051679/122658508-1d870c00-d1a9-11eb-882f-cf0f42a8463f.gif)

펀치 - 킥 - 펀치순으로 일정시간내에 입력하면 위와같이 콤보가 나간다.. 콤보는 구현을 좀 어눌하게 해서 잔버그가 있었다.. 전체 코드는 여기서 확인해 볼 수 있다. [링크](https://github.com/ZeroFive0505/Design_pattern_exercise)
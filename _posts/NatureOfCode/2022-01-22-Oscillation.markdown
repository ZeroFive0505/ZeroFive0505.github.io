---
layout: post
title: Oscillation
date: 2022-01-22 10:59:49 +0900
categories: DirectX
---

# Oscillation
> 삼각함수를 화면에 시각화를 해봤다.

![Imgur](https://imgur.com/184BEfN.gif)
> 코사인 함수

![Imgur](https://imgur.com/HAn88aL.gif)
> 삼각함수 합성 1 

![Imgur](https://imgur.com/RMXZwck.gif)
> 삼각함수 합성 2

```c++
	// 현재 시간을 구한다.
    m_ElapsedTime += deltaTime;

	size_t size = m_vecMover.size();
	float startAngle = m_ElapsedTime;
	for (size_t i = 0; i < size; i++)
	{	
		float y = m_vecMover[i]->GetAmplitude().y * cosf(startAngle);
		// y += m_vecMover[i]->GetAmplitude().x / 5.0f * sinf(startAngle * 2.0f);
		Vector2 pos = m_vecMover[i]->GetCenterPos();

		// y 값에 오프셋을 준다
		m_vecMover[i]->SetWorldPos(pos.x, pos.y + y, 0.0f);

		// 다음 객체는 지금보다 살짝 이후의 값
		startAngle += 0.1f;
	}
```

# 시계 추
> 각 가속도 = 중력 * sin(각도)로 간단하게 시계 추를 만들 수 있었다. 여기서 추의 길이로 나눠주면 추의 길이가 길면 길수록 왕복까지의 시간이 커지고 길이가 짧으면 짧을수록 왕복까지의 시간은 급격하게 줄어든다.

![Imgur](https://imgur.com/w6DMDM0.gif)


![Imgur](https://imgur.com/Hzlfl7b.gif)

> 길이가 다른 시계추 100개


![Imgur](https://imgur.com/hp9S2dn.gif)

> 길이는 같지만 시작각도가 다른 시계추 50개

```c++
...
		// 각 가속도는 중력(상수) / 추의 길이 * sin(각도)
		m_AngularAcceleration = (m_Gravity / m_ArmLength) * sinf(m_Angle);

		m_AngularVelocity += m_AngularAcceleration;

		m_Angle += m_AngularVelocity;
		
		// 각 속도를 매순간마다 살짝 감소하게 만든다.
		m_AngularVelocity *= m_Damping;

...
```
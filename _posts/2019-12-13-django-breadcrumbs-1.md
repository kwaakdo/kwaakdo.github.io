---
title: Django에서 Breadcrumbs 직접 구현하기
tags: Django Python Website Algorithm
---
![image](https://user-images.githubusercontent.com/56034782/70805512-69ab0080-1dfc-11ea-8958-13629c345292.png) 좌측 메뉴와 상단의 Breadcrumbs가 일치하는 것을 볼 수 있다.
![image](https://user-images.githubusercontent.com/56034782/70805153-a0344b80-1dfb-11ea-902e-2c6a04b88068.png)
이 포스트에서는 Django View에서 어떤방식으로 Breadcrumbs를 구현하는지 다루겠다.
<!--more-->
# Breadcrumbs 란?
브래드크럼은 핸젤과 그레텔에서 따온 용어(빵부스러기)로 사용자가 어떻게 이곳에 위치하게 되었는지에 대한 정보를 나타낸다. 
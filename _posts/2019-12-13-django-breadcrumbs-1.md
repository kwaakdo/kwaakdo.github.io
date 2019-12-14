---
title: Django에서 Breadcrumbs 직접 구현하기
tags: Django Python Website Algorithm
---
![image](https://www.highervisibility.com/wp-content/uploads/2019/06/target-breadcrumbs.png)

이번 포스트에서는 위와 같은 Breadcrumbs를 구현하는 방법에 대해 다루겠다.
<!--more-->
# Breadcrumbs 란?
브래드크럼은 핸젤과 그레텔에서 따온 용어(빵부스러기)이다. 겉보기에는 간단해보이지만 알고리즘은 생각보다는 복잡하다. 내가 직접 구현했었던 코드는 제일 하단에 있다.

## 어떻게 구현하는가?
### 가정
:![image](https://user-images.githubusercontent.com/56034782/70847195-ed202c80-1ea4-11ea-99eb-749d3212f027.png)
이해를 돕기 위해 좌측과 같은 구조의 Document들이 있다고 가정하겠다. 게시글6이 몇번째에 위치해 있는지 Checkpoint에 저장한 것이다. Depth는 게시글의 깊이를 표현한다. 블로그를 예로 들자면 메뉴안의 메뉴 같은 개념이다.

### 변수 선언
1. locations = [] 으로 list형 변수를 선언해줘야 한다. breadcrumbs를 기록하는 역할을 할 것이다. 
2. point_depth = document.depth 으로 현재 접속한 게시글의 depth를 변수에 저장해줘야한다. 

### 코드 작성
```python
for i in reversed(range(0, checkpoint+1)):
        if documents[i].depth < point_depth:
            locations.append(documents[i])
            point_depth -= 1
        if point_depth == 0:
            break
```
위 코드를 통해서 점점 위쪽으로 탐색하는 작업을 반복한다. point_depth가 0이 되면 종료되는 반복문이다. 참고로 i 는 point_depth를 비교할 대상이 되는 document번호라고 생각하면 된다. 

### For문 첫번째 루프
![image](https://user-images.githubusercontent.com/56034782/70847308-86037780-1ea6-11ea-917f-3043531bf601.png)

### For문 두번째 루프
![image](https://user-images.githubusercontent.com/56034782/70847319-b0553500-1ea6-11ea-9344-88e6e16a4fe7.png)


i의 값이 줄어들면서 점점 위쪽 게시글로 올라가는 모습이다.

### For문 세번째 루프
![image](https://user-images.githubusercontent.com/56034782/70847329-d084f400-1ea6-11ea-808a-bfb26831f2d2.png)

### For문 네번째 루프
![image](https://user-images.githubusercontent.com/56034782/70847337-e7c3e180-1ea6-11ea-8c00-335266fed7f4.png)

### 코드 종료
이런 식으로 위로 올라가며 계속 비교해가는 방식이다. Depth가 비교되고 참일경우에 계속 1씩 줄어드는데, 만약 상위 게시글과 Depth가 2정도 차이나면 location에 의도치않은 breadcrumbs가 들어가게 된다. Depth의 차이를 계산하는 과정을 추가하면 된다.

## 실제 코드
<iframe
  src="https://carbon.now.sh/embed?bg=rgba(171%2C%20184%2C%20195%2C%201)&t=seti&wt=none&l=python&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=56px&ph=56px&ln=false&fl=1&fm=Hack&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=def%2520document(request%252C%2520id)%253A%250A%2520%2520%2520%2520id_integer%2520%253D%2520int(id)%250A%2520%2520%2520%2520document%2520%253D%2520Document.objects.get(document_id%2520%253D%2520id_integer)%250A%2520%2520%2520%2520book_id_integer%2520%253D%2520int(Document.objects.get(document_id%2520%253D%2520id_integer).book.book_id)%250A%2520%2520%2520%2520book_detail%2520%253D%2520Book.objects.get(book_id%2520%253D%2520book_id_integer)%250A%2520%2520%2520%2520documents%2520%253D%2520book_detail.document_set.all()%250A%250A%2520%2520%2520%2520for%2520i%2520in%2520range(0%252C%2520documents.count())%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520if%2520document.document_id%2520%253D%253D%2520documents%255Bi%255D.document_id%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520checkpoint%2520%253D%2520i%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520break%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520locations%2520%253D%2520%255B%255D%250A%2520%2520%2520%2520point_depth%2520%253D%2520document.depth%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520for%2520i%2520in%2520reversed(range(0%252C%2520checkpoint%252B1))%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520if%2520documents%255Bi%255D.depth%2520%253C%2520point_depth%253A%250A%2509%2509%2509locations.append(documents%255Bi%255D)%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520point_depth%2520-%253D%25201%250A%2520%2520%2520%2520%2520%2520%2520%2520if%2520point_depth%2520%253D%253D%25200%253A%250A%2509%2509break%250A%250A%250A%2520%2520%2520%2520return%2520render(request%252C%2520'post%252Fdocument.html'%252C%2520%257B'document'%253A%2520document%252C'book_detail'%253A%2520book_detail%252C'documents'%253A%2520documents%252C'locations'%2520%253A%2520reversed(locations)%252C%257D)"
  style="transform:scale(0.7); width:100%; height:473px; border:0; overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>


![image](https://user-images.githubusercontent.com/56034782/70847410-789abd00-1ea7-11ea-88b1-4996117fd42f.png)


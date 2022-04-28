# Ford-Fulkerson Algorithm

## 네트워크 유량(Network Flow)

그래프의 경로의 길이가 아닌, '용량'의 관점으로 바라본다.

- Source(시작점) -> Sink(도착점)으로 *동시에 보낼 수 있는, 데이터나 사물의 최대 양*을 구하는 알고리즘

- 기본개념
용량과 유량으로 구분 짓습니다.

용량은 말그대로 유량이 흐를 수 있는 최대 크기입니다.
유량은 용량의 한계내에서 방향성으로 가지고 흐르는 크기입니다.

예를들어, u에서 v로의 용량이 10이고, v에서 u로의 용량이 5인 경우, 다음과 같이 나타낼 수 있습니다. 

![Capacity](https://postfiles.pstatic.net/MjAxOTAyMjVfMzIg/MDAxNTUxMDg0NTUyNjg3.tb9TlZklxw-kScWjUItHFg3Yt1w7S0D0P-gKHPTX16Yg.SRMP_8MH1uAmFFgN_Ggw2C-SdPxch0JDE77WvXxcEp8g.PNG.na_qa/SE-3aba914c-cda3-4daa-8cf5-18caff5f74f9.png?type=w773)

그리고 u에서 v로의 유량이 5이고, v에서 u로의 용량이 3인 경우, 다음과 같이 나타낼 수 있습니다.

![Capacity](https://postfiles.pstatic.net/MjAxOTAyMjVfMjA3/MDAxNTUxMDg0NTkwNzM3.ns59cWgfqam7lGx-EbsbQd10HryVL2invSpj7sPUVXEg.y96RkVIhn2nw-lzHEA7kGglPiNvUVkULOS5DKbjwuxUg.PNG.na_qa/SE-f8762aa8-495d-45bb-89ba-c52813e24851.png?type=w773)

그리고 순 유량은 5-3, 즉 u에서 v로 2이므로 다음과 같이 나타낼 수 있습니다.

![Capacity](https://postfiles.pstatic.net/MjAxOTAyMjVfMjM0/MDAxNTUxMDg0NjgzODA3.oYFBa7XLEZqXhwDyYZCqOp8Du-u4U9CE_iRSn2bMH70g.nbxfOqJCp4wU1E_mCoMe13oQj7EGZhIh7bnfc5BBUmQg.PNG.na_qa/SE-b244df7a-55a3-4831-9e82-02361bab0b0f.png?type=w773)

---
 Source: 시작점  
 Sink: 도착점  
 Capacity: 용량 (간선에서 소화 가능한 최대 양 or 값)   
 Flow: 유량 (간선에서 용량을 점유하고 있는, 사용하고있는 양 or 값)     
 c(a, b): 정점 a 에서 b로, 소화 가능한(남은) 용량 값   
 f(a, b): 정점 a 에서 b로, 사용하고 있는(쓴) 유량 값  
 
 --- 
 
 유량 네트워크가 만족해야 하는 세가지 속성
 - 용량 제한 속성: f(u,v) <= c(u,v)
 - 유량의 대칭성: f(u,v) = -f(u,v) -> u에서 v로 유량이 흐르면 v에서 u로 음수의 유량이 흐르는 것과 동일하다.
 - 유량의 보존: 각 정점에 들어오는 유량과 나가는 유량은 같다.

여기서 다루는 네트워크들은 유향 그래프 G = (V, E)로 구성됩니다. 그래프 G는 source와 sink인 두 개의 특별한 노드 s,t∈ Vs, t∈V를 갖고 용량이 c_e >0인 간선들을 갖습니다.



## 알고리즘 설명

네트워크의 유량의 가장 기본적인 알고리즘은, 포드 풀커슨(Ford - Fulkerson) 알고리즘과, 에드몬트카프(Edmonds - Karp) 알고리즘이 있다.

1. 소스노드에서 시작해서 싱크 노드로 도착하는 유량 
-유량 네트워크의 모든 간선의 유량을 0으로 초기화한다.
-소스에서 싱크로 유량을 더 보낼 수 있는 경로를 찾아 유량 보내기를 반복한다.

포드-풀커슨 알고리즘은 DFS 방식으로 증가 경로를 찾고,

에드몬드-카프 알고리즘은 BFS 방식으로 증가 경로를 찾습니다.

하지만, DFS 를 통해 경로를 탐색하는 방법은, 비효율적으로 동작할 수 있는 경우가 있습니다.

그림을 통해 이해해보겠습니다.

![Algorithm](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb1wkEf%2Fbtrn1zzImKX%2FkFqrs5B8VOhgCnV4kjMKI1%2Fimg.png)

위 그림에서, DFS( 를 이용한 증가 경로 탐색은 아래와 같습니다.
- A→B→C→D (1의 유량 보냄)
- A→C→B→D (역간선을 이용해 1의 유량 보냄)
- A→B→C→D (1의 유량 보냄)
- A→C→B→D (역간선을 이용해 1의 유량 보냄)
- 반복
BFS 를 이용한 증가 경로 탐색 결과는 아래와 같습니다.
- A→B→D (1000의 유량 보냄)
- A→C→D (1000의 유량 보냄)
- 즉, DFS 는 2000 번의 탐색으로 최대 유량을 찾아냈지만,
- BFS 는 2번의 탐색으로 최대 유량을 찾아냈습니다.
- 따라서, 애드몬드-카프 알고리즘이 일반적으로 더 효율적으로 동작하는 것을 알 수 있습니다.


## 구현
잔여 용량이 없을 때까지, 즉 보낼 수 있는 유량이 없을 때까지 BFS 또는 DFS 반복


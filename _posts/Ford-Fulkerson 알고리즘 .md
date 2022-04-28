# Ford-Fulkerson Algorithm

## 네트워크 유량(Network Flow)

그래프의 경로의 길이가 아닌, '용량'의 관점으로 바라본다.

- Source(시작점) -> Sink(도착점)으로 *동시에 보낼 수 있는, 데이터나 사물의 최대 양*을 구하는 알고리즘

**기본개념**  
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
 r(a, b): c(a,b) - f(a,b) : 정점 a에서 정점 b로 보낼 수 있는 잔여 용량 
 
 --- 
 
 유량 네트워크가 만족해야 하는 세가지 속성
 - 용량 제한 속성: f(u,v) <= c(u,v)
 - 유량의 대칭성: f(u,v) = -f(u,v) -> u에서 v로 유량이 흐르면 v에서 u로 음수의 유량이 흐르는 것과 동일하다.
 - 유량의 보존: 각 정점에 들어오는 유량과 나가는 유량은 같다.

여기서 다루는 네트워크들은 유향 그래프 G = (V, E)로 구성됩니다. 그래프 G는 source와 sink인 두 개의 특별한 노드 s,t∈ Vs, t∈V를 갖고 용량이 c_e >0인 간선들을 갖는다.



## Ford-Fulkerson알고리즘 설명

네트워크의 유량의 가장 기본적인 알고리즘은, 포드 풀커슨(Ford - Fulkersodn) 알고리즘과, 에드몬트카프(Edmonds - Karp) 알고리즘이 있다.

1. 네트워크에 존재하는 모든 간선의 유량을 0 으로 초기화하고, 역방향 간선의 유량도 0 으로 초기화한다.
2. 소스에서 싱크로 갈 수 있는, 잔여 용량이 남은 경로를 DFS 로 탐색한다.
3. 해당 경로에 존재하는 간선들의 잔여 용량 중, 가장 작은 값을 유량으로 흘려보낸다.
4. 해당 유량에 음수값을 취해, 역방향 간선에도 흘려보낸다. (*유량 상쇄*)
5. 더 이상 잔여 용량이 남은 경로가 존재하지 않을때까지 반복한다.

포드-풀커슨 알고리즘은 DFS 방식으로 증가 경로를 찾고,
에드몬드-카프 알고리즘은 BFS 방식으로 증가 경로를 찾는다.

그림을 통해 이해해보자.

![Algorithm](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIbcFj%2Fbtrn1gG9AuM%2FS3rGLKaRJQgXQUekMjSlv0%2Fimg.png)

- 위 그림에서, DFS(깊이 우선 탐색) 를 이용한 증가 경로 탐색은 아래와 같다.

![Algorithm](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAj8EH%2Fbtrn4q25OWZ%2FANSV0s2LKVeUj6s8byCkGk%2Fimg.png)

- s->b->a->t 경로에 존재하는 간선들의 잔여 용량을 확인한 뒤, 잔여 용량 중 가장 작은 값인 1을 유량으로 흘려보낸다.

![Algorithm](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbGipEw%2Fbtrn1hTxMaE%2FbrITVR2GHuUwqwjlKKnZ30%2Fimg.png)

- 결과적으로, 간선 (a,b)와 (b,a)가 상쇄되므로, 아래와 같은 정답이 도출되는 것을 알 수 있다.
![Algorithm](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeFiu69%2Fbtrn1y8CYON%2F4yDK5W48LKLXwtHHZQZyk1%2Fimg.png)




## 코드 구현
잔여 용량이 없을 때까지, 즉 보낼 수 있는 유량이 없을 때까지 DFS 반복

```
dfs 코드(깊이 우선 탐색)
    public static boolean dfs(int start) {
        if (start == Sink) {
            return true;
        }

        visited[start] = true;
        LinkedList<Integer> nexts = graph[start];
        for (int next: nexts) {
            if ( !visited[next] && capacity[start][next] - flow[start][next] > 0) {
                path[next] = start;

                if (dfs(next)) { // 경로를 끝까지 찾으면 탈출, 아니면, 끝까지 찾기 재시도
                    return true;
                }
            }
        }
        return false;
    }
```



```
ford-fulkerson 코드

 public static int FordFulkerson() {
        int total = 0;
        while (dfs(Source)) { // dfs로 경로 찾기(증가경로), 경로가 더이상 없으면 종료임.
            // 찾은 경로에서 차단 간선 찾기 min (capacity[u][v] - flow[u][v])
            // 결국 의미는 경로에서 흘릴수 있는 최대의 유량(flow)을 찾기
            int flowNum = Integer.MAX_VALUE;
            for(int i = Sink; i != Source; i = path[i]) {
                int from = path[i];
                int to = i;
                flowNum = Math.min(flowNum, (capacity[from][to]) - flow[from][to]);
            }
            // 찾은 경로에 유량을 흘려보내기, 역방향도 반드시 진행한다.
            for(int i = Sink; i != Source; i = path[i]) {
                int from = path[i];
                int to = i;

                flow[from][to] += flowNum;
                flow[to][from] -= flowNum;
            }

            total += flowNum;

            // 찾은 경로를 초기화해서 dfs로 경로 찾기를 Source > Sink 까지 다시 할 수 있게 함.
            Arrays.fill(path, -1);
            Arrays.fill(visited, false);
        }
        return total;
    }

```


### 시간복잡도

O(F*E) 의 시간복잡도를 가진다.(F: 그래프에 흐를 수 있는 최대 용량)  

최악의 경우가 존재하기도 해서 bfs를 이용하여 증가 경로 탐색을 진행하는 Edmonds - Karp 일고리즘을 이용하기도 한다.




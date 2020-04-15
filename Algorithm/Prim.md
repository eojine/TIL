# Prim Algorithm

> MST

- 하나의 정점에서 연결된 간선들 중에 하나씩 선택하면서 MST를 만들어가는 방식.
- 정점을 하나씩 선택할 때마다 간선을 추가하면서 트리 확장

### 동작 과정

1. 임의 정점을 하나 선택해서 시작
2. 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택, 갱신
3. 모든 정점이 선택될 때까지 1, 2 과정을 반복.

## Prim 인접행렬

```java
import java.io.StringReader;
import java.util.Arrays;
import java.util.Scanner;

public class Prim_Prac {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        sc = new Scanner(new StringReader(src));

        int V = sc.nextInt();
        int E = sc.nextInt();
        int[][] adj = new int[V][V];

        // 간선 정보를 받는다.
        for (int i = 0; i < E; i++) {
            int x = sc.nextInt();
            int y = sc.nextInt();
            int cost = sc.nextInt(); // 가중치

            adj[x][y] = cost;
            adj[y][x] = cost;
        }

        boolean[] check = new boolean[V];
        int[] key = new int[V]; // 현재 선택된 정점들로부터 도달할 수 있는 최소거리
        int[] p = new int[V]; // 최소신장트리의 구조를 저장할 배열

        Arrays.fill(key, Integer.MAX_VALUE); // key의 초기값은 모두 무한대!

        int start = 0; // start할 정점을 임의의 값(0)으로 설정한다.
        p[start] = -1;
        key[start] = 0;

        // 이미 시작 정점을 골랐으니 나머지 V-1개 정점을 돈다.
        for (int i = 0; i < V - 1; i++) {
            int min = Integer.MAX_VALUE; // 가장 작은 값을 가진 정점을 찾음.
            int idx = -1; // 찾은 정점의 인덱스를 저장
            for (int j = 0; j < V; j++) {
                if (check[j]) continue;
                if (min > key[j]) {
                    min = key[j];
                    idx = j;
                }
            }

            // 가장 작은값의 정점을 찾았다!
            check[idx] = true;

            // 이 작은값의 정점들에 인접하는 곳에 key값을 업데이트한다.
            for (int j = 0; j < V; j++) {
                // 체크가 되어있지 않고, 간선이 존재하고, 원래의 정점cost보다 작은 값만 갱신시켜준다.
                if (!check[j] && adj[idx][j] != 0 && adj[idx][j] < key[j]) {
                    key[j] = adj[idx][j];
                    p[j] = idx;
                }
            }
        }
        int result = 0;
        for (int i = 0; i < V; i++) {
            result += key[i];
        }
        System.out.println(result);
    }

    static String src = "7 11\\n" +
            "0 1 2\\n" +
            "0 2 2\\n" +
            "0 5 8\\n" +
            "1 2 1\\n" +
            "1 3 19\\n" +
            "2 5 5\\n" +
            "3 4 7\\n" +
            "3 5 11\\n" +
            "3 6 15\\n" +
            "4 5 9\\n" +
            "4 6 3";
}
```

## Prim 인접리스트, Priority Queue
```java
import java.io.StringReader;
import java.util.ArrayList;
import java.util.PriorityQueue;
import java.util.Scanner;

public class Prim_PQ {
    static class Edge implements Comparable<Edge> {
        int dest;
        int cost;

        Edge(int d, int c) {
            dest = d;
            cost = c;
        }

        @Override
        public int compareTo(Edge o) {
            return Integer.compare(this.cost, o.cost);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        sc = new Scanner(new StringReader(src));

        int V = sc.nextInt();
        int E = sc.nextInt();
        // 각 정점별로 인접리스트 참조변수
        ArrayList<Edge>[] adj = new ArrayList[V];
        for (int i = 0; i < V; i++) {
            adj[i] = new ArrayList<>();
        }
        // 입력되어지는 간선 배열을 인접리스트에 저장
        for (int i = 0; i < E; i++) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            int c = sc.nextInt();
            adj[a].add(new Edge(b, c));
            adj[b].add(new Edge(a, c));
        }
        // dist와 pq를 동기화.
        PriorityQueue<Edge> pq = new PriorityQueue<>();
        Edge[] dist = new Edge[V];
        
        // 시작 정점이 0이라고 가정.
        int start = 0;
        for (int i = 0; i < V; i++) {
            if (i == start) { // 시작은 거리 0부터 시작.
                dist[start] = new Edge(start, 0);
            } else { // dist안의 거리들은 무한대로 초기화.
                dist[i] = new Edge(i, Integer.MAX_VALUE);
            }
            pq.add(dist[i]);
        }

        boolean[] visit = new boolean[V];
        int result = 0;
        while (!pq.isEmpty()) {
            // 현재 dist가 가장 작은 친구를 데려와서
            Edge e = pq.poll();
            if (visit[e.dest]) {
                continue;
            }
            visit[e.dest] = true;
            result += e.cost;
            // 그 친구로부터 출발하는 모든 간선에 대해서
            for (Edge next : adj[e.dest]) {
                // 그 간선의 목적지가 아직 pq에 들어있는 정점이라면
                // 그리고 더 빨리 도착할 수 있다면
                if (!visit[next.dest] && dist[next.dest].cost > next.cost) {
                    // dist갱신
                    dist[next.dest].cost = next.cost;
                    // decrease key연산
                    pq.remove(dist[next.dest]);
                    pq.add(dist[next.dest]);
                }
            }
        }
        System.out.println(result);
        // System.out.println(Arrays.toString(p));
    }
    static String src = "7 11\n" +
            "0 1 2\n" +
            "0 2 2\n" +
            "0 5 8\n" +
            "1 2 1\n" +
            "1 3 19\n" +
            "2 5 5\n" +
            "3 4 7\n" +
            "3 5 11\n" +
            "3 6 15\n" +
            "4 5 9\n" +
            "4 6 3";
}
```

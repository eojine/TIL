# Kruskal

> 탐욕적인 방법을 이용하여 가중그래프의 모든 정점을 최소비용으로 연결하는 최소의 해를 갖는 것.

### 동작과정

1. 그래프의 간선들을 가중치의 오름차순으로 정렬한다.
2. 정렬된 간선 리스트에서 순서대로 사이클을 형성하지 않는 간선을 선택한다.
    1. 가중치가 낮은 간선을 선택한다.
    2. 사이클이 형성되면 다음 간선을 선택.
3. 해당 간선을 MST 집합에 추가한다 (n-1)개의 간선이 선택될 때까지 반복 // n : 정점의 수

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.Scanner;

public class Kruskal {

    static int[] parents;
    static int[] rank;

    // 입력은 첫 줄에 정점의 개수와 간선의 개수가 들어오고,
    // 그 다음줄부터 간선의 정보가 정점1, 정점2 가중치로 간선의 갯수만큼 들어옴.
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int V = sc.nextInt();
        int E = sc.nextInt();
        int[][] edges = new int[E][3]; // 정점 1, 정점 2, 가중치
        parents = new int[V];
        rank = new int[V];

        for (int i = 0; i < E; i++) {
            edges[i][0] = sc.nextInt();
            edges[i][1] = sc.nextInt();
            edges[i][2] = sc.nextInt(); // 가중치
        }

        // 간선들을 가중치 오름차순으로 정렬
        Arrays.sort(edges, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                // o1[2] - o2[2] // 뺄셈도 가능하긴 한데, 언더플로우 발생할 수 있기 때문에 Compare사용.
                return Integer.compare(o1[2], o2[2]);
            }
        });

        // 각 정점에 대해 유니온파인드 연산 준비.
        for (int i = 0; i < V; i++) {
            makeSet(i);
        }

        int result = 0;
        int cnt = 0; // 방문한 정점의 개수
        for (int i = 0; i < E; i++) {
            int a = findSet(edges[i][0]);
            int b = findSet(edges[i][1]);

            // 같은 팀이라면 패스
            if (a == b)
                continue;

            // 간선이 연결하는 두 정점이 같은 팀이 아니라면 한 팀으로 합쳐주고, 간선 선택
            union(a, b);

            // 간선을 선택.
            result += edges[i][2];
            cnt++;
            // 정점의 개수 n-1번 반복하면서
            if (cnt == V - 1) {
                break;
            }
        }

        System.out.println(result);
    }

    // union find
    static void makeSet(int x) {
        parents[x] = x;
    }

    static int findSet(int x) {
        if (x == parents[x])
            return x;
        else {
            parents[x] = findSet(parents[x]);
            return parents[x];
        }
    }

    // 두 개의 집합을 하나로 합침.
    static void union(int x, int y) {
        int px = findSet(x);
        int py = findSet(y);

        // 짧은 레벨을 긴쪽에 붙임.
        if (rank[px] > rank[py]) {
            parents[py] = px;
        } else {
            parents[px] = py;
            if (rank[px] == rank[py]) {
                rank[py]++;
            }
        }
    }
}
```

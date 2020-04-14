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

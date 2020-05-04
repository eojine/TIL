# Floyd Warshall
> O[n^3]
- 마지막 모든 최단 경로를 구하는 방법이다.
- 음의 가중치 사용 가능하다.
- 모든 정점에 대한 경로를 계산하므로 거리를 저장할 자료구조는 2차원 배열이 된다.
<br>

```java
import java.util.Arrays;

public class 플로이드워샬 {
    public static void main(String[] args) {
        final int M = Integer.MAX_VALUE; // 그냥 가장 큰 숫자인 천만정도로 넣어도 된다.
        int[][] D = {
                {0, M, 2, 3},
                {4, 0, 1, 8},
                {2, 5, 0, M},
                {M, 9 ,6, 0}
        };

        for (int k = 0; k < D.length; k++) { // 경유 정점
            for (int i = 0; i < D.length; i++) { // 시작 정점
                if (k == i) continue;
                for (int j = 0; j < D.length; j++) { // 도착 정점
                    if (k == j || i == j) continue; // 어차피 자기 자신으로 돌아오는 번호는 없으니 쓰지 않아도 상관 없다. (성능은 약간 좋아짐)
                    if (D[i][k] == M || D[k][j] == M) continue; // 그럼 이 주석이 없어도됨.
                    if (D[i][j] > D[i][k] + D[k][j]) {
                        D[i][j] = D[i][k] + D[k][j];
                    }
                }
            }
        }

        for (int i = 0; i < D.length; i++) {
            System.out.println(Arrays.toString(D[i]));
        }
    } // end of main
} // end of class
```

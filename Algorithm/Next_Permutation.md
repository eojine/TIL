# Next Permutation

> 순열을 만드는 방법 중 하나! (C++에는 Next Permutation API가 있다)
>
> Java로 직접 구현해보기~~

- 현재 순열의 상태에서 사전 순으로 나열했을 때 그 다음 단계의 순열을 생성함.
- 최종 목표
  - **가장 큰 순열**을 생성하는것!

- 장점
  - 중복된 수가 있는 배열에서 중복 제거된 순열을 얻을 수 있다. (효율적)
  - 함수 호출에 대한 오버헤드가 없다.
- 단점
  - N개중에 R개 뽑는 순열 구현 불가.
  - 중복순열 구현 불가.



### 구현법

1.  배열의 뒤쪽부터 탐색하며 꼭대기(i)를 찾는다. (i == 0이면 다음 단계 순열 생성 불가)
2.  배열의 뒤쪽부터 탐색하며 교환할 큰 값 찾기(j)
3.  i - 1과 j의 위치값을 교환한다.
4.  i위치부터 N-1(맨뒤)까지 내림차순형태의 숫자를 오름차순으로 만듦.


```java
import java.util.Arrays;

public class NextPermutation {

    static int N;
    static int[] p; // 순열용 배열!

    public static void main(String[] args) {

        int[] arr = {1, 3, 2, 4, 5};
        N = arr.length;
        p = new int[N];

        for (int i = 0; i < N; i++) {
            p[i] = arr[i];
        }

        do {
            System.out.println(Arrays.toString(p));
        } while (nextPermutation());
    }

    // boolean: true => 다음 순열 생성 ok, false => 다음 순열 생성 불가.(이미 제일 큰 순열이다.)
    private static boolean nextPermutation() {
        // 1. 뒤쪽부터 탐색하며 꼭대기(i) 찾기 : i - 1 => 교환위치
        int i = N - 1;
        while(i > 0 && p[i - 1] >= p[i]) --i;
        if (i == 0) return false; // 이미 제일 큰 마지막 순열이므로 다음 순열 없음.

        // 2. 뒤쪽부터 탐색하며 교환할 큰 값(j) 찾기.
        int j = N - 1;
        while(p[i - 1] >= p[j]) --j;

        // 3. i - 1과 j위치 교환.
        int temp = p[i - 1];
        p[i - 1] = p[j];
        p[j] = temp;

        // 4. i위치부터 N-1(맨뒤)까지 내림차순형태의 숫자를 오름차순(가장 작은 수)로 만들기 위해 정렬
        int k = N - 1;
        while(i < k) {
            temp = p[i];
            p[i] = p[k];
            p[k] = temp;
            ++i;
            --k;
        }
        return true;
    }
}

```


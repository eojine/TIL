# KMP 알고리즘
> 모든 경우를 다 비교하지 않아도 부분 문자열을 찾을 수 있는 알고리즘

- 접두사와 접미사 개념을 활용해서, 반복되는 연산을 최대한 줄이도록하는 컨셉.
- **지금까지 검사한 문자열의 접두사와 접미사가 일치하는 최대 길이만큼 겹침을 이용함.**
- 불일치가 발생한 텍스트 문자열의 앞부분에 어떤 문자가 있는지를 미리 알고있으므로, 불일치가 발생한 앞 부분에 대하여 다시 비교하지 않고 매칭을 수행
- 시간복잡도 : O(N+M) // N : 문자열의 길이, M :패턴 길이
    - 문자열 길이 : 100000, 패턴 길이 : 1000 일때,<br>
    **brute-force** : 100000 * 1000 = 1억번 검사<br>
    **KMP** : 100000 + 1000 = 101000번 검사


```java
public class KMP {
    // 파이 테이블 : 각 길이별로 접두사 접미사의 최대 길이가 저장된 배열
    static int[] getPi(String pattern) {
        // 정수배열 준비
        int[] pi = new int[pattern.length()];
        int j = 0;

        // i는 두 번째 칸부터 시작함. (계속 증가함)
        for (int i = 1; i < pattern.length(); i++) {

            while(j > 0 && pattern.charAt(i) != pattern.charAt(j))
                j = pi[j - 1];

            // 맞는 경우
            if (pattern.charAt(i) == pattern.charAt(j)) {
                // i 길이 문자열의 공통 길이는 j의 위치 + 1, j도 다음칸으로 이동
                pi[i] = ++j;
            }

        }
        return pi;
    }

    static void KMP(String origin, String pattern) {
        int[] pi = getPi(pattern);
        int j = 0;

        // 원문의 처음부터 끝까지 탐색함.
        for (int i = 0; i < origin.length(); i++) {

            // 일단 앞으로 이동한다.
            while(j > 0 && origin.charAt(i) != pattern.charAt(j))
                j = pi[j - 1];

            // 맞는 경우
            if(origin.charAt(i) == pattern.charAt(j)) {
                if ( j == pattern.length() - 1) {
                    System.out.println("ㅇㅋ " + (i - pattern.length() + 1));
                    j = pi[j];
                }

                // 맞았지만 패턴이 끝나지 않았다면 j를 하나 증가.
                else j++;
            }
        }
    }

    public static void main(String[] args) {
        String origin = "HELLSAOSSA";
        String pattern = "SA";
        KMP(origin, pattern);
    }
}
```

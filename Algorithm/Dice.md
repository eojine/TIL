# 주사위 던지기!
중복순열, 순열, 중복조합, 조합으로 주사위를 던진다.

```java
import java.util.Arrays;
import java.util.Scanner;

public class DiceTest {

    static int N; // 주사위 던지는 횟수
    static int M; // 주사위 던지는 모드
    static int[] number; // 뽑히는 주사위 수 넣을 배열

    static boolean[] isSelected;

    static int totalCnt; // 총 경우의 수

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        M = sc.nextInt();
        number = new int[N];
        isSelected = new boolean[7]; // 주사위 눈이 1 ~ 6이기 때문

        switch (M) {
            case 1: // 주사위던지기 1 : 중복순열
                dice1(0);
                break;
            case 2: // 주사위던지기 2 : 순열
                dice2(0);
                break;
            case 3: // 주사위 던지기 3 : 중복조합
                dice3(0, 1);
                break;
            case 4: // 주사위 던지기 4 : 조합
                dice4(0, 1);
                break;
            default:
        }

        System.out.println("총 경우의 수 : " + totalCnt);
    }

    private static void dice1(int cnt) { // 주사위 던지기 1 : 중복순열
        if (cnt == N) {
            totalCnt++;
            System.out.println(Arrays.toString(number));
            return;
        }

        for (int i = 1; i <= 6; i++) {
            number[cnt] = i;
            dice1(cnt + 1);
        }
    }

    private static void dice2(int cnt) { // 주사위 던지기 2 : 순열
        if (cnt == N) {
            totalCnt++;
            System.out.println(Arrays.toString(number));
            return;
        }

        for (int i = 1; i <= 6; i++) {
            if (isSelected[i]) continue;

            isSelected[i] = true;
            number[cnt] = i;
            dice2(cnt + 1);
            isSelected[i] = false;
        }
    }

    private static void dice3(int cnt, int cur) { // 주사위 던지기 3 : 중복조합
        if (cnt == N) {
            totalCnt++;
            System.out.println(Arrays.toString(number));
            return;
        }

        for (int i = cur; i <= 6; i++) { // cur ~ 6
            number[cnt] = i;
            dice3(cnt + 1, i);
        }
    }

    private static void dice4(int cnt, int cur) { // 주사위 던지기 4 : 조합
        if (cnt == N) {
            totalCnt++;
            System.out.println(Arrays.toString(number));
            return;
        }

        for (int i = cur; i <= 6; i++) { // cur ~ 6
            number[cnt] = i;
            dice4(cnt + 1, i + 1);
        }
    }
}
```

# 병합정렬

> 배열을 이용하여 병합정렬하기 <br>
> 시간복잡도 : O(nlogn)

* 병합 정렬은 다음의 단계들로 이루어진다.
  - 분할(Divide): 입력 배열을 같은 크기의 2개의 부분 배열로 분할한다.
  - 정복(Conquer): 부분 배열을 정렬한다. 부분 배열의 크기가 충분히 작지 않으면 순환 호출 을 이용하여 다시 분할 정복 방법을 적용한다.
  - 결합(Combine): 정렬된 부분 배열들을 하나의 배열에 합친다.

<br>

### Swift 코드

```swift
/// 정렬되지 않은 리스트를 n개의 서브 리스트로 만들기
func mergeSort(list: [Int]) -> [Int] {
    if list.count <= 1 {
        return list
    }

    var leftList: [Int] = []
    var rightList: [Int] = []

    let mid = list.count / 2
    leftList += list[0..<mid]
    rightList += list[mid..<list.count]

    var left = mergeSort(list: leftList)
    var right = mergeSort(list: rightList)

    return merge(&left, &right)
}

/// 서브 리스트들을 합침
func merge(_ left: inout [Int], _ right: inout [Int]) -> [Int] {
    var result: [Int] = []

    while !left.isEmpty && !right.isEmpty {
        let value = left.first! < right.first! ? left.removeFirst() : right.removeFirst()
        result += [value]
    }

    if !left.isEmpty {
        result += left
    }

    if !right.isEmpty {
        result += right
    }

    return result
}
```

#### Generic 사용

```swift
func merge<T: Comparable>(_ left: inout [T], _ right: inout [T]) -> [T]
func mergeSort<T: Comparable>(list: [T]) -> [T]
```



### Java 코드

```java
import java.util.Arrays;

public class MergeSort {

	public static int[] arr = { 6, 4, 8, 5, 7, 2, 9, 3, 0, 1 };

	public static void main(String[] args) {
		mergeSort(0, arr.length);
		System.out.println(Arrays.toString(arr));
	}

	/** 입력받은 배열의 left~right 덩어리를 반으로 쪼개서 재귀호출, 최소 단위까지 쪼갠것을 다시 합침 */
	// 앞 포함 뒤 미포함 !
	public static void mergeSort(int left, int right) {
		if (right - left <= 1) {
			return;
		}

		int mid = (left + right) / 2;

		mergeSort(left, mid);
		mergeSort(mid, right);

		merge(left, mid, right); // mid를 중심으로 두 영역을 합쳐라.
	}

	/** 입력받은 배열의 left~mid, mid~right 덩어리를 합치기 */
	public static void merge(int left, int mid, int right) {
		int[] temp = new int[right - left]; // 정복하면서 저장할 임시배열
		int l = left; // 왼쪽 배열의 시작 index
		int r = mid; // 오른쪽 배열의 시작 index
		int index = 0; // 병합한 배열의 index

		while (l < mid && r < right) { // 둘다 남아있는 경우
			if (arr[l] < arr[r])
				temp[index++] = arr[l++];
			else
				temp[index++] = arr[r++];
		}

		// 왼쪽만 남아있는 경우
		System.arraycopy(arr, l, temp, index, mid - l);

		// 오른쪽만 남아있는 경우
		System.arraycopy(arr, r, temp, index, right - r);

		// 임시배열 temp 원본 배열에 붙여넣기
		System.arraycopy(temp, 0, arr, left, right - left);
	}
}
```

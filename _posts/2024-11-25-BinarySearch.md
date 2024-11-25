---
layout: post
title: "[Algorithm] 이진탐색(BinarySearch)과 응용"
author:
    name: "JongSeo"
    link: https://github.com/csjsseo
date: 2024-11-25 23:45:00 +0900
categories: [CodingTest, Algorithm]
tags: [Algorithm, Java, BinarySearch]
---

# 이진 탐색 (Binary Search)

이진 탐색은 **정렬된 배열**에서 특정 값을 효율적으로 찾기 위한 알고리즘입니다.<br> 
데이터를 절반씩 줄여가며 탐색하기 때문에 매우 빠르고, 코딩 테스트에서 자주 등장하는 기법입니다.

---

## 기본 원리

1. **정렬된 배열**에서 중간값을 선택합니다.
2. 중간값과 찾고자 하는 값을 비교합니다:
   - **찾는 값 == 중간값**: 탐색 종료.
   - **찾는 값 < 중간값**: 중간값 기준 왼쪽 부분에서 탐색.
   - **찾는 값 > 중간값**: 중간값 기준 오른쪽 부분에서 탐색.
3. 탐색 범위를 절반으로 줄이며 반복합니다.
4. 검색 범위가 더 이상 없으면 값을 찾을 수 없다는 결과를 반환합니다.

---

## 시간 복잡도

- **O(log N)**: 탐색 범위를 절반씩 줄이므로 매우 효율적입니다.
- **O(1)** (반복문 방식) 또는 **O(log N)** (재귀 방식) 공간 복잡도를 갖습니다.

---
## 📌 문제 예시 1번 - 수 찾기

## 코드
```java
import java.util.Arrays;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // 배열 크기 및 배열 입력
        int N = sc.nextInt();
        int[] searchArray = new int[N];
        for (int i = 0; i < N; i++) {
            searchArray[i] = sc.nextInt();
        }

        // 배열 정렬
        Arrays.sort(searchArray);

        // 찾을 값 입력
        int M = sc.nextInt();
        int[] targetArray = new int[M];
        for (int i = 0; i < M; i++) {
            targetArray[i] = sc.nextInt();
        }

        // 각 값을 이진 탐색으로 확인
        for (int i = 0; i < M; i++) {
            System.out.println(binarySearch(searchArray, targetArray[i]) ? "1" : "0");
        }
    }

    public static boolean binarySearch(int[] arr, int target) {
        int start = 0;
        int end = arr.length - 1;

        while (start <= end) {
            int mid = start + (end - start) / 2;

            if (arr[mid] == target)
                return true;
            else if (arr[mid] < target)
                start = mid + 1;
            else
                end = mid - 1;
        }
        return false;
    }
}
```
---
## 입출력 예시

### 입력
첫 줄에 크기를 입력받고,
그 다음 줄에 크기만큼의 데이터를 입력받는다.
```plaintext
5
4 1 5 2 3
5
1 3 7 9 5
```

### 출력
```plaintext
1
1
0
0
1
```
---
## 📌 문제 예시 2번 - 나무 자르기
## 코드
```java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String[] split = br.readLine().split(" ");

        int K = Integer.parseInt(split[0]); // 가지고 있는 나무의 갯수
        int N = Integer.parseInt(split[1]); // 필요한 나무의 갯수

        int[] lanCables = new int[K];
        long maxCable = 0;

        for(int i=0;i<K;i++)
        {
            lanCables[i] = Integer.parseInt(br.readLine());
            if(maxCable < lanCables[i])
                maxCable = lanCables[i];
        }
        bw.write(binarySearch(lanCables,maxCable,N) + "\n");
        bw.flush();
        bw.close();
    }

    private static long binarySearch(int[] lanCables, long max, int N)
    {
        long start = 1; // 톱날의 최소 높이
        long end = max; // 톱날의 최대 높이
        long result = 0;

        while(start <= end) {
            long mid = (start + end) / 2;
            long count = 0;

            for (int lanCable : lanCables) {
                count += lanCable / mid;
            }

            if(count >= N)
            {
                result = mid;
                start = mid + 1; // 조건을 만족할 경우, 톱날을 저장하고 높이를 올림
            }
            else
            {
                end = mid - 1; // 조건을 만족하지 않아, 톱날을 낮춤
            }

        }

        return result;

    }
}
```
---
## 입출력 예시

### 입력
첫 줄에 가지고 있는 나무의 갯수와 필요한 나무의 갯수 입력한다.<br>
그 다음 오는 줄의 나무의 높이를 입력한다.
```plaintext
4 7
20 15 10 17
```

### 출력
```plaintext
15
```
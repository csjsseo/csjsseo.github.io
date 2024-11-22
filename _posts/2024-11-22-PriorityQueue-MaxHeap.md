---
layout: post
title: "[DataStructure] 우선순위 큐(PriorityQueue)과 최대 힙(MaxHeap)"
author:
    name: "JongSeo"
    link: https://github.com/csjsseo
date: 2024-11-22 17:00:00 +0900
categories: [CodingTest, DataStructure]
tags: [priorityQueue, Java, MaxHeap]
---

우선순위 큐(Priority Queue)는 **데이터를 우선순위에 따라 정렬하여 처리**하는 자료구조입니다. <br>
일반적인 큐(FIFO, First In First Out)와 달리, 우선순위 큐는 **우선순위가 높은 데이터가 먼저 처리**됩니다. <br>
이는 힙(Heap) 자료구조를 사용해 구현하는 것이 일반적이며, Java에서는 `PriorityQueue` 클래스를 제공합니다.

---

## PriorityQueue 클래스의 특징

### 1. 기본 동작
- 기본적으로 **최소 힙(Min-Heap)**으로 동작합니다. 즉, 값이 작은 데이터가 높은 우선순위를 가집니다.
- 우선순위를 커스터마이징하려면 `Comparator`를 사용해야 합니다.

### 2. 주요 메서드

| 메서드                   | 설명                                  |
|--------------------------|---------------------------------------|
| `add(E e)` / `offer(E e)` | 데이터를 큐에 추가                      |
| `poll()`                 | 우선순위가 가장 높은 데이터를 반환하고 제거 |
| `peek()`                 | 우선순위가 가장 높은 데이터를 반환하지만 제거하지 않음 |
| `isEmpty()`              | 큐가 비어 있는지 확인                  |

---
## 문제 정의

아래 코드는 **최대 힙(Max-Heap)**을 구현한 예제입니다.  
사용자는 정수를 입력하고, `0`을 입력하면 현재 큐에서 가장 큰 값을 출력합니다.  
큐가 비어 있으면 `0`을 출력합니다.

---
## 코드 구현

```java
import java.io.*;
import java.util.PriorityQueue;

public class Main {
    public static void main(String[] arg) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine()); // 입력 개수

        // 최대 힙 선언 (Comparator를 통해 우선순위 반전)
        PriorityQueue<Integer> queue = new PriorityQueue<>((a, b) -> b - a);

        for (int i = 0; i < N; i++) {
            int opt = Integer.parseInt(br.readLine());

            if (opt == 0) { // 큐에서 가장 큰 값 출력
                bw.write(queue.isEmpty() ? "0\n" : queue.poll() + "\n");
            } else { // 큐에 데이터 추가
                queue.add(opt);
            }
        }
        bw.flush();
        bw.close();
    }
}
```
---
## 입출력 예시

### 입력
다음과 같이 정수를 입력합니다.  
`0`은 큐에서 가장 큰 값을 출력하는 명령입니다.

```plaintext
5
1
2
0
3
0
```

### 출력
```plaintext
2
3
```
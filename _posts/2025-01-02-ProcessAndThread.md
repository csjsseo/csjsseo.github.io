---
layout: post
title: "프로세스 & 스레드"
date: 2025-01-02 21:00:00 +0900
categories: [ComputerScience, Operating System]
tags: [Process, Thread]
---

### 프로세스 (Process)
- 프로그램을 메모리 상에서 실행 중인 작업
- 프로세스마다 최소 1개의 스레드를 소유 (메인 스레드 포함)
- 각각 별도의 주소 공간 할당 (독립적)
- ex) 독립적으로 실행되어야 하며 다른 실행 단위와 격리된 작업 (예: 웹 브라우저 탭)

### 스레드 (Thread)
- 프로세스 안에서 실행되는 여러 흐름 단위
- 스레드는 Stack만 따로 할당받고, 나머지 메모리 영역(Code, Data, Heap)을 공유
- 하나의 프로세스가 생성될 때, 기본적으로 하나의 스레드가 같이 생성됨
- 동일한 자원을 활용하며 병렬 처리가 필요한 작업 (예: 게임에서 그래픽, 입력, 네트워크 처리)

---

### 1. 메모리 구조

#### 프로세스 메모리 구조
- **Code**: 프로그램 명령이 저장된 영역
- **Data**: 전역 변수, 정적 변수, 배열 등이 저장된 영역
  - 초기화된 데이터는 Data 영역에 저장
  - 초기화되지 않은 데이터는 BSS 영역에 저장
- **Heap**: 동적 할당 시 사용 (예: `new()`, `malloc()` 등)
- **Stack**: 지역 변수, 매개변수, 리턴 값 등 임시 데이터를 위한 메모리 영역

#### 스레드 메모리 구조
- 스레드는 **Stack**만 따로 할당받고, **Code, Data, Heap** 영역은 같은 프로세스 내에서 공유
- 스레드 간 메모리 공유로 인해 동기화 문제가 발생할 수 있음

#### 메모리 구조 시각화
![프로세스와 스레드 메모리 구조](assets/img/data/process_thread.PNG)

---

### 2. 주요 차이점

| 구분               | 프로세스                                 | 스레드                                |
|--------------------|------------------------------------------|---------------------------------------|
| **메모리 공유 여부** | 독립적인 메모리 공간을 가짐                 | 같은 프로세스 내에서 메모리와 자원을 공유 |
| **실행 속도**       | 상대적으로 느림                           | 상대적으로 빠름                        |
| **통신 방식**       | IPC(프로세스 간 통신) 필요                  | 메모리 공유로 통신이 간단함             |
| **오버헤드**        | 프로세스 생성 시 오버헤드 큼                 | 스레드 생성 시 오버헤드 적음             |
| **안정성**          | 하나의 프로세스가 종료되어도 다른 프로세스에 영향 없음 | 하나의 스레드가 문제를 일으키면 프로세스 전체에 영향 |

---


### 3. 멀티 프로세스
- 하나의 프로그램을 여러 개의 프로세스로 구성하여 병렬 작업 수행
- 장점
    - **안정성**: 메모리 침범 문제를 운영체제 차원에서 해결
- 단점
    - 각각 독립된 메모리 영역을 갖고 있어 작업량이 많을수록 **오버헤드** 발생
    - **Context Switching**으로 인한 성능 저하

> **Context Switching**
> - 프로세스의 상태 정보를 저장하고 복원하는 일련의 과정
> - 캐시 메모리 초기화 등의 무거운 작업이 진행될 때 오버헤드 발생

---

### 4. 멀티 스레드
- 하나의 응용 프로그램에서 여러 스레드를 구성해 각 스레드가 병렬적으로 작업 수행
- 장점
    - **효율성**: 독립적인 프로세스에 비해 스레드 간 메모리 공유로 빠른 데이터 접근이 가능 -> 시간, 자원 손실 감소
    - 전역 변수와 정적 변수에 대한 자료 공유 가능
- 단점
    - **안전성 문제**: 하나의 스레드가 데이터 공간을 망가뜨리면 모든 스레드가 작동 불능 상태가 될 수 있음
    - 동기화 문제 발생 가능성

> **동기화(Critical Section)**
> - 하나의 스레드가 공유 데이터 값을 변경하는 시점에 다른 스레드가 그 값을 읽으려 할 때 발생하는 문제를 해결
> - 상호 배제, 진행, 한정된 대기 조건을 충족해야 함

---

### 5. 정리

#### 메모리 구조
- 프로세스는 독립적인 힙/스택/코드/데이터 영역 사용
- 스레드는 공유된 힙/코드/데이터 영역을 사용, 각자 스택만 독립적으로 사용

#### 비교 사례 
- **상황 - 대규모 데이터 처리**
  - **멀티프로세스 사용**
    - 데이터가 독립적으로 처리되어야 함
    - 하나의 작업이 실패하더라도 나머지 작업에는 영향을 미치지 않아야 한다면 멀티프로세스가 적합
    - ex) PDF 변환 과정에서 각 파일 변환 작업을 별도의 프로세스로 처리

  - **멀티스레드 사용**
    - 데이터가 동일한 메모리 공간에서 처리되어야 함
    - 병렬 처리가 필요한 경우 멀티스레드가 적합
    - ex) 대규모 이미지 처리 프로그램에서 각 이미지의 필터를 다른 스레드로 처리
    

#### 실제 경험
- **예시 -  대학교 팀 프로젝트**
  - 팀원들과 실시간 서울 지하철 지연 예측 서비스 개발 과정에서 경로 찾기 알고리즘 사용해 경로 최적화를 구현
  - **멀티스레드**를 활용해 데이터 수집, 경로 계산, 사용자 인터페이스 업데이트를 병렬 처리
  - 이 경험을 통해 스레드 간 데이터 동기화 문제를 해결하며 안정성을 높이는 방법을 배움

---
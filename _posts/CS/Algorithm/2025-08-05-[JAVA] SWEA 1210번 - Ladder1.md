---
title: "[Algorithm/Java] SWEA 1210번 - Ladder1 (작성중)"
categories: [Algorithm]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

---

[1210. Ladder1](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14ABYKADACFAYh)

---

<br>

# 🔍 문제 풀이

## 문제 도식화

1. 좌우에 `0` padding을 추가하면 범위 체크를 생략할 수 있어 탐색 코드가 간결해진다.
2. 도착 지점(2)에서 **역방향으로 출발해 위로 올라가면** 경로 추적이 쉬워진다.
3. **사다리는 좌/우로 이어진 경로가 있을 수 있으므로,** 이동할 수 있는 방향이 있을 때는 그 방향으로 끝까지 이동해야 한다.

<br>

## 풀이 방법

1. 도착 지점의 위치 (`si, sj`)를 찾는다.
2. `while(ci > 0)` 반복:
   - 왼쪽(`arr[ci][cj-1] == 1`)이면 왼쪽 끝까지 `cj--`
   - 아니고 오른쪽(`arr[ci][cj+1] == 1`)이면 오른쪽 끝까지 `cj++`
   - 좌우에 없으면 위로 한 칸(`ci--`)
3. 반복이 끝났을 때 `cj`는 정답 좌표

<br>

> 두 가지 방법으로 풀이할 수 있다.

- 0으로 지나온 길을 지우면서 이동
  - 조건문은 간단하지만 좌우 방향으로 한 칸씩만 이동
  - 좌우에 길이 있는 경우, 다음 반복에서 같은 칸을 또 방문하지 않게 0으로 막아야 함

<br>

- while문으로 좌우 끝까지 이동
  - `while(arr[ci][cj ± 1] == 1)`로 좌/우로 계속 이동
  - 방향이 끝나면 위로 한 칸 올라가도록 함

<br><br>

# 💻 전체 코드

## [방법 1] 0으로 길 없애기, 좌우 padding 적용

```java
import java.io.*;
import java.util.*;

public class Solution {
    static int[][] arr;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int t = 10;
        for(int tc=1; tc<=t; tc++){
            int format = Integer.parseInt(br.readLine());

            // 입력 및 초기화
            arr = new int[100][102]; // 좌/우 padding

            for(int i=0; i<100; i++){
                StringTokenizer st =  new StringTokenizer(br.readLine());
                for(int j=1; j<=100; j++){  // j = 1부터 100까지 저장
                    arr[i][j] = Integer.parseInt(st.nextToken());
                }
            }

            // 도착 지점(2) 위치 저장
            int si = 99, sj = 0;
            for(int j=1; j<=100; j++){
                if(arr[si][j] == 2){
                    sj = j;
                    break;
                }
            }

            // 함수 출력
            int resultSj = solved(si, sj);
            print(tc, resultSj);
        }
    }

    static int solved(int ci, int cj){
        while(ci > 0){

            // 왼쪽에 길이 있으면 끝까지 이동
            if(arr[ci][cj -1] == 1){
                arr[ci][cj] = 0;
                cj--;
            }
            // 오른쪽에 길이 있으면 끝까지 이동
            else if(arr[ci][cj +1] == 1){
                arr[ci][cj] = 0;
                cj++;
            }
            // 좌/우 둘 다 없으면 위로 이동
            else ci--;
        }
        return cj -1;
    }

    static void print(int tc, int resultSj){
        StringBuilder sb = new StringBuilder();
        sb.append("#").append(tc).append(" ");
        sb.append(resultSj);

        System.out.println(sb);
    }
}
```

<br>

## [방법 2] while문으로 좌/우 끝까지 이동

```java
import java.io.*;
import java.util.*;

public class Solution {
    static int[][] arr;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int t = 10;
        for(int tc=1; tc<=t; tc++){
            int format = Integer.parseInt(br.readLine());

            // 입력 및 초기화
            arr = new int[100][100];

            for(int i=0; i<100; i++){
                StringTokenizer st =  new StringTokenizer(br.readLine());
                for(int j=0; j<100; j++){
                    arr[i][j] = Integer.parseInt(st.nextToken());
                }
            }

            // 도착 지점(2) 위치 저장
            int si = 99, sj = 0;
            for(int j=0; j<100; j++){
                if(arr[si][j] == 2){
                    sj = j;
                    break;
                }
            }

            // 함수 출력
            int resultSj = solved(si, sj);
            print(tc, resultSj);
        }
    }

    static int solved(int ci, int cj){
        while(ci > 0){

            // 왼쪽에 길이 있으면 끝까지 이동
            if(cj-1 >= 0 && arr[ci][cj-1] == 1){
                while (cj-1 >= 0 && arr[ci][cj - 1] == 1) cj--;
                ci--;
            }
            // 오른쪽에 길이 있으면 끝까지 이동
            else if(cj+1 < 100 && arr[ci][cj+1] == 1){
                while (cj+1 < 100 && arr[ci][cj + 1] == 1) cj++;
                ci--;
            }
            // 좌/우 둘 다 없으면 위로 이동
            else ci--;
        }
        return cj;
    }

    static void print(int tc, int resultSj){
        StringBuilder sb = new StringBuilder();
        sb.append("#").append(tc).append(" ");
        sb.append(resultSj);

        System.out.println(sb);
    }
}
```

<br>

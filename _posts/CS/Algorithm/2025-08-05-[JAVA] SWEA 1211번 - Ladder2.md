---
title: "[Algorithm/Java] SWEA 1211번 - Ladder2 (작성중)"
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

[1211. Ladder2](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14BgD6AEECFAYh)

---

<br>

# 🔍 문제 풀이

## 문제 도식화

<br>

## 풀이 방법

<br>

<br><br>

# 💻 전체 코드

```java
import java.io.*;
import java.util.*;

class Data{
    int sj;

    public Data(int sj){
        this.sj = sj;
    }
}

public class Solution {
    static int[][] arr;
    static List<Data> data;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int t = 10;
        for(int tc=1; tc<=t; tc++){
            int format = Integer.parseInt(br.readLine());

            // 입력 및 초기화
            arr = new int[100][102]; // 좌/우 padding
            data = new ArrayList<>();


            for(int i=0; i<100; i++){
                StringTokenizer st =  new StringTokenizer(br.readLine());
                for(int j=1; j<=100; j++){  // j = 1부터 100까지 저장
                    arr[i][j] = Integer.parseInt(st.nextToken());
                }
            }

            // 출발점 위치 저장
            for(int j=1; j<=100; j++){
                if (arr[0][j] == 1) {
                    data.add(new Data(j));
                }
            }


            // 최단 거리 탐색
            int min = Integer.MAX_VALUE;
            int result = 0;

            for (Data d : data) {
                int dist = getDistance(0, d.sj);

                if (dist < min) {
                    min = dist;
                    result = d.sj - 1;
                } else if (dist == min) {
                    result = Math.max(result, d.sj - 1); // 거리 같으면 더 큰 x좌표
                }
            }
            print(tc, result);
        }
    }

    static int getDistance(int si, int sj) {
        int ci = si;
        int cj = sj;
        int count = 0;

        while(ci < 100) {
            if (arr[ci][cj - 1] == 1) {
                while (cj > 0 && arr[ci][cj - 1] == 1) {
                    cj--;
                    count++;
                }
            } else if (arr[ci][cj + 1] == 1) {
                while (cj < 101 && arr[ci][cj + 1] == 1) {
                    cj++;
                    count++;
                }
            }
            ci++;
            count++;
        }
        return count;
    }

    static void print(int tc, int result){
        StringBuilder sb = new StringBuilder();
        sb.append("#").append(tc).append(" ");
        sb.append(result);

        System.out.println(sb);
    }
}
```

<br>

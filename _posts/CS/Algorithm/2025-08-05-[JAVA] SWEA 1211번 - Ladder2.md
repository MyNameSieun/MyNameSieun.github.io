---
title: "[Algorithm/Java] SWEA 1211번 - Ladder2"
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

- `while (x < 100)` -> 100행(없는 행) 까지 내려가려 함 -> 틀림
- `while (x < 99)` -> 99행에서 정확히 멈춤 -> 정답

![assets/images/2024/SWEA 1211.jpg](<../../../assets/images/2024/SWEA 1211.jpg>)

<br><br>

# 💻 전체 코드

```java
import java.io.*;
import java.util.*;

public class Solution {
    static int[][] arr;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int tc = 10;
        while(tc --> 0){
            int t = Integer.parseInt(br.readLine());

            // 배열 입력 및 초기화
            arr = new int[100][100];
            for(int i=0; i<100; i++){
                StringTokenizer st = new StringTokenizer(br.readLine());
                for(int j=0; j<100; j++){
                    arr[i][j] = Integer.parseInt(st.nextToken());
                }
            }

            // 최단거리 탐색
            int min = Integer.MAX_VALUE;
            int ans = 0;
            int sx = 0, sy = 0;
            for (int j = 0; j < 100; j++) {
                if (arr[0][j] == 1) {
                    sy = j;
                    int val = solve(sx, sy);

                    if(val < min){
                        min = val;
                        ans = j;
                    }
                }
            }

            System.out.print("#" + t + " ");
            System.out.println(ans);
        }
    }

    static int solve(int x, int y){
        int cnt = 0;
        while(x < 99){ // 99 행에서 루프 종료
            // 오른쪽 쭉 이동
            if(y + 1 < 100 && arr[x][y+1] == 1){
                while(y + 1 < 100 && arr[x][y+1] == 1){
                    y++; cnt ++;
                }
            }
            // 왼쪽 쭉 이동
            else if(y - 1 >= 0 && arr[x][y-1] == 1){
                while(y - 1 >= 0 && arr[x][y-1] == 1){
                    y--; cnt ++;
                }
            }
            // 아래로 쭉 이동
            x++; cnt ++;
        }

        return cnt;
    }
}
```

<br>

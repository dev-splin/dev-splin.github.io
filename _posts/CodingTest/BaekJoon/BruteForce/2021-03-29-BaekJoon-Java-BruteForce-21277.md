---
title: "BaekJoon : 21277번(짠돌이 호석)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Brute Force"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Brute Force

BaekJoon Brute Force(브루트포스)  저의 문제풀이 입니다. <br>핵심 부분은 **Bold**해 놓겠습니다!

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다! 



## 21277

DIY(Do It Yourself)는 호석이가 제일 좋아하는 컨텐츠이다. 이번 DIY는 동네 친구 하늘이랑 각자 직소 퍼즐을 하나씩 맞춰보기로 했다. **두 개의 퍼즐은 각자 N1 행 M1 열**과 **N2 행 M2 열의 격자 형태**로 이루어져 있다. 각 격자는 정사각형 모양이며, 퍼즐 조각이 있을 수도 있고, 없을 수도 있다. 즉, 아래 그림도 올바른 퍼즐의 완성 형태이다.

<img src="https://upload.acmicpc.net/df7fb12e-b45f-43ac-87c3-2de7f8672251/-/preview/" alt="img" style="zoom: 25%;" />

성공적으로 DIY가 끝나서 퍼즐이 2개가 완성되었는데, 보관해야 하는 액자를 아직 구매하지 않았다. 그 이유는, 호석이는 엄청난 짠돌이기 때문에 퍼즐 하나마다 액자 하나를 사는 것은 상상도 못하기 때문이다. 액자의 가격은 액자의 넓이**(행의 개수 × 열의 개수)** 로 결정된다. 즉, **퍼즐 두 개를 퍼즐 조각끼리 같은 격자에서 만나지 않도록 배치**해서 **하나의 액자에 담는 방법 중에 가장 적은 비용일 때**를 찾아보자! 단, **각 퍼즐은 90도, 180도, 270도로 자유롭게 회전시켜도 된다.**



### 입력

**첫 줄에 퍼즐의 크기를 나타내는 *N1, M1*이 공백으로 구분**되어 주어진다. 이어서 ***N1*개의 줄에 걸쳐서 완성된 첫 번째 퍼즐의 정보**가 주어진다. **각 행마다 *M1*개의 0 또는 1이 공백없이** 주어진다. **다음 줄에 퍼즐의 크기를 나타내는 *N2, M2*이 공백으로 구분**되어 주어진다. 이어서 ***N2*개의 줄에 걸쳐서 완성된 두 번째 퍼즐의 정보**가 주어진다. **각 행마다 *M2*개의 0 또는 1이 공백없이** 주어진다. **0이 써있는 격자는 퍼즐 조각이 없는 격자**이며 **1은 있는 격자**이다. 두 퍼즐 모두 4개 모서리에 최소 하나의 1은 존재하는 것이 보장된다.

**1 ≤ *N1*, *M1, N2, M2* ≤ 50**

```java
5 5		// 첫 번째 퍼즐의 크기 N1, M1을 입력합니다.
11111	// 여기부터
10000
11111
10000
11111	// 여기까지 N1개의 줄에 M1개의 퍼즐의 정보를 입력합니다.
5 3		// 두 번째 퍼즐
101
101
101
101
111
```



### 출력

두 개의 퍼즐을 담을 수 있는 액자들 중에 **최소 넓이를 출력**한다.

```java
30		// 액자의 최소 넓이를 출력합니다.
```



### 퍼즐을 회전시키는 방법

예를 들어보면 이해가 훨씬 쉽습니다. `3X3배열 (NXM)`이 있다고 가정하고 오른쪽으로 90도 회전시켜 보겠습니다.

```
1 2 3  ->  3 6 9
4 5 6  ->  2 5 8
7 8 9  ->  1 4 7
```

왼쪽 배열을 A, 오른쪽 배열을 B라고 했을 때, 몇 개를 보게되면<br>`A[0][0],A[0][1],A[0][2] -> B[2][0],B[1][0],B[0][0]` <br>`A[1][0],A[1][1],A[1][2] -> B[2][1],B[1][1],B[0][1]` 이 되는 것을 볼 수 있습니다.

`A의 행 -> B의 열`이 되고,<br>`A열의 길이(M) - 해당 열번호 - 1 -> B의 행`이 되는 것을 볼 수 있습니다.<br>**N이 행의 길이, M이 열의 길이, i가 행의 번호, j가 열의 번호**라고 했을 때, 이경우(A를 오른쪽으로 90도 회전)를 간단하게 표현해보면,

`A[i][j] -> B[M-j-1][i]`로 나타낼 수 있습니다. <br>**만약 행열의 시작을 1,1로** 두면 `A[i][j] -> B[M-j+1][i]` 이 됩니다.



### 배열을 이용한 방법

**두 개의 퍼즐이 전부 들어갈만한 배열(액자)을 2개(가운데에 고정된 퍼즐이 있는 배열, 비교퍼즐을 한칸 씩 이동시켜 줄 배열)** 만들어,  **비교퍼즐이 있는 액자에서 비교퍼즐을 돌리고 이동**시키면서 **고정된 퍼즐이 있는 액자 중 고정된 퍼즐이 있는 위치만 비교**하여 액자를 만들 수 있는 **경우의 수의 액자 넓이를 비교해 제일 작은 넓이의 액자를 찾습니다.**

만약 `3X2퍼즐`과 `4X4퍼즐`이 있다고 가정하면, `4X4퍼즐`을 고정시켜 놓고 두 퍼즐이 전부 들어갈만한 배열 `(3+4+3) X (2+4+2) -> 10 X 8배열`을 만듭니다. (중앙에 `4X4퍼즐`이 고정되어 있고 팔방향으로 `3X2퍼즐`이 들어갈 수 있기 때문입니다.)

액자의 넓이는 고정 퍼즐과 비교퍼즐이 액자에 올라가 있을 때,  **`가로(M)` 중 제일 작은 위치, 제일 큰 위치**와 **`세로(N)` 중 제일 작은 위치, 제일 큰 위치를 갱신**해주면서 액자의 최소 넓이를 비교해주는 방법으로 구했습니다.



```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
		
	static Puzzle puzzles[]; 
	
	public static class Puzzle {
		int n;
		int m;
		int puzzleBoard[][];
	}
	
	// 퍼즐을 만드는 메서드
	public static Puzzle makePuzzle() throws Exception{
		String nums = br.readLine();
		StringTokenizer stk = new StringTokenizer(nums);
		
		Puzzle puzzle = new Puzzle();
		
		puzzle.n = Integer.parseInt(stk.nextToken());
		puzzle.m = Integer.parseInt(stk.nextToken());
		puzzle.puzzleBoard = new int [puzzle.n+1][puzzle.m+1];
		
		for (int i = 1; i <= puzzle.n; i++) {
			String puzzleNums = br.readLine();
			
			for (int j = 1; j <= puzzleNums.length(); j++)
				puzzle.puzzleBoard[i][j] = Character.getNumericValue(puzzleNums.charAt(j-1));
			
		}
		
		return puzzle;
	}
	
	// 회전하는 메서드 퍼즐객체를 받아 puzzleBoard를 돌려 새로운 배열을 만들고 바꿔줍니다.
	public static void rotate(Puzzle puzzle) {
		int n = puzzle.m;
		int m = puzzle.n;
		
		int tmpPuzzleBoard[][] = new int[n + 1][m + 1];
		
		for (int i = 1; i <= puzzle.n; i++) {
			for (int j = 1; j <= puzzle.m; j++) 
				tmpPuzzleBoard[puzzle.m + 1 - j][i] = puzzle.puzzleBoard[i][j];
		}
		
		puzzle.puzzleBoard = tmpPuzzleBoard;
		puzzle.n = n;
		puzzle.m = m;
	}
	
	// 액자를 구하는 메서드
	public static void makeFrame() {
		Puzzle basePuzzle;
		Puzzle cmpPuzzle;
		
		// 더 큰 퍼즐을 고정(base)로 합니다.
		if(puzzles[0].n + puzzles[0].m > puzzles[1].n + puzzles[1].m) {
			basePuzzle = puzzles[0];
			cmpPuzzle = puzzles[1];
		}
		else {
			basePuzzle = puzzles[1];
			cmpPuzzle = puzzles[0];
		}
		
		int size = 3000;
		
		// 회전할 수 있는 만큼 반복
		for (int i = 0; i < 4; i++) {
			rotate(cmpPuzzle);
			
			// (비교할 퍼즐 + 고정 퍼즐 + 비교할 퍼즐)로 두 퍼즐이 전부 들어갈만한 배열의 길이를 구합니다.
			int lengthN = basePuzzle.n + cmpPuzzle.n * 2;
			int lengthM = basePuzzle.m + cmpPuzzle.m * 2;
			
			Integer baseFrame[][] = new Integer[lengthN + 1][lengthM + 1];
			
			// 고정된 퍼즐로 N의 최대최소, M의 최대최소를 저장할 변수
			int baseMinN = 1000;
			int baseMaxN = 0;
			int baseMinM = 1000;
			int baseMaxM = 0;
			
			// 고정된 퍼즐을 액자에 위치시켜 주면서 퍼즐의 N의 최대최소, M의 최대최소를 구합니다.
			for (int j = 1; j <= basePuzzle.n; j++)
				for (int k = 1; k <= basePuzzle.m; k++) {
					baseFrame[j+cmpPuzzle.n][k+cmpPuzzle.m] = basePuzzle.puzzleBoard[j][k];
					
					baseMinN = Math.min(baseMinN, j+cmpPuzzle.n);
					baseMaxN = Math.max(baseMaxN, j+cmpPuzzle.n);
					baseMinM = Math.min(baseMinM, k+cmpPuzzle.m);
					baseMaxM = Math.max(baseMaxM, k+cmpPuzzle.m);
				}
				
			// 비교 액자를 순회하기 위한 변수(비교 퍼즐이 한칸 씩 이동할 때 사용)
			int addN = 0;
			int addM = 0;
			
			while(true) {
				
				if(addM + cmpPuzzle.m > lengthM) {
					addM = 0;
					++addN;
				}
				
				if(addN + cmpPuzzle.n > lengthN)
					break;
				
				Integer cmpFrame[][] = new Integer[lengthN + 1][lengthM + 1];
				
				// 액자의 넓이를 구하기 위한 N의 최대최소, M의 최대최소 (고정 퍼즐로 초기화)
				int minN = baseMinN;
				int maxN = baseMaxN;
				int minM = baseMinM;
				int maxM = baseMaxM;
							
				// 비교액자에 비교퍼즐을 만들면서 N의 최대최소, M의 최대최소를 갱신합니다.
				for (int j = 1; j <= cmpPuzzle.n; j++) 
					for (int k = 1; k <= cmpPuzzle.m; k++) {
						cmpFrame[j+addN][k + addM] = cmpPuzzle.puzzleBoard[j][k];

						minN = Math.min(minN, j+addN);
						maxN = Math.max(maxN, j+addN);
						minM = Math.min(minM, k+addM);
						maxM = Math.max(maxM, k+addM);
					}
				
				// 겹치는지 체크
				boolean checkOverlap = false;
				
				for (int j = 1 + cmpPuzzle.n; j <= cmpPuzzle.n + basePuzzle.n; j++) {
					for (int k = 1 + cmpPuzzle.m; k <= cmpPuzzle.m + basePuzzle.m; k++) {
							
						// Integer로 액자를 초기화 했기 때문에 null과 비교, 액자끼리 비교해서 같은 인덱스의 값이 1로 같으면 겹치는 것
						if(cmpFrame[j][k] != null && baseFrame[j][k] == 1 && cmpFrame[j][k] == 1){
							checkOverlap = true;
							break;
						}
					}
					if(checkOverlap)
						break;
				}
				++addM;
				
				if(checkOverlap) {
					continue;
				}
				
				int finalN = maxN - minN + 1;
				int finalM = maxM - minM + 1;
				
				// N과 M을 구해 액자의 최소넓이를 갱신
				size = Math.min(size, finalN * finalM);
			}
		}
		System.out.println(size);
	}
		
	public static void main(String[] args) {
		
		try {
			puzzles = new Puzzle[2];
			
			for (int i = 0; i < 2; i++)
				puzzles[i] = makePuzzle();
				
			makeFrame();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### N과 M(배열의 인덱스)을 하나의 객체로 만드는 방법

배열을 이용하게 되면 고정된 퍼즐을 가운데에 놓아주어야 하는 작업도 필요하고, 순회하는 것도 신경써야 하는 등 여러가지로 직관적이라고 하기엔 애매하게 됩니다. 그래서 [Gravekper](https://www.youtube.com/watch?v=Zt38XizwbAw&t=2445s)님 유튜브를 참고하여 배열의 인덱스를 객체로 만들어 다시 만들어 보았습니다.

이 방법은 배열을 만들지 않고 **배열의 인덱스(N,M)을 가지는 객체**를 하나 만들고 퍼즐의 1에 해당하는 즉, **비어있지 않은 부분(어차피 겹치는 건 비어있지 않은 부분만 체크하기 때문에)만 객체로 만들어 저장**하게 됩니다.

**고정된 퍼즐들 중에서 비교 퍼즐 객체의 속성과 같은 값을 가지는 객체가 있는지 확인**해야 되기 때문에 검색의 시간복잡도가 `O(n)`인  `HashSet`에, 비교 퍼즐의 객체는 `ArrayList`에 저장합니다. 그 후, **고정 퍼즐을 그대로 둔 상태(0,0)에서 비교 퍼즐에 -50 ~ 50(최대 크기가 50이기 때문)까지 더해서 비교**하면 조금 더 직관적이고 손쉽게 문제를 해결할 수 있습니다.<br><u>이 때, 고정된 퍼즐의 `HashSet`에서 비교할 때 같은 N,M을 가지는 객체는 같은 객체로 판단해야 하기 때문에 `hashcode()`와 `equals()`를 오버라이드 해주어야 합니다.</u>



```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;
import java.util.StringTokenizer;

public class Main {
	static InputStreamReader isr = new InputStreamReader(System.in);
	static BufferedReader br = new BufferedReader(isr);
	
	// 고정된 퍼즐의 N의 최대최소, M의 최대최소를 저장할 변수
	static int minBaseN = 1000;
	static int maxBaseN = 0;
	static int minBaseM = 1000;
	static int maxBaseM = 0;
	
	static Set<Point> base;
	static List<Point> cmp;
		
	// 배열의 인덱스(N,M)을 속성으로 가지는 객체
	// HashSet에서 같은 객체의 판별을 하기 위해 hashCode()와 equals()를 오버라이딩 합니다.
	public static class Point {
		int n;
		int m;
		
		public Point() {}
		public Point(int n, int m) {
			this.n = n;
			this.m = m;
		}

		@Override
		public int hashCode() {
			final int prime = 31;
			int result = 1;
			result = prime * result + n;
			result = prime * result + m;
			return result;
		}
        
		@Override
		public boolean equals(Object obj) {
			if (this == obj)
				return true;
			if (obj == null)
				return false;
			if (getClass() != obj.getClass())
				return false;
			Point other = (Point) obj;
			if (n != other.n)
				return false;
			if (m != other.m)
				return false;
			return true;
		}
	}
	
	// 퍼즐을 만드는 메서드
	public static void makePuzzle() throws Exception{
		String nums = br.readLine();
		StringTokenizer stk = new StringTokenizer(nums);
				
		int n = Integer.parseInt(stk.nextToken());
		int m = Integer.parseInt(stk.nextToken());
		
		base = new HashSet<>();
		
		for (int i = 0; i < n; i++) {
			String puzzleNums = br.readLine();
			
			// 값이 1에 해당하는 즉, 비어있지 않은 퍼즐만 객체로 만들어 저장합니다.
			for (int j = 0; j < m; j++) {
				if(puzzleNums.charAt(j) == '1') {
					Point point = new Point(i,j);
					base.add(point);
					minBaseN = Math.min(minBaseN, i);
					maxBaseN = Math.max(maxBaseN, i);
					minBaseM = Math.min(minBaseM, j);
					maxBaseM = Math.max(maxBaseM, j);
				}
			}
		}
		
		nums = br.readLine();
		stk = new StringTokenizer(nums);
				
		n = Integer.parseInt(stk.nextToken());
		m = Integer.parseInt(stk.nextToken());
		
		cmp = new ArrayList<>();
		
		for (int i = 0; i < n; i++) {
			String puzzleNums = br.readLine();
			
			for (int j = 0; j < m; j++) {
				if(puzzleNums.charAt(j) == '1') {
					Point point = new Point(i,j);
					cmp.add(point);
				}
			}
		}
	}
	
	
	public static void rotate() {
		int maxM = 0;
		// ArrayList는 배열처럼 따로 행과열의 길이가 없기 때문에 M의 최대값을 찾아줍니다.
		for(Point point : cmp) {
			if(maxM < point.m)
				maxM = point.m;
		}
		
		// 시작인덱스를 0으로 잡았고,
		// 배열로 생각했을 때 길이가 3이면 M의 최대값은 2가 나오기 때문에 maxM - point.m의 값으로 회전을 시켜줄 수 있게 됩니다. 
		// 기존 회전하는 공식인 M-j+1에서 -1을 하게 되기 때문
		for(Point point : cmp) {
			int n = point.n;
			point.n = maxM - point.m;
			point.m = n;
		}
	}
	
	public static void makeFrame() {
		
		int size = 3000;
				
		for (int k = 0; k < 4; k++) {
			
			// -50 ~ 50까지 순회
			for (int i = -50; i < 50; i++) {
				for (int j = -50; j < 50; j++) {
					
					boolean isImpossible = true;
					
					int minN = minBaseN;
					int maxN = maxBaseN;
					int minM = minBaseM;
					int maxM = maxBaseM;
					
					// 비교 퍼즐을 순회하면서 n과 m에 i와 j를 더해 마치 배열을 움직여 비교하는 것처럼 할 수 있습니다.
					for(Point point : cmp) {
						int n = point.n + i;
						int m = point.m + j;
						Point cmpPoint = new Point(n, m);
						
						// 객체에서 n과 m을 이용해 hashcode를 만들어 주었기 때문에 n,m 값이 같으면 같은 객체로 인식하게 됩니다. 
						if(base.contains(cmpPoint)) {
							isImpossible = false;
							break;
						}
						
						minN = Math.min(minN, n);
						maxN = Math.max(maxN, n);
						minM = Math.min(minM, m);
						maxM = Math.max(maxM, m);
					}
					
					// 1이 겹치지 않는 상황 즉, 가능한 상황만 사이즈를 구해 비교합니다.
					if(isImpossible)
						size = Math.min(size, (maxN - minN + 1) * (maxM - minM + 1));
				}
			}
			
			// 회전
			rotate();
		}
		
							
		System.out.println(size);
	}
		
	public static void main(String[] args) {
		
		try {
			makePuzzle();
				
			makeFrame();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.


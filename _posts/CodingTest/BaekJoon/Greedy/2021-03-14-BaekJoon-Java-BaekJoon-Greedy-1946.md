---
title: "BaekJoon : 1946번(신입사원)"
excerpt_separator: <!--more-->
categories:
  - "Coding Test"
  - BaekJoon
tags:
  - "Coding Test"
  - BaekJoon
  - "BaekJoon : Greedy"
toc: true
toc_sticky: true
toc_label: 목차
---

# Java : BaekJoon Greedy

BaekJoon Greedy  저의 문제풀이 입니다. 

혹시 더 좋은 방법 알려주신다면 정말 감사하겠습니다!



## 1946

언제나 최고만을 지향하는 굴지의 대기업 진영 주식회사가 신규 사원 채용을 실시한다. 인재 선발 시험은 1차 서류심사와 2차 면접시험으로 이루어진다. 최고만을 지향한다는 기업의 이념에 따라 그들은 최고의 인재들만을 사원으로 선발하고 싶어 한다.

그래서 진영 주식회사는, 다른 모든 지원자와 비교했을 때 **서류심사 성적과 면접시험 성적 중 적어도 하나가 다른 지원자보다 떨어지지 않는 자만 선발**한다는 원칙을 세웠다. 즉, 어떤 지원자 A의 성적이 다른 어떤 지원자 B의 성적에 비해 **서류 심사 결과와 면접 성적이 모두 떨어진다면 A는 결코 선발되지 않는다.**

이러한 조건을 만족시키면서, 진영 주식회사가 이번 신규 사원 채용에서 선발할 수 있는 신입사원의 최대 인원수를 구하는 프로그램을 작성하시오.



첫째 줄에는 테스트 케이스의 개수 T(1 ≤ T ≤ 20)가 주어진다. 각 테스트 케이스의 첫째 줄에 지원자의 숫자 N(1 ≤ N ≤ 100,000)이 주어진다. 둘째 줄부터 N개 줄에는 각각의 지원자의 서류심사 성적, 면접 성적의 순위가 공백을 사이에 두고 한 줄에 주어진다. 두 성적 순위는 **모두 1위부터 N위까지 동석차 없이 결정**된다고 가정한다.

```java
2		// 테스트 케이스 수를 입력합니다.
5		// 지원자의 수를 입력합니다.
3 2
1 4
4 1
2 3
5 5		// 첫 번째 테스트 케이스 지원자들의 서류,면접성적을 입력합니다.
7		// 두 번째 테스트 케이스 지원자 수를 입력합니다.
3 6
7 3
4 2
1 4
5 7
2 5
6 1		// 두 번째 테스트 케이스 지원자들의 서류,면접성적을 입력합니다.
4		// 첫 번째 테스트 케이스에서 선발된 지원자 수를 출력합니다. 
3		// 두 번째 테스트 케이스에서 선발된 지원자 수를 출력합니다.
```



### 내코드

면접이 1등(서류6,면접1)인 지원자와 서류가 1등(서류1,면접4)인 지원자 중 체크 개수가 더 적은 쪽을 기준(서류1,면접4)으로 정렬하고 지원자를 찾는 방법으로 풀었습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
	
	// 지원자 클래스, 면접등급과 인터뷰등급을 가지고 있습니다.
	public static class Applicant {
		int documentGrade;
		int interviewGrade;
		
		public Applicant() {}
		public Applicant(int documentGrade, int interviewGrade) {
			this.documentGrade = documentGrade;
			this.interviewGrade = interviewGrade;
		}		
	}
	
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int t = Integer.parseInt(br.readLine().trim());
			// 합격자 수를 저장할 배열
			int newRecruitNums[] = new int[t];
			
			// 입력한 t(테스트 케이스) 만큼 반복합니다.
			for (int i = 0; i < t; i++) {
				int n = Integer.parseInt(br.readLine().trim());
				
				// 지원자 객체들을 담을 배열
				Applicant applicants[] = new Applicant[n];
				
				// 면접과 서류 각각 1등인 지원자를 저장해 줄 변수
				Applicant firstPlaceInterview = null;
				Applicant firstPlaceDocument = null;
				
				
				// 지원자들의 면접,서류 등수를 입력하고 배열에 저장한 뒤, 면접/서류 각각 1등을 변수에 저장합니다.
				for (int j = 0; j < n; j++) {
					String grades = br.readLine().trim();
					StringTokenizer stk = new StringTokenizer(grades);
					
					int documentGrade = Integer.parseInt(stk.nextToken());
					int interviewGrade = Integer.parseInt(stk.nextToken());
					
					applicants[j] = new Applicant(documentGrade, interviewGrade);
					
					if(documentGrade == 1)
						firstPlaceDocument = applicants[j];
					
					if(interviewGrade == 1)
						firstPlaceInterview = applicants[j];
				}
				
				// 만약 어떤 지원자가 서류 1등 면접 4등이라면 나머지 합격할 수 있는 지원자는 면접 1,2,3등 중에 있습니다.
				// 그러므로 면접 1,2,3 등만 체크하면 되고, 이 때 cmpCount(비교할 숫자)는 4가 됩니다.(1등 포함)
				int cmpCount = 0;
				
				// 만약 "서류 1등 면접 4등인 A"와 "서류 5등 면접 1등 B"가 있을 때 A를 4번 체크하는게 B를 5번 체크 하는 것 보다 빠릅니다.
				// A를 체크하게 되면 면접 기준으로 오름차순 정렬 한 후 4번만 체크하게 됩니다.
				// 이 때, 서류기준으로 정렬했는 지, 면접기준으로 정렬했는 지 체크해주는 boolean변수입니다.
				boolean sortByDocumentOrInterview = true;
				
				// 위의 A,B로 예를 들면, A(서류 1등)의 면접 4등과 B(면접 1등)의 서류 5등을 비교해 더 적게 비교할 수 있는 경우를 찾은 후,
				// 해당 기준으로 정렬해주고(람다식 이용해서) cmpcount를 설정해주고 어떤 기준으로 정렬했는 지(sortByDocumentOrInterview) 체크합니다.
				if(firstPlaceDocument.interviewGrade > firstPlaceInterview.documentGrade) {
					Arrays.sort(applicants,(a,b)->a.documentGrade - b.documentGrade);
					cmpCount = firstPlaceInterview.documentGrade;
					sortByDocumentOrInterview = true;
				}
				else {
					Arrays.sort(applicants,(a,b)->a.interviewGrade - b.interviewGrade);
					cmpCount = firstPlaceDocument.interviewGrade;
					sortByDocumentOrInterview = false;
				}	
				
				// A를 기준으로 (서류1,면접4) 정렬하게 되면 면접1,2,3,4만 체크하게 되는데 이 때 면접 1도 무조건 합격입니다.
				// 이 때, 면접1의 서류 등수를 저장할 변수입니다. 서류 등수를 저장하는 이유는 만약 면접 1의 서류등수가 2등이라면
				// 면접2,3의 서류등수와 비교하면 면접1,서류2를 면접2,3이 어떤 서류등수가 와도 합격하지 못하기 때문에(면접4는 서류가1등)
				// 서류등수를 비교할 변수가 필요합니다.
				int cmpNum = n + 1;
				// 합격자 수를 저장할 변수
				int newRecruitCount = 0;
				
				// 서류기준 정렬이면 면접을 비교하고, 면접기준 정렬이면 서류를 비교한 후
				// cmpNum(비교할 변수) 보다 빠른 등수면 합격이기 때문에 합격자수를 늘려줍니다.
				// 비교할 변수는 n+1로 초기화 되있기 때문에 1등은 합격되고 1등의 면접/서류 등수가 cmpNum에 들어갑니다.
				for (int j = 0; j < cmpCount; j++) {
					if(sortByDocumentOrInterview) {
						if(cmpNum > applicants[j].interviewGrade) {
							cmpNum = applicants[j].interviewGrade;
							++newRecruitCount;
						}
					} else {
						if(cmpNum > applicants[j].documentGrade) {
							cmpNum = applicants[j].documentGrade;
							++newRecruitCount;
						}
					}
				}
				
				newRecruitNums[i] = newRecruitCount;
			}
			
			// 합격자 수 출력
			for(int newRecruitNum : newRecruitNums) {
				bw.write(Integer.toString(newRecruitNum));
				bw.newLine();
			}
				
			bw.flush();
			bw.close();
			
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```



### 더 좋은 방법

구글링해서 더 좋은 방법으로 다시 구현해보았습니다. 핵심은 배열의 인덱스로 서류등수를 넣고 해당 인덱스에 면접등수를 넣는 것입니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
		
	public static void main(String[] args) {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		try {
			int t = Integer.parseInt(br.readLine().trim());
			int newRecruitNums[] = new int[t];
			
			for (int i = 0; i < t; i++) {
				int n = Integer.parseInt(br.readLine().trim());	
				// 지원자를 저장할 배열(인덱스는 서류등수)
				int applicants[] = new int[n]; 
				
				// [서류 등수 인덱스]에 면접등수를 저장합니다.
				for (int j = 0; j < n; j++) {
					String grades = br.readLine().trim();
					StringTokenizer stk = new StringTokenizer(grades);
					
					int documentGrade = Integer.parseInt(stk.nextToken());
					int interviewGrade = Integer.parseInt(stk.nextToken());
					
					applicants[documentGrade-1] = interviewGrade;
				}
				
				// 서류 1등은 무조건 합격이기 때문에 서류 1등의 면접등수를 비교할 면접등수로 잡습니다.
				int cmpInterview = applicants[0];
				// 서류 1등합격이 있기 때문에 1로 초기화 해줍니다. 
				int newRecruitCount = 1;
				
				// cmpInterview 면접등수와 비교해서 해당 인덱스(서류등수)의 면접등수가 더 낮으면 cmpInterview를 교체해주고
				// 합격자수를 늘려줍니다.
				for (int j = 1; j < applicants.length; j++) {
					if(cmpInterview > applicants[j]) {
						cmpInterview = applicants[j];
						++newRecruitCount;
					}
				}
				
				newRecruitNums[i] = newRecruitCount;
			}
			
			// 합격자 수 출력
			for(int newRecruitNum : newRecruitNums) {
				bw.write(Integer.toString(newRecruitNum));
				bw.newLine();
			}
				
			bw.flush();
			bw.close();
			
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

참고 : [https://velog.io/@yeoj1n/%EB%B0%B1%EC%A4%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98](https://velog.io/@yeoj1n/%EB%B0%B1%EC%A4%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

[https://plplim.tistory.com/39](https://plplim.tistory.com/39)

## 

---

해당 코드들은 [저의 GitHub](https://github.com/dev-splin/Coding-Test)에서 확인할 수 있습니다.
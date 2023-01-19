---
title: 리트코드(leetcode) 13번 문제 Roman To Integer
date: 2023-01-19 11:33:34 +09:00
categories: [CodingTest, LeetCode]
tags: [CodingTest, LeetCode]
---

<br/>

> ## [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

<br/>

문자열로 된 로마 숫자를 아라비아 숫자로 변환하는 문제이다.

<br/>

> ### 문제 내용



<br/><br/>

문제에도 나와있듯 I, X, C 같은 로마 숫자는 특정 로마 숫자 뒤에 올 때 다음 숫자에서 값을 빼줘야 한다.

그래서 I, X, C 의 경우에는 임시 변수에 저장해주고 숫자를 계산해줬다. 

먼저 전달 받은 로마 숫자 문자열을 char 배열 형태로 변환한 후

반복문을 통해 요소에 하나 씩 접근해줬다.



또 시간을 줄이기 위해 switch 문을 이용하여 조건이 맞을 시 바로 loop를 넘겼다.

<br/><br/>


> ### 답안 보기
  
<br/>
  
```java

class Solution {
    public int romanToInt(String s) {
        
        char[] arr = s.toCharArray();
		int answer = 0;
		char temp = '0';
		
		for(int i=0; i<arr.length; i++) {
			switch(arr[i]) {
			
				case 'I': 
					answer += 1;
                    
                    // 임시 변수에 저장
					temp='I';
					break;
					
				case 'V':
                    // 임시 변수가 I일 때와 아닐 때 값을 다르게 추가
					if(temp == 'I') answer += 3;
					else answer += 5;
					temp = 'V';
					break;
					
				case 'X':
					if(temp == 'I') answer += 8;
					else answer += 10;
					temp = 'X';
					break;
					
				case 'L':
					if(temp == 'X') answer += 30;
					else answer += 50;
					temp = 'L';
					break;
					
				case 'C':
					if(temp == 'X') answer += 80;
					else answer += 100;
					temp = 'C';
					break;
					
				case 'D':
					if(temp == 'C') answer += 300;
					else answer += 500;
					break;
					
				case 'M':
					if(temp == 'C') answer += 800;
					else answer += 1000;
					break;
			}
		}
		
        return answer;
    }
}
                               

```


<br/><br/>

확실히 for문만 쓰는 것보다 switch문으로 break를 걸어주었더니 속도가 빨라지는 것을 느낄 수 있었다.



<img src="/assets/img/codingtest/roman-to-integer.jpg"/>

<br/><br/>






















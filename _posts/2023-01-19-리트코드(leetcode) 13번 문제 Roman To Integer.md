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

<h2><a href="https://leetcode.com/problems/roman-to-integer/">13. Roman to Integer</a></h2><h3>Easy</h3><hr><div><p>Roman numerals are represented by seven different symbols:&nbsp;<code>I</code>, <code>V</code>, <code>X</code>, <code>L</code>, <code>C</code>, <code>D</code> and <code>M</code>.</p>

<pre><strong>Symbol</strong>       <strong>Value</strong>
I             1
V             5
X             10
L             50
C             100
D             500
M             1000</pre>

<p>For example,&nbsp;<code>2</code> is written as <code>II</code>&nbsp;in Roman numeral, just two ones added together. <code>12</code> is written as&nbsp;<code>XII</code>, which is simply <code>X + II</code>. The number <code>27</code> is written as <code>XXVII</code>, which is <code>XX + V + II</code>.</p>

<p>Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not <code>IIII</code>. Instead, the number four is written as <code>IV</code>. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as <code>IX</code>. There are six instances where subtraction is used:</p>

<ul>
	<li><code>I</code> can be placed before <code>V</code> (5) and <code>X</code> (10) to make 4 and 9.&nbsp;</li>
	<li><code>X</code> can be placed before <code>L</code> (50) and <code>C</code> (100) to make 40 and 90.&nbsp;</li>
	<li><code>C</code> can be placed before <code>D</code> (500) and <code>M</code> (1000) to make 400 and 900.</li>
</ul>

<p>Given a roman numeral, convert it to an integer.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre><strong>Input:</strong> s = "III"
<strong>Output:</strong> 3
<strong>Explanation:</strong> III = 3.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre><strong>Input:</strong> s = "LVIII"
<strong>Output:</strong> 58
<strong>Explanation:</strong> L = 50, V= 5, III = 3.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre><strong>Input:</strong> s = "MCMXCIV"
<strong>Output:</strong> 1994
<strong>Explanation:</strong> M = 1000, CM = 900, XC = 90 and IV = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 15</code></li>
	<li><code>s</code> contains only&nbsp;the characters <code>('I', 'V', 'X', 'L', 'C', 'D', 'M')</code>.</li>
	<li>It is <strong>guaranteed</strong>&nbsp;that <code>s</code> is a valid roman numeral in the range <code>[1, 3999]</code>.</li>
</ul>
</div>


<br/><br/>

문제에도 나와있듯 I, X, C 같은 로마 숫자는 특정 로마 숫자 뒤에 올 때 다음 숫자에서 값을 빼줘야 한다.

그래서 I, X, C 의 경우에는 임시 변수에 저장해주고 숫자를 계산해줬다. 

먼저 전달 받은 로마 숫자 문자열을 char 배열 형태로 변환한 후

반복문을 통해 요소에 하나 씩 접근해줬다.



또 시간을 줄이기 위해 switch 문을 이용하여 조건이 맞을 시 바로 loop를 넘겼다.

<br/><br/>


> ### 답안 보기
  
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






















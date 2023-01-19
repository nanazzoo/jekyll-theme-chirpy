---
title: 2023-01-19-리트코드(leetcode) 9번 문제 palindrome-number
date: 2023-01-19 11:18:30 +09:00
categories: [CodingTest, LeetCode]
tags: [CodingTest, LeetCode, palindrome]
---

<br/>

> ## [Palindrome Number(숫자 회문)](https://leetcode.com/problems/palindrome-number/)

<br/>

<br/>

주어진 숫자를 뒤집었을 때에도 같은 숫자일 때에만 true, 아닐 때에는 false를 반환 시키는 문제이다.

<br/>

<br/>

---

> ### 문제 내용

<h3>Easy</h3><hr><div><p>Given an integer <code>x</code>, return <code>true</code><em> if </em><code>x</code><em> is a </em><span data-keyword="palindrome-integer"><em><strong>palindrome</strong></em></span><em>, and </em><code>false</code><em> otherwise</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre><strong>Input:</strong> x = 121
<strong>Output:</strong> true
<strong>Explanation:</strong> 121 reads as 121 from left to right and from right to left.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre><strong>Input:</strong> x = -121
<strong>Output:</strong> false
<strong>Explanation:</strong> From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre><strong>Input:</strong> x = 10
<strong>Output:</strong> false
<strong>Explanation:</strong> Reads 01 from right to left. Therefore it is not a palindrome.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>-2<sup>31</sup>&nbsp;&lt;= x &lt;= 2<sup>31</sup>&nbsp;- 1</code></li>
</ul>

<p>&nbsp;</p>
<strong>Follow up:</strong> Could you solve it without converting the integer to a string?</div>

---



<br/><br/>

Java에서 숫자를 뒤집는 방법 중 하나는 숫자를 문자열로 변환하고, 그 문자열을 또 char형 배열로 변환하여 반복문을 통해 뒤집는 것이다.

<br/><br/>

> ### 답안 보기

```java
class Solution {
    public boolean isPalindrome(int x) {
        
        // 숫자를 문자열로 형변환
        String str = String.valueOf(x);
        
        // 변환된 문자열을 다시 char 배열로 형변환
		char[] arr = str.toCharArray();
        
        // 뒤집은 결과물을 담을 배열을 하나 선언(기존 배열의 길이와 같게)
		char[] arr2 = new char[arr.length];
		
        // 반복문을 통해 배열에 역순으로 요소를 집어넣음
		for(int i=0; i<arr.length; i++) {
			char temp = arr[i];
			arr2[i] = arr[arr.length-i-1];
		}
		
        // 뒤집은 배열을 문자열로 변환하여
		String str2 = String.valueOf(arr2);

        // 같으면 true, 다르면 false를 반환해준다.
        if(str.equals(str2)) return true;
        else return false;
    }
}
```

<br/><br/>

> 값을 int 형으로 다시 변환하여 비교할 시 -기호 때문에 예외가 발생함. 문자열 형태로 비교해야 정확한 비교가 가능.

{: .prompt-tip }

<br/><br/>






















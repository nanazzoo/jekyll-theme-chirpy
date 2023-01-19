---
title: [Java] 배열(Array)의 문법과 응용
date: 2023-01-18 20:01:00 +09:00
categories: [Backend, Java]
tags: [Java, Array]
---



> ### 배열
>
> 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것



<img src="/assets/img/java/javaarray.jpg" width="60%" alt="javaarray">



1. 배열을 구성하는 각각의 값을 요소(element)라고 하며, 배열의 값의 위치를 가리키는 숫자를 인덱스(index)라고 한다.
2. 인덱스를 사용하여 값에 바로 접근 가능
3. 새로운 값을 삽입하거나 특정 인덱스에 있는 값을 삭제하기 어렵다. 값을 삽입하거나 삭제하려면 해당 인덱스 주변에 있는 값을 이동시키는 과정이 필요하다.
4. 배열의 크기는 선언할 때 지정할 수 있으며, **한 번 선언하면 크기를 늘리거나 줄일 수 없다.**
5. 구조가 간단하므로 코딩 테스트에서 많이 사용한다

<br/>

<br/>

### 배열의 선언

위에서 말한 것처럼 배열은 처음 선언할 때 미리 공간의 개수(길이)를 지정해줘야 한다.

자바에서 배열은 간단한 구조를 가지고 있지만, 그만큼 새로운 요소를 넣거나 삭제하는 데에는 어려움이 따른다.




```java
// int 형 배열의 선언 & 초기화
int[] nums = new int[3]; // int 타입의 값 3개가 저장될 빈 배열 생성
// 각 빈 공간에 값을 초기화, (인덱스 번호는 0부터 시작)
nums[0] = 1;
nums[1] = 2;
nums[2] = 3;

// 배열의 초기화 방법에는 for문으로 값을 넣어주는 방법도 있다.
for(int i=1; i<nums.length; i++) {
    nums[i] = i;
}

// 선언과 동시에 초기화
int[] nums2 = {1, 2, 3};
```

<br/>

<br/>

### 배열의 출력

배열을 출력하기 위해 System.out.println()을 사용하게 되면 이상한 값이 출력 되는데, 이는 메모리 영역에 있는 배열의 주소 값을 가리키는 것이므로 for문을 이용하여 배열의 각 요소에 접근하여 출력하거나, 자바에서 제공해주는 Arrays.toString() 메서드를 이용하여 출력할 수 있다.



> Arrays 클래스에 있는 메서드를 사용하기 위해서는 반드시 java.util.Arrays 패키지를 inport해주어야 한다.
{: .prompt-info }



```java
import java.util.Arrays;

class Test{
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        
        // for문을 이용한 배열 출력
        for(int i=0; i<arr.length; i++) {
            System.out.println(arr[i]);
        }
        
        // 결과
        // 1
        // 2
        // 3
        // 4
        // 5
        
        
        // Arrays.toString() 메서드를 이용한 배열 출력
        System.out.println(Arrays.toString(arr));
        
        // 결과
        // [1, 2, 3, 4, 5]
        
        
    }
}
```



>  char 형 같은 문자열 배열은 println 구문으로 바로 출력이 가능하다.
{: .prompt-tip}

<br/>

<br/>
























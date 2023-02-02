---
title: 알고리즘(Algorithm) 순환, 정렬
date: 2023-02-02 17:21:00 +09:00
categories: [전산 기초, 알고리즘]
tags: [알고리즘, 재귀 함수, 정렬]

---

<br><br>



# **알고리즘(Algorithm)**

<br><br>

## **1. 순환(Recursion, 재귀함수)**

자기 자신을 호출(재참조)하는 함수(메서드)전달되는 상태인 매개변수가 달라질 뿐 똑같은 일을 하는 함수

```java
public class main {
 public static void main(String[] args) {
  func();
 }

 public static void func() {
  system.out.println("hello");
  func();
 }
}
```



<br>



위의 코드처럼 자기 자신을 아무런 조건 없이 호출하게 되면 **무한 루프**에 빠질 수 있다.

**무한 루프에 빠지지 않으려면 어떻게 해야할까?**



<br>



```java
public class main {
  public static void main(String[] args) {
    int n = 4;
    func(n);
  }
  public static void func(int k) {
    if(k <= 0)
      return;
    else {
      System.out.println("hello");
      func(k-1);
    }
  }
}
```

<br>



위의 코드처럼 **매개변수**를 주고, **조건**을 정하여 호출하게 되면 무한루프에 빠지는 상황을 방지할 수 있다. 

이와 같이 무한 루프에 빠지지 않을 조건을 **Base case(기반조건)**라고 한다.

재귀함수를 사용할 때에는 적어도 **하나의 기반조건이 존재**해야 한다.

함수 안에서 자기 자신을 호출하는 구문, 위에서는 `func(k-1);`에 해당하는 부분을 Recursive case라고 하는데, Recursive case를 반복할 때에는 결국 **기반조건으로 수렴**해야 한다.



<br>



예제를 통해 이해해보자.

```java
public class main {
  public static void main(String[] args) {
    int result = func(4);
    System.out.println(result);
  }
  public static int func(int n) {
    if (n==0)
      return 0;
    else
      return n + func(n-1);
  }
}
```



<br>



위의 함수를 실행해보면 결과가 **10**이 나온다.

왜 결과가 10일까?



<br>



<img src="/assets/img/cs/Recursion.png">

<br>



**답은 간단하다.**

처음 호출 시 n의 값은 4였다. 

다음 호출 식에서 n + func(n-1)을 호출했기 때문에 4 + func(3)이 된다. 

기반 조건인 n==0이 될 때까지 반복하면 답이 10이 된다.



<br>



**정리**: func(int n)은 음이 아닌 정수 n에 대해서 0에서 n까지의 합을 올바로 계산한다.

**증명**:

- **n=0**인 경우 **0을 반환**한다. 올바르다.

- 임의의 양의 정수 k에 대해서 **n<k**인 경우 **0에서 n까지의 합을 올바르게 계산하여 반환**한다고 가정하자.

- **n=k**인 경우: func은 먼저 func(k-1) 호출하는데 2번의 가정에 의해서 **0에서 k-1까지의 합이올바로 계산되어 반환**된다. 메서드 func은 **그 값에 n을 더해서 반환**하므로 결국 **0에서 k까지의 합**을 올바로 계산하여 반환한다.

<br><br>

---



### **Factorial: n!**

일반적으로 재귀함수는 Factorial을 계산할 때 많이 쓰인다.

<br>

<div align="center">
    0! = 1 <br>
1! = 1<br>
n! = n*(n-1)!<br>
</div>



<br>

```java
public static int factorial(int n) {
    if (n==0 || n==1)
      return 1;
    else
      return n*factorial(n-1);
  }
```



<br>



위의 메서드처럼 n이 0이나 1이면 1을 반환, 1보다 크면 n*n-1!을 해준다.

만약 factorial(4)를 호출하면 답은 24가 나온다.

<br>



<img src="/assets/img/cs/factorial.png">





<br>

**정리**: factorial(int n)은 음이 아닌 정수 n에 대해서 n!을 올바로 계산한다.

**증명**:

* n= 0 또는 1인 경우 1을 반환한다.

* 임의의 양의 정수 k에 대해서 n<k인 경우 n!을 올바르게 계산한다고 가정하자.

* n=k인 경우를 고려해보자. factorial은 먼저 factorial(k-1) 호출하는데 2번의 가정에 의해서 (k-1)!이 계산되어 반환된다. 따라서 메서드 factorial은 k*(k-1)!=k!을 반환한다.


<br><br>

---

<br>

### **시간복잡도란?**

시간복잡도는 문제를 해결하는 데 걸리는 시간과 입력의 함수 관계를 가리킨다. 어떠한 알고리즘의 로직이 얼마나 오랜 시간이 걸리는 지 나타내는 데 쓰이며 보통 **빅오 표기법**으로 나타낸다.



<br>



#### **빅오 표기법(Big O)**

입력값에 따라 연산을 수행할 때, 연산 횟수에 비해 시간이 얼마나 걸리는가를 표기하는 방법

시간 복잡도의 최악을 고려한 방법이다.



<br>



#### **Big-O 표기법의 종류**

* O(1)
* O(n)
* O(log n)
* O(n^2) 등



<img src="/assets/img/cs/Big-O-Complexity-Chart.png">



---



<br><br>



## **2. 정렬(Sort)**

* 거품 정렬(Bubble Sort)
* 선택 정렬(Selection Sort)
* 삽입 정렬(insertion Sort)
* 퀵 정렬(Quick Sort)
* 병합 정렬(Merge Sort)
* 힙 정렬(Heap Sort)
* 기수 정렬(Radix Sort)
* 계수 정렬(Counting Sort)

<br><br>

### **a.** **거품** **정렬(Bubble Sort)**

거품 정렬은 선택 정렬(Selection Sort)와 유사한 개념으로 **서로 인접한 두 원소의 대소를 비교하고, 조건에 맞지 않다면 자리를 교환하며 정렬하는 알고리즘**이다.



<img src="/assets/img/cs/bubble-sort-001.gif">



1. 1회전에 첫 번째 원소와 두 번째 원소를, 두 번째 원소와 세 번째 원소를 … 이런 식으로 마지막 원소까지 **비교하여 조건에 맞지 않는다면 서로 자리를 교환**한다.
2. 1회전이 끝나면 가장 큰 원소가 마지막 자리로 이동하므로 2회전에서는 마지막 원소는 제외되고 3회전에서는 마지막 + 1앞 원소까지 제외… 모든 원소가 정렬되면 회전이 끝난다.



<br>



#### **Java Code**

```java
void bubbleSort(int[] arr) {
  int temp = 0;
	for(int i = 0; i < arr.length; i++) {    // 1.
		for(int j= 1 ; j < arr.length-i; j++) { // 2.
			if(arr[j-1] > arr[j]) {       // 3.
        // swap(arr[j-1], arr[j])
				temp = arr[j-1];
				arr[j-1] = arr[j];
				arr[j] = temp;
			}
		}
	}
	System.out.println(Arrays.toString(arr));
}
```

1. 제외될 원소의 개수
2. 원소를 비교할 index를 의미, **j는 현재원소**를 가리키고 **j-1은 이전 원소**를 가리키므로 j는 1부터 시작한다.
3. 현재 가리키고 있는 두 원소를 비교하여 자리를 교환한다.




<br>



**시간 복잡도**: O(n^2)

**장점**

* 구현이 매우 간단하고 소스코드가 직관적

* 정렬하고자 하는 배열 안에서 교환하는 방식이므로 다른 메모리 공간을 필요로 하지 않는다

* 안정 정렬(Stable Sort)이다.

  

**단점**

* 시간 복잡도가 O(n^2)으로 굉장히 비효율적이다.
* 정렬 돼있지 않은 원소가 정렬 됐을 때의 자리로 가기 위해서 교환 연산(swap)이 많이 일어나게 된다.



<br><br><br><br>

### **b.** **선택** **정렬(Selection Sort)**

선택 정렬은 거품 정렬과 유사한 알고리즘으로, **해당 순서에 원소를 넣을 위치는 이미 정해져 있고, 어떤 원소를 넣을지 선택**하는 알고리즘이다.

선택 정렬은 삽입 정렬과 다르며 배열에서 **해당 자리를 선택하고 그 자리에 오는 값을 찾는 것**이라고 생각하면 편하다.



<img src="/assets/img/cs/selection-sort-001.gif">



1. 주어진 배열 중에 **최소값**을 찾는다.
2. 그 값을 **맨 앞에 위치한 값과 교체**한다.
3. 맨 처음 위치를 뺀 나머지 배열을 같은 방법으로 교체한다.



<br>



#### **Java Code**

```java
void selectionSort(int[] arr) {
  int indexMin, temp;
  for (int i = 0; i < arr.length-1; i++) {    // 1.
    indexMin = i;
    for (int j = i + 1; j < arr.length; j++) { // 2.
      if (arr[j] < arr[indexMin]) {      // 3.
        indexMin = j;
      }
    }
    // 4. swap(arr[indexMin], arr[i])
    temp = arr[indexMin];
    arr[indexMin] = arr[i];
    arr[i] = temp;
 }
 System.out.println(Arrays.toString(arr));
}
```

1. 위치(index)를 선택해준다.

2. i+1번째 원소부터 선택한 index의 값과 비교한다.

3. 오름차순이므로 현재 선택한 자리에 있는 값보다 순회하고 있는 값이 작다면 index를 갱신해준다.

4. ‘2’번 반복문이 끝난 후에 indexMin에 선택한 위치에 들어가야 하는 값의 위치가 들어있으므로 서로 교환(swap)해준다.




<br>



**시간복잡도**: O(n^2)

**장점**

* Bubble Sort와 마찬가지로 알고리즘이 단순하다.

* 정렬을 위한 비교 횟수는 많지만, 실제 교환하는 횟수는 적기 때문에 많은 교환이 일어나야 하는 자료 상태에서 비교적 효율적이다.

* Bubble Sort와 마찬가지로 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리 공간을 필요로 하지 않는다. => 제자리 정렬(in-place sorting)

  

**단점**

* 시간 복잡도가 O(n^2)으로 비효율적이다

* **불안정 정렬(Unstable Sort)**이다


<br><br><br><br>

### **안정** **정렬과** **불안정** **정렬**

#### 안정 정렬(Stable Sort)

안정 정렬은 중복된 값을 입력 **순서와 동일하게 정렬**하는 정렬 알고리즘의 특성을 말한다.

대표적으로 **삽입 정렬, 병합 정렬, 버블 정렬**이 있다.

<br>



#### **불안정 정렬(Unstable Sort)**

불안정 정렬은 안정 정렬과 반대로 중복된 값이 입력 **순서와 동일하지 않게 정렬**되는 알고리즘을 말한다.

대표적으로 **퀵정렬, 선택정렬, 계수정렬**이 있다.

<br>

ex)

[3, 2, 1, 5, 7, 5] 배열이 있다고 가정한다.



이를 오름차순으로 정렬한다고 할 때

[1, 2, 3, 5, 5, 7]이 된다면 **안정 정렬**

[1, 2, 3, 5, 5, 7]이 될 수 있다면 **불안정 정렬**이다.



<br><br><br><br>



### **c.** **삽입** **정렬(Insertion Sort)**

손 안의 카드를 정렬하는 방법과 유사하다. 

삽입 정렬은 선택 정렬과 유사하지만 좀 더 효율적인 알고리즘이다.

삽입 정렬은 2번째 원소부터 시작하여 그 앞(왼 쪽)의 원소들과 비교하여 삽입할 위치를 지정한 후, 원소를 뒤로 옮기고 지정된 자리에 자료를 삽입하여 정렬하는 알고리즘이다.

최선의 경우 O(n)이라는 빠른 효율성을 가지고 있어, 다른 정렬 알고리즘의 일부로 사용될 만큼 좋은 정렬 알고리즘이다.



<img src="/assets/img/cs/insertion-sort-001.gif">



1. 정렬은 2번 째 위치(index)의 값을 temp에 저장한다.

2. temp와 이전에 있는 원소들과 비교하여 삽입해나간다.

3. ‘1’번으로 돌아가 다음 위치(index)의 값을 temp에 저장하고 반복한다.




<br>



#### **Java Code**

```java
void insertionSort(int[] arr)
{
  for(int index = 1 ; index < arr.length ; index++){ // 1.
   int temp = arr[index];
   int prev = index - 1;
   while( (prev >= 0) && (arr[prev] > temp) ) {  // 2.
     arr[prev+1] = arr[prev];
     prev--;
   }
   arr[prev + 1] = temp;              // 3.
  }
  System.out.println(Arrays.toString(arr));
}
```

1. 첫 번째 원소 앞(왼쪽)에는 어떤 원소도 가지고 있지 않기 때문에 두 번째 위치(index)부터 탐색을 시작한다. temp에 임시로 해당 위치(index) 값을 저장하고, prev에는 해당 index 이전 index를 저장한다

2. 이전 index를 가리키는 prev가 음수가 되지 않고 이전 index의 값이 ‘1’번에서 선택한 값보다 크다면 서로 값을 교환해주고 prev를 더 이전 위치를 가리키도록 한다.

3. ‘2’번에서 반복문이 끝나고 난 뒤, prev에는 현재 **temp값보다 작은 값 중 제일일 큰 값의 index**를 가리키게 된다. 따라서 (prev+1)에 temp값을 삽입해준다.




<br>



**시간복잡도**: O(n^2), 정렬 되어 있는 경우 O(n)



<br>



**장점**

* 알고리즘이 단순하다.

* 대부분의 원소가 이미 정렬되어 있는 경우, 매우 효율적일 수 있다.

* 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리 공간을 필요로 하지 않는다. => 제자리 정렬(in-place sorting)

* **안정 정렬(Stable Sort)** 이다.

* Selection Sort나 Bubble Sort과 같은 O(n^2) 알고리즘에 비교하여 상대적으로 빠르다.

  

**단점**

* 평균과 최악의 시간복잡도가 O(n^2)으로 비효율적이다.
* Bubble Sort와 Selection Sort와 마찬가지로, 배열의 길이가 길어질수록 비효율적이다.

<br><br><br><br>



### **d.** **퀵** **정렬(Quick Sort)**

퀵 정렬은 분할 정복(divide and conquer) 방법을 통해 주어진 배열을 정렬한다.



<br>



>  [분할 정복 방법]
>  문제를 작은 2개의 문제로 분리하고 각각을 해결한 다음, 결과를 모아서 원래의  문제를 해결하는 전략
{: .prompt-tip}



<br>



<img src="/assets/img/cs/quick-sort-001.gif">



1. 배열 가운데서 하나의 원소를 고른다. 이렇게 고른 원소를 **피벗(pivot)**이라고 한다.

2. 피벗 앞에는 피벗보다 작은 값이 오고, 피벗 뒤에는 피벗보다 큰 값이 오도록 피벗을 기준으로 배열을 둘로 나눈다. 이렇게 배열을 둘로 나누는 것을 **분할(Divide)이**라고 한다. 

3. 분할을 마친 뒤에 피벗은 더 이상 움직이지 않는다.

4. 분할된 두 개의 작은 배열에 대해 재귀(Recursion)적으로 이 과정을 반복한다.

5. 재귀 호출이 한 번 진행될 때마다 최소한 하나의 원소는 최종적으로 위치가 정해지므로, 이 알고리즘은 반드시 끝난다는 것을 보장할 수 있다.




<br>



#### **Java Code**

```java
public class main {
  public static void quickSort(int[] arr) {
    // 전달받은 배열의 시작 index와 끝 index를 전달
    quickSort(arr, 0, arr.length-1);
  }

  public static void quickSort(int[] arr, int start, int end) {

    // partition 메서드를 통해 정렬할 배열의 중간지점(오른쪽 파트의 시작점) index를 반환
    int part2 = partition(arr, start, end);
    // 분할 정복
    // 오른쪽 파트의 시작점(part2-1)의 값이 배열의 시작점의 값보다 클 때 
    if(start < part2 -1) {
      // quickSort 재귀 호출(왼쪽 파트)
      quickSort(arr, start, part2-1);
    }
    // 오른쪽 파트의 시작점(part2)의 값값이 배열의 마지막 값보다 작을때
    if(part2 < end) {
      // quickSort 재귀 호출(오른쪽 파트)
      quickSort(arr, part2, end);
    }
  }

  // 배열의 중간지점으로 배열을 두 부분으로 나누는 메서드
  public static int partition(int[] arr, int start, int end) {
    // 배열의 중간 index를 pivot에 초기화
    int pivot = arr[(start+end) / 2];

    // 시작 index가 끝 index보다 작을 때(서로 교차하지 않았을 때)
    while(start <= end) {
      // 시작 index가 pivot보다 크면 loop 종료
      while(arr[start] < pivot) start++;

      // 끝 index가 pivot보다 작으면 loop 종료
      while(arr[end] > pivot) end--;

      // start 값이 end 값보다 작을때(서로 pivot을 기준으로 교차하지 않았을 때)
      if(start <= end) {
        // start와 end의 위치에 있는 값을 교체
        swap(arr, start, end);

        // start와 end 한 칸씩 이동
        start++;
        end--;
      }
    }
    // start index 반환(배열을 두 개로 나눈 후 오른쪽 part의 시작점)
    return start;
  }

  // 두 원소의 값을 교체하는 메서드
  public static void swap(int[] arr, int start, int end) {
    int tmp = arr[start];
    arr[start] = arr[end];
    arr[end] = tmp;
  }

  public static void printArray(int[] arr) {
    for(int data : arr) {
      System.out.print(data + ", ");
    }
    System.out.println();
  }

  public static void main(String[] args) {
    int[] arr = {3, 9, 4, 7, 5, 0, 1, 6, 8, 2};
    printArray(arr);
    quickSort(arr);
    printArray(arr);
  }
}
```



<br>



**시간복잡도**: 최악의 경우 O(n^2)이지만 보통의 경우 O(n log n)의 시간복잡도를 가진다



<br>



**장점**

* 불필요한 데이터의 이동을 줄이고 먼 거리의 데이터를 교환할 뿐만 아니라, 한 번 결정된 피벗들이 추후 연산에서 제외되는 특성 때문에, 시간 복잡도가 O(n log₂n)를 가지는 다른 정렬 알고리즘과 비교했을 때도 가장 빠르다.

* 정렬하고자 하는 배열 안에서 교환하는 방식이므로, 다른 메모리 공간을 필요로 하지 않는다.

  

**단점**

* **불안정 정렬(Unstable Sort)** 이다.

* 정렬된 배열에 대해서는 Quick Sort의 불균형 분할에 의해 오히려 수행시간이 더 많이 걸린다.

  


<br><br>



> **JAVA에서 Arrays.sort() 내부적으로도 Dual Pivot Quick Sort로 구현되어 있을 정도로 효율적인 알고리즘이고, 기술 면접에서 정말 빈번하게 나오는 주제이므로 반드시 숙지하자**
{: .prompt-tip}



<br><br>



나머지 정렬과 다른 알고리즘 이론은 다음 포스팅에 계속…



<br><br>



#### **참고 **

[2018 봄학기 알고리즘](http://alg.pknu.ac.kr/t/topic/576) 

[Time COmplexity (시간 복잡도)](https://hanamon.kr/알고리즘-time-complexity-시간-복잡도/)

[Tech Interview](https://gyoogle.dev/blog/)

[안정 정렬(Stable Sort)](https://riverblue.tistory.com/39)

[엔지니어 대한민국](https://youtu.be/7BDzle2n47c) - 퀵 정렬(QuickSort)
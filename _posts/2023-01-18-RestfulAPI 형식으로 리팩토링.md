---
title: RestfulAPI 형식으로 리팩토링
date: 2023-01-18 21:45:09 +09:00
categories: [Farmfarm, refactoring]
tags: [proejct, refactoring, restAPI]
---









프로젝트가 끝나고 성공적으로 발표를 마쳤습니다. 감사하게도 학원 내 **프로젝트 최우수상(1등)**이라는 성과도 얻을 수 있어 즐거웠던 프로젝트 경험이 될 것 같습니다. 

학원 수료 후 파이널 프로젝트 팀원들과 CS 스터디를 진행하면서 RestAPI에 대해서 알게 되었는데 우리가 만들었던 프로젝트는 RestAPI 형식에 전혀 맞지 않는다는 사실을 깨닫게 되었습니다. 



<br><br>



### 1. 문제 상황

기존에는 각각의 API마다 URI가 다르게 설정되어 있었고 HTTP 메서드도 일관성이 없었습니다. 이로 인해 코드의 가독성과 유지보수성이 떨어지는 문제가 있었습니다.

따라서 RESTful API를 적용하여 기존 코드를 리팩토링하기로 결정했습니다.

처음에는 RESTful API에 대해 잘 알지 못했기 때문에, URI 설계와 HTTP Method 결정 등이 어려웠습니다. 하지만, RESTful API에 대한 개념과 규칙을 이해하고, 목표를 명확하게 설정한 뒤에는 진행이 수월해졌습니다.



<br><br>



### RestAPI의 조건

#### 1) Uniform (유니폼 인터페이스)

Uniform Interface는 URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일을 말합니다.

#### 2) Stateless (무상태성)

REST는 무상태성 성격을 갖습니다. 다시 말해 작업을 위한 상태 정보를 따로 저장하고 관리하지 않습니다. 세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 됩니다. 때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해집니다.

#### 3) Cacheable (캐시 가능)

REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능합니다. 따라서 HTTP가 가진 캐싱 기능이 적용 가능합니다. HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능합니다.

#### 4) Self-descriptiveness (자체 표현 구조)

REST의 또 다른 큰 특징 중 하나는 REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다는 것입니다.

#### 5) Client - Server 구조

REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 됩니다.

#### 6) 계층형 구조

REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있게 합니다.



<br><br>



위에 나와있는 RestAPI의 특징 중 1, 4번에 중점을 놓고 프로젝트의 문제점을 찾았습니다.



<br><br>



#### 문제

- 프로젝트 컨트롤러의 요청 방식이 GET과 POST로 한정되어 있어 RestfulAPI 형식과 일치하지 않는 문제

- 요청 주소가 양식에 맞지 않아 직관적, 일관적이지 않고 중구난방인 문제





### 2. 해결 방안

1. 요청 방식

> 1. 단순 **조회** 기능의 경우 **@GetMapping** 사용
> 2. **삽입**(회원가입, 글 등록, 이미지 삽입 등) 기능의 경우 **@PostMapping** 사용
> 3. **수정(전체 수정)** 기능의 경우 **@PutMapping** 사용
> 4. **수정(부분 수정)** 기능의 경우 **@PatchMapping** 사용
> 5. **삭제** 기능의 경우 **@DeleteMapping** 사용

2. 요청 주소

> 1. 요청 주소에서 동사 -> **명사**로 변경
> 2. 단수 -> **복수**로 변경
> 3. 대상이 동일할 경우 요청 방식을 다르게, 주소는 동일하게 변경
> 4. 직관적이고 간결한 주소로 변경
> 5. 식별자 역할의 파라미터는 @PathVariable로 전달



<br><br>



### 3. 해결 과정

프로젝트 전반적으로 수정을 진행하였습니다.

아래 예시는 후기 목록 조회, 후기 상세 조회, 후기 등록,  후기 삭제의 수정 전, 후 입니다.



<br><br>



#### 후기 목록 조회 Controller 변경 전

``` java
	// 기존 코드
	@GetMapping("/review")
	public String selectReviewList(int productNo,
			@SessionAttribute(name = "loginMember", required = false) Member loginMember,
			@RequestParam(name = "sortFl", required = false, defaultValue = "R") String sortFl,
			@RequestParam(name = "cp", required = false, defaultValue = "1") int cp) {
		
		Map<String, Object> paramMap = new HashMap<String, Object>();
		paramMap.put("productNo", productNo);
		paramMap.put("cp", cp);
		paramMap.put("sortFl", sortFl);
		
		if(loginMember != null) {
			paramMap.put("memberNo", loginMember.getMemberNo());	
		} else {
			paramMap.put("memberNo", 0);		
		}
		
		Map<String, Object> map = service.selectReviewList(paramMap);
		
		return new Gson().toJson(map);
	}
	
```



<br><br>



#### 후기 목록 조회 Controller 변경 후

```java
	// 변경 후 코드
	@GetMapping("/reviews")
	public String selectReviewList(int productNo,
			@SessionAttribute(name = "loginMember", required = false) Member loginMember,
			@RequestParam(name = "sortFl", required = false, defaultValue = "R") String sortFl,
			@RequestParam(name = "cp", required = false, defaultValue = "1") int cp) {
		
		Map<String, Object> paramMap = new HashMap<String, Object>();
		paramMap.put("productNo", productNo);
		paramMap.put("cp", cp);
		paramMap.put("sortFl", sortFl);
		
		if(loginMember != null) {
			paramMap.put("memberNo", loginMember.getMemberNo());	
		} else {
			paramMap.put("memberNo", 0);		
		}
		
		Map<String, Object> map = service.selectReviewList(paramMap);
		
		return new Gson().toJson(map);
	}
```

- `단수`로 표현되었던 요청 주소를 `복수`로 변경하였습니다.



<br><br>



---



<br><br>



#### 후기 상세 조회 Controller 변경 전

```java
         // 기존 코드
	@GetMapping("/select/review")
	public String reviewDetail(int memberNo, int reviewNo) {
		
		Map<String, Object> map = new HashMap<String, Object>();
		
		map.put("memberNo", memberNo);
		map.put("reviewNo", reviewNo);
		
		Review review = service.selectReview(map);
		
		return new Gson().toJson(review);
	}
```



<br><br>



#### 후기 상세 조회 Controlloer 변경 후

```java
	// 수정 후 코드
	@GetMapping("/review/{reviewNo}")
	public String reviewDetail(int memberNo, 
			@PathVariable("reviewNo") int reviewNo) {
		
		Map<String, Object> map = new HashMap<String, Object>();
		
		map.put("memberNo", memberNo);
		map.put("reviewNo", reviewNo);
		
		Review review = service.selectReview(map);
		
		return new Gson().toJson(review);
	}
```

- `review` -> `reviews`로 단수를 복수로 변경하였습니다.
- 요청주소에서 `select`를 제거하였습니다.
- 파라미터로 전달된 `reviewNo`를 `PathVariable`로 전달하도록 변경하였습니다.



<br><br>



---



<br><br>



#### 후기 등록 Controller 변경 전

```java
	// 기존 코드
	@PostMapping("/review")
	public int writeReview(Review review, String reviewContent,
			@SessionAttribute("loginMember") Member loginMember,
			HttpServletRequest req,
			@RequestParam(value="reviewImg", required = false) List<MultipartFile> imageList

			) throws IOException {
		
		review.setMemberNo(loginMember.getMemberNo());
		
		String webPath = "/resources/images/product/review/";
		
		String filePath = req.getSession().getServletContext().getRealPath(webPath);
		
		int result = service.writeReview(webPath, filePath, review, imageList);
		
		
		return result;
	}
```



<br><br>



#### 후기 등록 Controller 변경 후

```java
	// 변경 후 코드
	@PostMapping("/reviews")
	public int writeReview(Review review, String reviewContent,
			@SessionAttribute("loginMember") Member loginMember,
			HttpServletRequest req,
			@RequestParam(value="reviewImg", required = false) List<MultipartFile> imageList

			) throws IOException {
		
		review.setMemberNo(loginMember.getMemberNo());
		
		String webPath = "/resources/images/product/review/";
		
		String filePath = req.getSession().getServletContext().getRealPath(webPath);
		
		int result = service.writeReview(webPath, filePath, review, imageList);
		
		
		return result;
	}
```

- `review` -> `reviews`로 단수에서 복수로 변경하였습니다.



<br><br><br>



---



#### 후기 삭제 Controller 변경 전

```java
    // 기존 코드
	@GetMapping("/review/delete")
	public int deleteReview(int reviewNo) {
		
		return service.deleteReview(reviewNo);
	}
	
```



<br><br>



#### 후기 삭제 Controller 변경 후

```java
    // 수정 후 코드
	@PatchMapping("/reviews/{reviewNo}")
	public int deleteReview(@PathVariable("reviewNo") int reviewNo) {
		
		return service.deleteReview(reviewNo);
	}
```

- 동사 `select`를 제거하였습니다
- `review` -> `reviews`로 단수를 복수로 변경하였습니다.
- `Get` -> `Patch`로 요청 방식을 변경하였습니다.
- 삭제 기능이지만 DB에서 Delete Flag를 'N' -> 'Y'로 `update`하는 방식을 사용하기 때문에 `Delete` 방식이 아닌 `Patch` 방식을 사용하였습니다.



<br><br>



---



<br><br>



### 4. 결과 및 마무리

이렇게 리팩토링을 마치고, 기존에 있던 중구난방했던 요청 주소 및 요청 방식이 가진 문제점이 해결되어서 코드의 가독성과 유지보수성이 향상된 것을 확인할 수 있었습니다. 또한, RESTful API를 적용함으로써, 코드의 일관성과 통일성을 유지하는 것의 중요성을 알게 되었습니다.

이번 과정을 통해 RESTful API의 개념과 규칙을 배우고 적용해볼 수 있었습니다. 앞으로도 이런 규칙을 유지하며 API를 설계하고 구현하는 데 노력해야겠습니다.



<br><br>

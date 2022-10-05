---
title:  "RequestEntity, ResponseEntity"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - Backend
tags:
  - Backend
---

Http 요청과 응답을 담당하는 클래스인 HttpEntity가 있다.

이 HttpEntity를 상속받은 녀석들이 RequestEntity와 ResponseEntity이다.

RequestEntity와 ResponseEntity는 세개의 필드값을 가진다 (헤더, 바디, 상태코드);

ResponseEntity를 이용한 컨트롤러 단에서의 예시는 다음과 같다

```java
public ResponseEntity<ResponseDto> checkJoinStep(@Request String email) {
        int step = memberService.checkJoinStep(email);
        ResponseDto<Integer> response = ResponseDto.<Integer>builder()
                .success(true)
                .response("회원가입 단계 확인")
                .data(step)
                .build();
        return ResponseEntity.ok().body(response);
    }
```

ResponseEntity를 조금 커스터마이징 하면 다음과 같이 짧아 질 수 있다.

밑의 스태틱 함수를 추가시켜주면

```java
public static <T> ResponseEntity<T> OK(T response) {
    return new ResponseEntity<>(true, response, null);
  }
```

다음과 같이 더욱 짧게 사용할 수 있다.
```java
public ResponseEntity<ResponseDto> checkJoinStep(@RequestBody String email) {
    return OK(memberService.checkJoinStep(email());
  }
```

컨트롤러 클래스의 코드 문장수가 짧아지고 가독성이 좋아진다.
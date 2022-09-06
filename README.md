# 💚 할머니는 다 들어주셔
> 스포츠 뉴스 커뮤니티<br/>

<br/>

## 1. 제작 기간 & 참여 인원
- 2022년 6월 24일 ~ 7월 29일
- 4인 팀 프로젝트

<br/>

## 2. 사용 기술
#### `Back-end`
  - Java 11
  - Spring Boot 2.7.2
  - Gradle 7.4.1
  - Spring Data JPA
  - MySQL 8.0.28
  - Spring Security
#### `Front-end`
  - HTML5
  - CSS3
  - Javascript
  - Bulma
  - BootStrap
  - JQuery

<br/>

## 3. 서비스 소개
네이버의 스포츠 뉴스를 스크래핑하여 클로바 요약 API로 각 뉴스를 요약해 보여줍니다.<br/>
회원들이 뉴스에 대한 의견을 댓글로 나눌 수 있습니다.<br/>
마음에 드는 뉴스를 북마크하거나 다른 회원의 댓글과 북마크를 모아볼 수 있습니다.<br/><br/>
[상세 정보 보기](https://github.com/2022-Harmony/NewsCommunity-bFinal)<br/>
[API 명세서 보기](https://github.com/2022-Harmony/NewsCommunity-bFinal/wiki/API-%EB%AA%85%EC%84%B8%EC%84%9C#)

https://user-images.githubusercontent.com/101458083/188725418-1b6b58cc-1f80-4ebc-b1c9-a8d90805272e.mp4

<br/>

## 4. ERD 설계
![](https://user-images.githubusercontent.com/96354426/181658487-2947bde3-6809-4a9b-9e25-b1467369ca15.png)

<br/>

## 5. 담당 기능
**인증**과 더불어 **회원**과 관련된 기능 (회원가입, 프로필 등) 을 담당했습니다.<br/>
아래의 토글 항목에서 주된 기능들의 코드와 간략한 설명을 보실 수 있습니다.<br/>

<details>
<summary><b>회원 생성</b></summary>
<div markdown="1"><br/>
  
  - json body를 받아 @Valid 어노테이션을 통해 유효성을 검증합니다.
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/e0e14bc37ba7ada45f6e423c7c698e34b841c3dc/src/main/java/com/teamharmony/newscommunity/domain/users/controller/UserController.java#L63-L66

<br/>

  - 트랜잭션으로 회원 객체를 저장할 때 기본 권한과 프로필을 함께 저장합니다.
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/e0e14bc37ba7ada45f6e423c7c698e34b841c3dc/src/main/java/com/teamharmony/newscommunity/domain/users/service/UserService.java#L87-L104
  
<br/></div>
</details>

<details>
<summary><b>인증</b></summary>
<div markdown="1"><br/>

  - Spring Security를 사용하여 필터에서 처리합니다.<br/>
  html form으로 입력받은 값을 HttpServletRequest 객체에서 가져옵니다.<br/>
  dto 객체를 통해 유효성 검증 후 UserDetailsService에 전달하여 조회하고 UserDetails 인터페이스를 구현한 User 객체를 생성합니다.
  UsernamePasswordAuthenticationToken을 생성, AuthenticationManager에 전달합니다.<br/>
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/e0e14bc37ba7ada45f6e423c7c698e34b841c3dc/src/main/java/com/teamharmony/newscommunity/domain/auth/filter/CustomAuthenticationFilter.java#L41-L62

<br/>

  - 인증에 성공하면 JWT를 발급합니다.<br/>
  토큰은 접근 토큰과 갱신 토큰을 발급하며, 사용자 ID와 함께 DB에 저장됩니다.<br/>
  다른 기능에서 인증된 사용자 ID를 필요로 하는 경우 사용할 수 있도록 헤더에 사용자 ID도 함께 반환합니다.<br/>
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/e0e14bc37ba7ada45f6e423c7c698e34b841c3dc/src/main/java/com/teamharmony/newscommunity/domain/auth/filter/CustomAuthenticationFilter.java#L65-L111
  
  
<br/></div>
</details>

<details>
<summary><b>인가</b></summary>
<div markdown="1"><br/>
  
  - Spring Security를 사용하여 필터에서 처리합니다.<br/>
  접근 토큰을 풀어 사용자의 정보를 확인하고 DB에 저장된 토큰 값으로 재확인 합니다.<br/>
  정상적인 접근이라면 UserDetails 인터페이스를 구현한 User 객체와 권한으로 UsernamePasswordAuthenticationToken을 생성합니다.<br/>
  SecurityContext에 보관합니다.
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/e0e14bc37ba7ada45f6e423c7c698e34b841c3dc/src/main/java/com/teamharmony/newscommunity/domain/auth/filter/CustomAuthorizationFilter.java#L38-L82
  
<br/></div>
</details>

<details>
<summary><b>프로필 수정</b></summary>
<div markdown="1"><br/>

- 인증된 사용자 ID로 기존 프로필 정보를 찾고, 입력받은 프로필 정보로 변경합니다.<br/>
  프로필 사진은 aws sdk 라이브러리를 사용하여 s3에 업로드합니다.<br/>
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/e0e14bc37ba7ada45f6e423c7c698e34b841c3dc/src/main/java/com/teamharmony/newscommunity/domain/users/service/UserService.java#L234-L261
  
<br/></div>
</details>

<br/>

## 6. 트러블 슈팅

### 6.1 핵심 트러블 슈팅

- 로그인 사용자의 정보 가져오기 실패<br/>[UserDetails가 null을 반환하는 문제](https://github.com/2022-Harmony/NewsCommunity-bFinal/issues/11)<br/>
댓글 등 다른 기능에서 사용자의 정보가 필요하여 @AuthenticationPrincipal 어노테이션을 사용하여 SecurityContext의 UserDetails 객체를 가져오려고 했습니다.<br/>
Authentication 객체의 Principal을 가져오는 과정에서 기존에는 username 자체, 즉 String을 넣었기 때문에 null이 반환되었습니다.<br/>
이를 UsernamePasswordAuthenticationToken 객체를 생성할 때 UserDetails를 구현한 User 객체를 넣어줌으로써 [해결](https://github.com/2022-Harmony/NewsCommunity-bFinal/commit/27de9174ca21834508c7cb8d6dc3c844f03f6138)하였습니다.

- 비로그인 사용자 서비스 원활히 이용 불가<br/>[비로그인 사용자의 경우 principal이 null을 반환하여 오류 발생하는 문제](https://github.com/2022-Harmony/NewsCommunity-bFinal/issues/157)<br/>
이 서비스의 경우, 비로그인 사용자도 조회 기능을 제한 없이 이용할 수 있도록 하고자 했습니다.<br/>
그러나 @AuthenticationPrincipal 어노테이션을 사용하여 사용자의 정보를 필요로 하는 조회 API의 경우 anonymousUser 문자열을 반환하게 되어 오류가 발생했습니다.<br/>
이를 해당 어노테이션을 커스텀하여 비로그인 사용자의 경우 null을 반환하도록 함으로써 [해결](https://github.com/2022-Harmony/NewsCommunity-bFinal/pull/158/commits/fede50d9a79ac2c41067b3a399ffd94a9cf04d0f)하였습니다.

<br/>

### 6.2 그 외 트러블 슈팅

<details>
<summary><b>회원 ID 중복 조회 불가</b></summary>
<div markdown="1">
  
  - 중복 조회 url이 SecurityConfig 내 permitAll() 누락되어 추가하여 [해결](https://github.com/2022-Harmony/NewsCommunity-bFinal/pull/164/commits/385d2d8f48efeef03ddee1acf93eb5b0ab9512bc)
  
</div>
</details>

<details>
<summary><b>필터 예외 처리 방식</b></summary>
<div markdown="1">
  
  - 컨트롤러와 통일되지 않아 ExceptionHandlerFilter 클래스 생성, 커스텀 예외를 발생시켜 [해결](https://github.com/2022-Harmony/NewsCommunity-bFinal/pull/241)
  
</div>
</details>

<details>
<summary><b>간헐적으로 로그인, 로그아웃이 제대로 이루어지지 않는 문제</b></summary>
<div markdown="1">
  
  - AJAX에 async 옵션을 주어 동기식 처리로 해결
  
</div>
</details>

<br/>

## 7. 회고 / 느낀점
[최종 프로젝트 회고](https://enyo9rt.notion.site/2a998f14fa654b89962a366b86e74473)

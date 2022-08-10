# 💚 할머니는 다 들어주셔
> 스포츠 뉴스 커뮤니티<br/>

</br>

## 1. 제작 기간 & 참여 인원
- 2022년 6월 24일 ~ 7월 29일
- 4인 팀 프로젝트

</br>

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

</br>

## 3. 서비스 소개
네이버의 스포츠 뉴스를 스크래핑하여 클로바 요약 API로 각 뉴스를 요약해 보여줍니다.<br/>
회원들이 뉴스에 대한 의견을 댓글로 나눌 수 있고, 마음에 드는 뉴스를 북마크하거나 다른 회원의 댓글과 북마크를 모아볼 수 있습니다.<br/><br/>
[상세 정보 보기](https://github.com/2022-Harmony/NewsCommunity-bFinal)
[API 명세서 보기](https://github.com/2022-Harmony/NewsCommunity-bFinal/wiki/API-%EB%AA%85%EC%84%B8%EC%84%9C#)

</br>

## 4. ERD 설계
![](https://user-images.githubusercontent.com/96354426/181658487-2947bde3-6809-4a9b-9e25-b1467369ca15.png)

</br>

## 5. 담당 기능
**인증**과 더불어 **회원**과 관련된 기능 (회원가입, 프로필 등) 을 담당했습니다.

<details>
<summary><b>회원 생성</b></summary>
<div markdown="1"><br/>
  
  - json body를 받아 @Valid 어노테이션을 통해 유효성을 검증합니다.
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/a8e44796da7787a9906e114e62277385576661b3/src/main/java/com/teamharmony/newscommunity/domain/users/controller/UserController.java#L54-L57

<br/>

  - 트랜잭션으로 회원 객체를 저장할 때 기본 권한과 프로필을 함께 저장합니다.
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/a8e44796da7787a9906e114e62277385576661b3/src/main/java/com/teamharmony/newscommunity/domain/users/service/UserService.java#L93-L110
  
<br/></div>
</details>

<details>
<summary><b>인증</b></summary>
<div markdown="1"><br/>

  - Spring Security를 사용하여 필터에서 처리합니다.<br/>
  html form으로 입력받은 값을 HttpServletRequest 객체에서 가져옵니다.<br/>
  dto 객체를 통해 유효성 검증 후 UserDetailsService에 전달하여 조회하고 UserDetails 인터페이스를 구현한 User 객체를 생성합니다.
  UsernamePasswordAuthenticationToken을 생성, AuthenticationManager에 전달합니다.<br/>
  https://github.com/enyo9rt/NewsCommunity-bFinal/blob/a8e44796da7787a9906e114e62277385576661b3/src/main/java/com/teamharmony/newscommunity/domain/auth/filter/CustomAuthenticationFilter.java#L41-L62

<br/>

  - 인증에 성공하면 JWT를 발급합니다.<br/>
  토큰은 접근 토큰과 갱신 토큰을 발급하며, 사용자 ID와 함께 DB에 저장됩니다.<br/>
  다른 기능에서 인증된 사용자 ID를 필요로 하는 경우 사용할 수 있도록 헤더에 사용자 ID도 함께 반환합니다.<br/>
  https://github.com/enyo9rt/NewsCommunity-bFinal/blob/a8e44796da7787a9906e114e62277385576661b3/src/main/java/com/teamharmony/newscommunity/domain/auth/filter/CustomAuthenticationFilter.java#L65-L111
  
  
<br/></div>
</details>

<details>
<summary><b>인가</b></summary>
<div markdown="1"><br/>
  
  - Spring Security를 사용하여 필터에서 처리합니다.<br/>
  접근 토큰을 풀어 사용자의 정보를 확인하고 DB에 저장된 토큰 값으로 재확인 합니다.<br/>
  정상적인 접근이라면 UserDetails 인터페이스를 구현한 User 객체와 권한으로 UsernamePasswordAuthenticationToken을 생성합니다.<br/>
  SecurityContext에 보관합니다.
https://github.com/enyo9rt/NewsCommunity-bFinal/blob/a8e44796da7787a9906e114e62277385576661b3/src/main/java/com/teamharmony/newscommunity/domain/auth/filter/CustomAuthorizationFilter.java#L38-L82
  
<br/></div>
</details>

<details>
<summary><b>프로필 수정</b></summary>
<div markdown="1"><br/>

- 인증된 사용자 ID로 기존 프로필 정보를 찾고, 입력받은 프로필 정보로 변경합니다.<br/>
  프로필 사진은 aws sdk 라이브러리를 사용하여 s3에 업로드합니다.<br/>
  https://github.com/enyo9rt/NewsCommunity-bFinal/blob/a8e44796da7787a9906e114e62277385576661b3/src/main/java/com/teamharmony/newscommunity/domain/users/service/UserService.java#L240-L271
  
<br/></div>
</details>

<br/>

## 6. 트러블 슈팅

</br>

## 7. 회고 / 느낀점

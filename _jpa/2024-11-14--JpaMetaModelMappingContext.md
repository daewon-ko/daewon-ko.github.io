---
layout: post
title: "@JpaMetaModelMappingContext"
tags:
- jpa

---

# @JpaMetaModelMappingContext





#@JpaMetaModelMappingContext란?

- JPA를 사용할때, Entity에 대한 매핑 정보를 제공하는 클래스
- 즉, JPA Entity에 대한 메타데이터를 제공하는 클래스



**문제**

- Presentation Layer 관련 테스트를 진행할때, @WebMvcTest를 통해 Controller관련 빈만 실제로 띄우고 Service Layer등에서 필요한 Bean등을 Mocking 처리해서 작성하곤 한다.
  - 그러나, 테스트코드 등을 작성하다보면 JPA관련한 Bean들을 띄워야할 때가 있고, 실제로 위와 같은 상황에서 JPA관련한 Bean을 띄우는데 없어서 발생한다.
- 그렇다면 왜 Jpa관련한 Bean을 띄우려고 했을까?
  - Web Layer를 테스트하는 경우 Service 이하의 Layer에 대해서는 Mocking처리를 하는데 어째서 JPA까지 의존적인 Bean을 띄우려고 한 것일까?



**해결책**

- 다음과 같이 하단 설정을 Presentation Layer Class Level 또는 Class 내에 설정해주면 된다. 해당 객체를 Mock객체로 설정해주면 된다.



>  *@MockBean(JpaMetamodelMappingContext.class)*



그렇다면 원인은 무엇일까 ?



**이유**

- @WebMvcTest의 경우, 내부적으로 Application 클래스의 설정을 먼저 로드하고, 웹 레이어에 필요한 클래스를 스캔한다.
- 그러나, @EnableJpaAuditing과 같이 JPA관련한 Bean의 경우에 Application설정에만 있고 테스트진영에는 해당 설정이 없게되면 JPA 관련한 예외가 발생한다.
- 따라서 Application 쪽에 @EnableJpaAuditing을 통해 JPA관련 설정해주는 Annotation을 삭제하거나 @JpaMetaModelMappingContext를 MockBean으로 등록해줘야 한다.





[참고](https://tlatmsrud.tistory.com/140) 

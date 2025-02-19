# Example Implementation of a Hexagonal Architecture

[![CI](https://github.com/thombergs/buckpal/actions/workflows/ci.yml/badge.svg)](https://github.com/thombergs/buckpal/actions/workflows/ci.yml)

[![Get Your Hands Dirty On Clean Architecture](https://reflectoring.io/assets/img/get-your-hands-dirty-260x336.png)](https://reflectoring.io/book)

This is the companion code to my eBook [Get Your Hands Dirty on Clean Architecture](https://leanpub.com/get-your-hands-dirty-on-clean-architecture).

It implements a domain-centric "Hexagonal" approach of a common web application with Java and Spring Boot. 

## Companion Articles

* [Hexagonal Architecture with Java and Spring](https://reflectoring.io/spring-hexagonal/)
* [Building a Multi-Module Spring Boot Application with Gradle](https://reflectoring.io/spring-boot-gradle-multi-module/)

## Prerequisites

* JDK 11
* this project uses Lombok, so enable annotation processing in your IDE

# 정리 

## Layered Architecture

JPA를 사용하는 경우 영속성 계층에 엔티티와 리포지토리가 함께 위치하게 되고 
도메인 계층에서 엔티티에 접근할 수 있게 돼 영속성 계층과 도메인 계층에 강한 결합이 생김 
영속성 코드가 도메인 코드에 녹아들어가면 유지보수가 힘들어짐 

아키텍처 규칙을 강제하고 규칙이 깨지는 경우 빌드가 실패하도록 만들면 도움이 됨 

SI프로젝트에서는 종종 컨트롤러에서 바로 영속성계층에 접근하는데.. 
이렇게 도메인계층을 뛰어넘으면 두 가지 문제가 발생 

1. 도메인로직을 웹계층에서 구현 -> 지금은 간단하더라도 추후 확장 시 책임 섞이고 도메인로직 분산
2. 웹 계층 테스트에서 영속성 계층도 Mocking -> 테스트 복잡도 증가해 테스트코드 작성보다 종속성 이해 + 목 생성 시간이 길어짐 

이렇듯 계층형 아키텍처로 개발 시 여러 함정을 염두해 두고 개발할 것 

## IoC 

Single Responsibility Principle 
컴포넌트를 변경하는 이유는 하나뿐이어야 한다. 

인터페이스를 도입해 의존성을 역전시키고 영속성 계층이 도메인 계층에 의존하도록 설정 
도메인 코드에서는 어떤 영속성 프레임워크나 UI 프레임워크가 사용되는지 알 수 없음 

이렇게되면 도메인의 Entity와 리포지토리의 Entity를 따로 관리해 줘야 함 

## 헥사고날 아키텍처 패키지 구조 

domain persistence web 각 계층별로 디렉토리 구조를 가져가면
각 디렉토리가 어떤 유스케이스를 제공하는지 파악하기 힘들고 아키텍처를 파악할 수 없음 

기능으로만 디렉토리를 구성하면 가시성이 떨어짐 

기능 - 계층 - adapter - port 로 구성된 패키지 구조를 사용 


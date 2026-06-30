# Deployment Guide

> AWS 기반 High Availability 3-Tier(Web-WAS-DB) Infrastructure 구축 과정

---

# Deployment Overview

본 프로젝트는 AWS 환경에서 Web, WAS, Database 계층을 분리한 3-Tier 아키텍처를 구축하고, Spring PetClinic 애플리케이션을 배포하는 것을 목표로 하였습니다.

전체 구축 과정은 다음과 같은 순서로 진행하였습니다.

```text
IAM
    ↓
VPC & Networking
    ↓
EC2
    ↓
Apache HTTP Server (Web)
    ↓
Apache Tomcat (WAS)
    ↓
Amazon RDS
    ↓
Spring PetClinic
    ↓
Application Load Balancer
    ↓
Network Load Balancer
    ↓
Auto Scaling
    ↓
CloudWatch
```

---

# Step 1. IAM Configuration

AWS 리소스 접근 제어를 위해 IAM을 구성하였습니다.

## Configuration

* IAM User 생성
* IAM Group 구성
* IAM Policy 적용
* 최소 권한 원칙(Least Privilege) 적용

### Purpose

* 안전한 AWS 리소스 관리
* 사용자별 권한 관리

---

# Step 2. AWS Network Configuration

AWS 환경에 서비스 배포를 위한 네트워크를 구성하였습니다.

## Configuration

* Amazon VPC 생성
* Public Subnet 구성
* Private Subnet 구성
* Internet Gateway 연결
* NAT Gateway 구성
* Route Table 설정
* Security Group 설정

### Purpose

* 외부 인터넷과 내부 서비스 분리
* Web / WAS / Database 계층 분리
* 보안성과 확장성 확보

---

# Step 3. Web Layer Deployment

Web Layer에는 Apache HTTP Server를 구축하였습니다.

## Configuration

* Apache HTTP Server 설치
* Reverse Proxy 모듈 활성화
* 서비스 자동 실행 설정
* 기본 페이지 구성

### Purpose

* 클라이언트 요청 수신
* Reverse Proxy 역할 수행

---

# Step 4. WAS Layer Deployment

Web Application Server에는 Apache Tomcat을 구축하였습니다.

## Configuration

* Amazon Corretto (Java 8) 설치
* JAVA_HOME 환경 변수 설정
* Apache Tomcat 설치
* Maven 설치
* Tomcat 서비스 실행

### Purpose

* Java 기반 웹 애플리케이션 실행
* Spring PetClinic 운영

---

# Step 5. Apache Reverse Proxy Configuration

Apache HTTP Server를 Reverse Proxy로 구성하여 Web과 WAS를 분리하였습니다.

## Configuration

* ProxyPass 설정
* ProxyPassReverse 설정
* Apache 재시작
* Reverse Proxy 연결 테스트

### Purpose

* Web과 WAS 역할 분리
* 내부 WAS 직접 노출 방지
* 요청 전달 구조 구성

---

# Step 6. Database Deployment

Amazon RDS(MySQL)를 구축하여 Database Layer를 구성하였습니다.

## Configuration

* Amazon RDS 생성
* MySQL Database 생성
* Security Group 설정
* 애플리케이션 연동

### Purpose

* 데이터 영속성 확보
* 관리형 데이터베이스 활용

---

# Step 7. Spring PetClinic Deployment

Spring PetClinic 애플리케이션을 배포하였습니다.

## Configuration

* GitHub Repository Clone
* Maven Build
* WAR 파일 생성
* Apache Tomcat 배포
* 애플리케이션 실행

### Purpose

* 실제 서비스 환경 구성
* Web-WAS-Database 전체 연동 확인

---

# Step 8. Load Balancer Configuration

서비스 가용성 확보를 위해 Load Balancer를 구성하였습니다.

## Application Load Balancer (ALB)

* HTTP 요청 수신
* Web Layer 트래픽 분산
* Health Check 수행

## Network Load Balancer (NLB)

* Web Layer와 WAS Layer 간 TCP 통신
* WAS 트래픽 분산
* 내부 통신 처리

### Purpose

* 고가용성 확보
* 장애 발생 시 정상 인스턴스로 요청 전달

---

# Step 9. Auto Scaling Configuration

부하 상황에 따라 EC2 인스턴스가 자동으로 확장되도록 구성하였습니다.

## Configuration

* Launch Template 생성
* Auto Scaling Group 구성
* Target Tracking Scaling Policy 적용

### Purpose

* 서비스 확장성 확보
* 장애 발생 시 자동 복구
* 운영 비용 최적화

---

# Step 10. Monitoring

운영 상태를 지속적으로 확인하기 위해 Amazon CloudWatch를 적용하였습니다.

## Monitoring Target

* EC2 CPU Utilization
* Network I/O
* Application Load Balancer 상태
* Auto Scaling 상태
* EC2 Health Check

### Purpose

* 서비스 상태 실시간 모니터링
* 장애 조기 탐지
* 운영 환경 가시성 확보

---

# Validation

구축 완료 후 다음 항목을 검증하였습니다.

* Apache HTTP Server 정상 동작
* Apache Tomcat 정상 실행
* Apache Reverse Proxy 연동
* Spring PetClinic 정상 서비스
* Amazon RDS 연동
* Application Load Balancer Health Check 통과
* Network Load Balancer 연결 확인
* Auto Scaling 정상 동작
* CloudWatch Dashboard 모니터링 확인

---

# Deployment Summary

| Layer | Technology |
|--------|------------|
| Web | Apache HTTP Server |
| WAS | Apache Tomcat |
| Database | Amazon RDS (MySQL) |
| Load Balancer | Application Load Balancer / Network Load Balancer |
| Scaling | Auto Scaling |
| Monitoring | Amazon CloudWatch |
| Security | IAM, Security Group, Session Manager, Secrets Manager |

---

# Deployment Result

최종적으로 다음과 같은 운영 환경을 구축하였습니다.

```text
Internet
    ↓
Route53
    ↓
CloudFront
    ↓
Application Load Balancer
    ↓
Apache HTTP Server
    ↓
Network Load Balancer
    ↓
Apache Tomcat
    ↓
Amazon RDS
```

이를 통해 Web, WAS, Database 계층을 분리한 고가용성 3-Tier 인프라를 구축하고, 운영 환경을 고려한 **확장성(Scalability)**, **가용성(High Availability)**, **모니터링(Observability)** 체계를 갖춘 서비스를 구현하였습니다.
# AWS High Availability 3-Tier Web Infrastructure

> AWS 기반의 고가용성 3-Tier(Web?WAS?DB) 아키텍처를 구축하고, Apache · Tomcat · Amazon RDS를 연동하여 Spring PetClinic 애플리케이션을 배포한 팀 프로젝트입니다.

<div align="center">

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge\&logo=amazonaws\&logoColor=white)
![Apache](https://img.shields.io/badge/Apache-D22128?style=for-the-badge\&logo=apache\&logoColor=white)
![Tomcat](https://img.shields.io/badge/Tomcat-F8DC75?style=for-the-badge\&logo=apachetomcat\&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge\&logo=mysql\&logoColor=white)

</div>

---

# Project Overview

## Background

웹 서비스는 단순히 애플리케이션을 실행하는 것만으로는 안정적인 서비스 운영이 어렵습니다.

실제 운영 환경에서는

* 안정적인 트래픽 분산
* 장애 발생 시 서비스 지속성
* 보안
* 확장성
* 운영 모니터링

등을 함께 고려한 인프라 설계가 필요합니다.

본 프로젝트에서는 AWS 환경에서 **High Availability 3-Tier Architecture**를 구축하고, Apache Web Server, Tomcat WAS, Amazon RDS를 연동하여 Spring PetClinic 서비스를 운영 가능한 형태로 배포하는 것을 목표로 하였습니다.

또한 Auto Scaling, CloudWatch, Load Balancer 등을 적용하여 운영 환경을 고려한 인프라를 구현하였습니다.

---

# Project Goals

* AWS 기반 3-Tier 아키텍처 구축
* Web / WAS / DB 계층 분리
* Reverse Proxy 기반 Apache-Tomcat 연동
* ALB/NLB 기반 트래픽 분산
* Auto Scaling 기반 확장성 확보
* CloudWatch 기반 운영 모니터링
* Spring PetClinic 배포

---

# Project Information

| Category    | Description                                 |
| ----------- | ------------------------------------------- |
| Project     | AWS High Availability 3-Tier Infrastructure |
| Type        | Team Project (5 Members)                    |
| Duration    | 3 Weeks                                     |
| Position    | Team Leader                                 |
| Domain      | Cloud Infrastructure                        |
| Environment | AWS                                         |

---

# My Contributions

## Infrastructure

* AWS IAM 사용자 및 권한 정책 구성
* AWS 3-Tier(Web-WAS-DB) 인프라 구축
* Apache Web Server 구축
* Apache Reverse Proxy 구성
* Apache ↔ Tomcat 연동
* ALB 및 NLB 기반 서비스 구성
* Spring PetClinic 배포

## Monitoring & Reliability

* CloudWatch 기반 운영 모니터링
* Auto Scaling 구성 및 동작 검증
* Health Check 기반 서비스 상태 확인
* 부하 테스트를 통한 서비스 안정성 검증

## Team Management

* 프로젝트 일정 관리
* 인프라 구축 일정 조율
* 팀원 역할 분배
* 프로젝트 최종 통합 및 발표

---

# Key Features

## High Availability

* Multi-AZ 기반 인프라 구성
* Application Load Balancer
* Network Load Balancer
* Auto Scaling

---

## Security

* IAM 기반 접근 제어
* Security Group 구성
* Private Subnet 기반 WAS / DB 분리
* Session Manager를 활용한 안전한 서버 접근
* Secrets Manager를 활용한 민감 정보 관리

> Session Manager 및 Secrets Manager는 프로젝트 기능으로 구현되었으며 팀원과 협업하여 완성하였습니다.

---

## Monitoring

* Amazon CloudWatch
* Health Check
* Auto Scaling Monitoring
* Resource Monitoring

---

## Delivery

* Spring PetClinic Application 배포
* Apache Reverse Proxy 기반 서비스 제공
* Amazon RDS 연동

---

# Tech Stack

## Cloud

* AWS EC2
* VPC
* Internet Gateway
* NAT Gateway
* Public / Private Subnet
* Route Table
* Security Group
* Application Load Balancer
* Network Load Balancer
* Amazon RDS
* IAM
* CloudWatch
* Auto Scaling
* Route53
* CloudFront
* Session Manager
* Secrets Manager

---

## Web

* Apache HTTP Server 2.4
* Reverse Proxy

---

## WAS

* Apache Tomcat 9
* Java 8 (Amazon Corretto)
* Maven

---

## Database

* Amazon RDS (MySQL)

---

# Architecture

> **Architecture Diagram**

![alt text](image.png)

---

# Service Flow

```text
User

↓

Route53

↓

CloudFront

↓

Application Load Balancer

↓

Apache Web Server

↓

Network Load Balancer

↓

Tomcat WAS

↓

Amazon RDS
```

서비스 요청은 ALB를 통해 Web Layer로 전달되며, Apache Reverse Proxy를 이용하여 내부 WAS 계층으로 라우팅됩니다.

WAS는 Amazon RDS와 통신하여 데이터를 처리하며, Auto Scaling과 CloudWatch를 통해 서비스의 확장성과 안정성을 확보하였습니다.

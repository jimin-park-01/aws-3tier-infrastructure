# Design Decisions

프로젝트를 설계하고 구축하는 과정에서 내린 주요 기술적 의사결정과 그 이유를 기록한다.

각 의사결정은 서비스의 **확장성(Scalability)**, **가용성(High Availability)**, **보안성(Security)** 및 **운영 효율성(Operational Efficiency)** 을 고려하여 수행하였다.

---

# Decision 001 - Adopting a 3-Tier Architecture

## Context

웹 서버와 애플리케이션 서버가 하나의 인스턴스에서 동작하는 경우 서비스 규모가 증가할수록 유지보수와 확장이 어려워진다.

또한 하나의 서버 장애가 전체 서비스 장애로 이어질 수 있다.

## Decision

Web, WAS, Database를 각각 독립된 계층으로 분리한 3-Tier Architecture를 적용하였다.

## Alternatives

### Single Tier

* Web / WAS / DB 통합
* 구축이 단순함
* 장애 발생 시 전체 서비스 중단

### 3-Tier Architecture (Selected)

* 역할 분리
* 독립적인 확장 가능
* 유지보수 용이
* 보안성 향상

## Reason

서비스 확장성과 운영 안정성을 고려하여 Web, WAS, Database를 독립된 계층으로 분리하였다.

또한 고가용성(High Availability)을 고려한 구조를 적용하여 장애 발생 시에도 서비스 지속성을 확보할 수 있도록 설계하였다.

---

# Decision 002 - Apache Reverse Proxy

## Context

클라이언트가 Apache Tomcat에 직접 접근하는 경우 애플리케이션 서버가 외부에 노출되어 보안성과 운영 관리 측면에서 한계가 있다.

## Decision

Apache HTTP Server를 Reverse Proxy로 구성하여 모든 요청을 Web Layer에서 수신하도록 설계하였다.

## Alternatives

### Direct Access

* 구조 단순
* WAS 직접 노출
* 보안 취약

### Apache Reverse Proxy (Selected)

* WAS 보호
* 요청 제어 가능
* 운영 관리 용이

## Reason

Web Layer와 WAS Layer를 분리하여 보안성을 향상시키고, 운영 환경에서 요청을 효율적으로 관리할 수 있도록 하였다.

---

# Decision 003 - Application Load Balancer for External Traffic

## Context

외부 사용자의 요청을 여러 Web Server에 안정적으로 분산할 필요가 있었다.

## Decision

Application Load Balancer(ALB)를 Public Subnet에 배치하여 외부 트래픽을 처리하였다.

## Alternatives

### Direct EC2 Access

* 단순한 구성
* 단일 장애 지점 발생

### Application Load Balancer (Selected)

* HTTP/HTTPS 처리
* Health Check
* Load Balancing
* 고가용성 확보

## Reason

서비스 장애 발생 시에도 정상 인스턴스로 요청을 전달하고, 외부 트래픽을 안정적으로 처리하기 위해 ALB를 선택하였다.

---

# Decision 004 - Network Load Balancer Between Web and WAS

## Context

Web Layer와 WAS Layer 간 내부 통신을 안정적으로 처리해야 했다.

## Decision

Network Load Balancer(NLB)를 사용하여 Apache HTTP Server와 Apache Tomcat 사이의 트래픽을 분산하였다.

## Alternatives

### Direct Connection

* 단순한 구성
* WAS 장애 시 서비스 영향

### Network Load Balancer (Selected)

* TCP 기반 통신
* WAS 이중화
* 내부 트래픽 분산

## Reason

WAS 계층의 가용성과 확장성을 확보하고 내부 트래픽을 안정적으로 처리하기 위해 NLB를 적용하였다.

---

# Decision 005 - Auto Scaling

## Context

트래픽 증가 시 하나의 EC2 인스턴스로는 안정적인 서비스 제공이 어렵다.

## Decision

Auto Scaling Group을 구성하여 EC2 인스턴스를 자동으로 확장하도록 설계하였다.

## Alternatives

### Fixed EC2

* 관리 단순
* 확장 불가

### Auto Scaling (Selected)

* 자동 확장
* 자동 복구
* 운영 비용 최적화

## Reason

트래픽 변화에 따라 서비스 성능을 안정적으로 유지하고, 장애 발생 시 자동으로 인스턴스를 복구할 수 있도록 Auto Scaling을 적용하였다.

또한 불필요한 리소스 사용을 줄여 운영 비용을 효율적으로 관리할 수 있도록 설계하였다.

---

# Decision 006 - IAM Based Access Control

## Context

AWS 리소스를 여러 사용자가 함께 관리하는 환경에서는 체계적인 권한 관리가 필요하다.

## Decision

IAM User, Group, Policy를 이용하여 최소 권한 원칙(Least Privilege)을 적용하였다.

## Alternatives

### Root Account

* 모든 권한 보유
* 보안 위험

### IAM (Selected)

* 권한 분리
* 사용자 관리
* 보안 강화

## Reason

운영 환경에서 안전한 리소스 접근과 사용자별 권한 관리를 위해 IAM 기반 접근 제어를 적용하였다.

---

# Decision 007 - Amazon CloudWatch Monitoring

## Context

서비스 운영 중 장애와 리소스 사용량을 실시간으로 확인할 필요가 있었다.

## Decision

Amazon CloudWatch를 이용하여 EC2와 Auto Scaling 상태를 모니터링하였다.

## Alternatives

### Manual Monitoring

* 지속적인 확인 필요
* 장애 대응 지연

### Amazon CloudWatch (Selected)

* 실시간 모니터링
* 운영 상태 확인
* Auto Scaling 연계

## Reason

운영 환경에서 장애를 빠르게 탐지하고 리소스 사용량을 지속적으로 확인하기 위해 Amazon CloudWatch를 적용하였다.

---

# Decision 008 - Secrets Manager for Credential Management

## Context

애플리케이션에서 데이터베이스 계정 정보를 직접 설정 파일에 저장할 경우 민감 정보가 노출될 위험이 있다.

## Decision

AWS Secrets Manager를 이용하여 데이터베이스 접속 정보를 안전하게 관리하였다.

## Alternatives

### Plain Configuration

* 설정 파일에 직접 저장
* 관리 단순
* 보안 위험

### AWS Secrets Manager (Selected)

* 민감 정보 암호화
* 중앙 집중 관리
* 보안성 향상

## Reason

운영 환경에서 데이터베이스 계정 정보를 안전하게 관리하고, 애플리케이션 코드와 민감 정보를 분리하기 위해 AWS Secrets Manager를 적용하였다.
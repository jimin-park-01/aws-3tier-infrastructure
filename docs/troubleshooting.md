# Troubleshooting

프로젝트를 구축하는 과정에서 발생한 주요 문제와 원인, 해결 과정을 기록한다.

운영 환경에서는 단순히 기능 구현뿐 아니라 장애 원인을 분석하고 단계적으로 검증하는 과정이 중요하다는 점을 경험하였다.

---

# Trouble 001 - Apache Reverse Proxy Connection Failed

## Problem

Apache HTTP Server를 Reverse Proxy로 구성하였지만 요청이 Apache Tomcat으로 정상 전달되지 않아 서비스에 접근할 수 없었다.

## Cause

* ProxyPass 및 ProxyPassReverse 설정 오류
* Apache HTTP Server와 Apache Tomcat 간 연결 설정 문제
* Apache 설정 변경 후 서비스 재시작 누락

## Solution

* ProxyPass 설정을 재검토하였다.
* Apache Tomcat 서비스와 8080 포트 상태를 확인하였다.
* Apache HTTP Server를 재시작하여 변경 사항을 적용하였다.
* `curl` 명령을 이용하여 Apache → Tomcat 내부 통신을 단계적으로 검증하였다.

## Result

Apache HTTP Server가 Reverse Proxy 역할을 수행하며 Apache Tomcat으로 요청을 정상 전달하는 것을 확인하였다.

---

# Trouble 002 - ALB Health Check Failed

## Problem

Application Load Balancer의 Target Group이 Healthy 상태로 변경되지 않아 외부 요청이 Web Server로 전달되지 않았다.

## Cause

* Security Group 접근 정책 오류
* Health Check Path 설정 오류
* Apache HTTP Server 미실행

## Solution

* ALB와 Web Server의 Security Group 규칙을 수정하였다.
* Health Check Path를 점검하고 수정하였다.
* Apache HTTP Server 실행 상태를 확인하였다.

## Result

Target Group이 Healthy 상태로 변경되었으며 ALB를 통한 트래픽 분산이 정상적으로 이루어졌다.

---

# Trouble 003 - Amazon RDS Connection Failed

## Problem

Spring PetClinic 애플리케이션이 Amazon RDS(MySQL)에 정상적으로 연결되지 않았다.

## Cause

* RDS Endpoint 설정 오류
* Security Group 접근 제한
* JDBC Connection 정보 오류

## Solution

* Amazon RDS Endpoint를 재확인하였다.
* Database Security Group 규칙을 수정하였다.
* JDBC Connection 정보를 수정한 후 애플리케이션을 재배포하였다.

## Result

Spring PetClinic과 Amazon RDS 간 정상적인 데이터 통신을 확인하였다.

---

# Trouble 004 - Auto Scaling Validation

## Problem

Auto Scaling Group을 구성하였지만 테스트 환경에서는 Scale Out이 발생하지 않아 정책이 정상적으로 동작하는지 확인하기 어려웠다.

## Cause

일반적인 테스트 환경에서는 CPU 사용률이 Auto Scaling 정책의 임계값에 도달하지 않았다.

## Solution

* k6를 이용하여 부하 테스트를 수행하였다.
* CloudWatch Dashboard에서 CPU Utilization과 Network I/O를 실시간으로 모니터링하였다.
* Auto Scaling Group의 Instance Count 변화를 확인하였다.

## Result

CPU 사용률 증가에 따라 새로운 EC2 인스턴스가 자동으로 생성되는 것을 확인하였으며, Auto Scaling 정책이 정상적으로 동작함을 검증하였다.

---

# Trouble 005 - Spring PetClinic Deployment Failed

## Problem

Apache Tomcat에 WAR 파일을 배포하였지만 애플리케이션이 정상적으로 실행되지 않았다.

## Cause

* WAR 파일 배포 위치 오류
* Apache Tomcat 재시작 누락
* Java 실행 환경 설정 문제

## Solution

* WAR 파일 배포 위치를 수정하였다.
* Apache Tomcat 서비스를 재시작하였다.
* Java 실행 환경을 확인하였다.
* 애플리케이션 로그를 분석하여 원인을 확인하였다.

## Result

Spring PetClinic 애플리케이션이 정상적으로 배포되었으며 Web-WAS-Database 전체 연동을 확인하였다.

---

# Lessons Learned

* 장애 발생 시 로그와 서비스 상태를 단계적으로 확인하는 과정이 문제 해결에 가장 효과적이라는 점을 경험하였다.
* Security Group, Health Check, Reverse Proxy 등 인프라 구성 요소가 서비스 정상 동작에 직접적인 영향을 미친다는 점을 확인하였다.
* CloudWatch와 부하 테스트를 활용하여 운영 환경을 검증하는 과정의 중요성을 이해하였다.
* 단순한 구축을 넘어 장애 분석과 검증 과정을 반복하며 운영 관점에서 문제를 해결하는 경험을 쌓을 수 있었다.
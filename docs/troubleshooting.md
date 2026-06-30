# Troubleshooting

프로젝트를 구축하는 과정에서 발생한 주요 문제와 원인, 해결 과정을 기록한다.

---

# Trouble 001 - Apache Reverse Proxy Connection Failed

## Problem

Apache를 통해 WAS(Tomcat)로 요청을 전달하도록 Reverse Proxy를 구성하였지만, 정상적으로 페이지가 출력되지 않았다.

---

## Cause

* ProxyPass 대상 주소 설정 오류
* Tomcat 서비스가 정상적으로 실행되지 않음
* Apache 설정 변경 후 서비스 재시작 누락

---

## Solution

* ProxyPass 및 ProxyPassReverse 설정을 재검토하였다.
* Tomcat 프로세스 및 8080 포트 상태를 확인하였다.
* Apache 서비스를 재시작하여 변경된 설정을 적용하였다.
* `curl` 명령을 이용하여 Apache → Tomcat 내부 통신을 단계적으로 검증하였다.

---

## Result

Apache가 Reverse Proxy 역할을 수행하며 Tomcat으로 요청을 정상 전달하는 것을 확인하였다.

---

# Trouble 002 - ALB Health Check Failed

## Problem

Application Load Balancer의 Target Group이 Healthy 상태로 변경되지 않아 트래픽이 전달되지 않았다.

---

## Cause

* Security Group 접근 정책 오류
* Health Check Path 설정 오류
* Apache 서비스 미실행

---

## Solution

* ALB Security Group과 Web Security Group의 인바운드 규칙을 수정하였다.
* Health Check 경로를 확인 및 수정하였다.
* Apache 서비스 실행 상태를 점검하였다.

---

## Result

Target Group이 Healthy 상태로 변경되었으며 ALB를 통한 요청 분산이 정상적으로 이루어졌다.

---

# Trouble 003 - Amazon RDS Connection Failed

## Problem

Spring PetClinic 애플리케이션에서 Amazon RDS(MySQL) 연결에 실패하였다.

---

## Cause

* RDS Endpoint 설정 오류
* Database Security Group 접근 제한
* JDBC 연결 정보 오류

---

## Solution

* RDS Endpoint를 재확인하였다.
* Database Security Group 규칙을 수정하였다.
* JDBC Connection 정보를 수정하고 애플리케이션을 재배포하였다.

---

## Result

애플리케이션에서 Amazon RDS와 정상적으로 통신하는 것을 확인하였다.

---

# Trouble 004 - Auto Scaling Validation

## Problem

Auto Scaling Group을 구성하였지만 Scale Out이 발생하지 않아 정상 동작 여부를 확인하기 어려웠다.

---

## Cause

테스트 환경에서는 CPU 사용률이 Auto Scaling 정책 기준에 도달하지 않았다.

---

## Solution

* 부하 테스트를 수행하여 CPU 사용률을 증가시켰다.
* CloudWatch에서 CPU Utilization을 모니터링하였다.
* Auto Scaling 정책이 정상적으로 동작하는지 확인하였다.

---

## Result

CPU 사용률 증가에 따라 새로운 EC2 인스턴스가 생성되는 것을 확인하여 Auto Scaling 정책이 정상적으로 동작함을 검증하였다.

---

# Trouble 005 - Spring PetClinic Deployment Failed

## Problem

Tomcat에 WAR 파일을 배포하였지만 애플리케이션이 정상적으로 실행되지 않았다.

---

## Cause

* WAR 배포 위치 오류
* Tomcat 재시작 누락
* Java 실행 환경 문제

---

## Solution

* WAR 파일 배포 위치를 수정하였다.
* Tomcat 서비스를 재시작하였다.
* Java 및 Tomcat 실행 상태를 확인하였다.
* 애플리케이션 로그를 분석하여 배포 상태를 검증하였다.

---

## Result

Spring PetClinic 애플리케이션이 정상적으로 배포되었으며 Web-WAS-DB 전체 연동을 확인하였다.

---

# Lessons Learned

* 문제 발생 시 로그와 서비스 상태를 단계적으로 확인하는 것이 가장 중요하다는 것을 경험하였다.
* 단순히 설정을 수정하기보다 원인을 분석하고 하나씩 검증하는 방식이 문제 해결에 효과적이었다.
* AWS 환경에서는 Security Group, Health Check, 네트워크 설정이 서비스 정상 동작에 큰 영향을 미친다는 점을 확인하였다.
* 구축 이후에도 모니터링과 검증 과정을 통해 서비스 안정성을 지속적으로 확인하는 것이 중요함을 배웠다.

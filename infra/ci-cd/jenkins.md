# Jenkins

## CI / CD

💡 해당 정보들을 단계별로 진행하시면 CI / CD 구성이 가능합니다.

⚙️ SpringBoot, S3, ElasitcBeanstalk 를 기준으로 작성하였습니다.

## 개요

* 애플리케이션 개발 단계를 자동화하여 짧은 주기로 서비스를 제공하는 방식입니다.
* 본 개념은 지속적인 통합, 지속적인 서비스 제공, 지속적인 배포입니다.
* 새로운 코드 통합으로 인해 개발 및 운영팀에 발생하는 문제을 해결하기 위한 방법입니다.
* 특히, CI/CD는 애플리케이션의 통합 및 테스트 단계에서부터 제공 및 배포에 이르는 애플리케이션의 라이프사이클 전체에 걸쳐 지속적인 자동화와 지속적인 모니터링을 제공합니다. 이러한 구축 사례를 일반적으로 "CI/CD 파이프라인"이라 부르며 DevOps를 통해 지원됩니다.

## 구성 및 흐름

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 10.00.33.png>)

1. 기호에 맞는 분산 버전 관리 툴을 선정합니다. (본 구성도는 github 기준)
2. Github 내 변경이력을 감지하여 Jenkins로 WebHook을 날리게 됩니다.
3. Jenkins에서 다음의 과정을 수행합니다.
   1. 각종 빌드 툴(maven, gradle etc)에 적합한 build script 수행
   2. AWS S3와 같은 리소스 저장소에 빌드 결과물 업로드
   3. Elastic Beanstalk → Deploy
4. Eleastic Beanstalk는 S3에 업로드된 빌드 결과물을 가지고 각 서버로 배포를 진행하게 됩니다.

***

## 구현하기 - \[Eleastic Beanstalk] 생성



* 환경 생성을 수행합니다.

![ ](<../../.gitbook/assets/스크린샷 2022-07-08 오전 10.09.57.png>)

* 웹 어플리케이션을 구현하기 위해 \[웹 서버 환경]을 선택합니다.

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 10.10.23.png>)

* 기본 구성을 작성합니다.
  * 환경 이름을 작성합니다.&#x20;
  * 도메인 명은 환경명을 디폴트로 구성됩니다.
  * 플랫폼은 Tomcat을 선택하였습니다.
  * \[Jenkins]를 구성하기 전에 애플리케이션 코드에는 샘플 애플리케이션을 선택하여 \[Eleastic Beanstalk]의 정상 구동을 확인합니다.

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 10.10.47.png>)

* 서비스 환경에 맞는 추가 옵션 구성을 통해 몇 가지를 더 설정합니다.

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 10.11.19.png>)

## 구현하기 - \[Jenkins] 인스턴스 생성편

* EC2를 활용하여 \[Jenkins]를 구성합니다.&#x20;
* \[Jenkins]에서 활용해야하는 다음 2가지 IAM Role 정책을 선택하고 생성합니다.



* Elastic Beanstalk admin access

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 10.15.10.png>)

* S3 access

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 10.15.58.png>)

* 인스턴스 추가를 수행합니다.
  * 각 개발환경에 맞는 리눅스를 선택합니다.
  * 버전 및 벤더 별로 명령어는 상의하기 때문에 예제를 참고하여 해당 환경에 맞는 스크립트를 작성합니다. (예제 : Ubuntu Server 18.04)
  * 테스트 가능한 최소 인스턴스 유형을 선택합니다. (t2.small)
  * 인스턴스의 보안그룹을 생성합니다.
    * 22 포트 : 내부에서만 접속 가능하도록 설정
    * 80 포트 : Git Webhook을 받아야 하기 때문에 위치 무관으로 설정
  * 생성해둔 IAM Role 정책을 연결합니다.

## 구현하기 - \[Jenkins] 인스턴스 구성편

* 인스턴스에 접속하여 다음의 스크립트를 수행 및 확인합니다.

```bash
# 자바 설치

# 패키지 관리툴 업데이트
$ sudo apt update
# 자바 설치
$ sudo apt install openjdk-8-jdk
# 버전 체크
$ java -version
# 홈 디렉토리 확인
$ update-alternatives --get-selections | grep ^java
# JAVA_HOME 설정
$ sudo vi /etc/environment
# 수정사항 적용을 위한 file load
$ source /etc/environment
# JAVA_HOME 확인
$ echo $JAVA_HOME

# AWS CLI 설치

# 설치
$ sudo apt install awscli
# 확인
$ aws --version

# Jenkins 확인

# 시스템 저장소에 jenkin 저장소 키 추가 
$ wget -q -O - <https://pkg.jenkins.io/debian/jenkins.io.key> | sudo apt-key add -
# 이전 단계 서명관련 오류시 아래 명령어로 재시도
$ wget -q -O - <https://pkg.jenkins.io/debian-stable/jenkins.io.key> | sudo apt-key add -
# soure.list에 데비안 패키기 저장소 추가
$ sudo sh -c 'echo deb <http://pkg.jenkins.io/debian-stable> binary/ > /etc/apt/sources.list.d/jenkins.list'
# 패키지 업데이트
$ sudo apt update
# Jenkins 설치
$ sudo apt install jenkins
# Jenkins 상태 확인
$ systemctl status jenkins

#Nginx 설치

# 설치
$ sudo apt install nginx
# 확인
$ systemctl status nginx

# Reserve Proxy 설정

# location 내용변경
# proxy_pass <http://localhost:8080>;
# proxy_set_header X-Real-IP $remote_addr;
# proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
# proxy_set_header Host $http_host;
$ sudo vi /etc/nginx/sites-available/default
# 재시작
$ sudo systemctl restart nginx

# Jenkin 초기설정 비밀번호 확인
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

## \[S3] 생성&#x20;

* 특별한 설정은 하실 필요가 없습니다. 구글링하여 기본 생성을 진행합니다.

## \[Jenkins] 파이프라인 구성

* Freestyle, Pipeline 방식을 선택하여 구성합니다. (본 예제는 Pipeline 방식)
* Blue Ocean Plugin 설치 (Pipeline 시각화 용)

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 11.20.14.png>)

* Blue Ocean 접근

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 11.21.17.png>)

* 알맞는 저장소를 선택합니다.

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 11.22.16.png>)

* 해당 저장소에 Access Token을 설정합니다.

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 11.23.27.png>)

* Webhook 등록을 진행합니다.

![](<../../.gitbook/assets/스크린샷 2022-07-08 오전 11.35.22.png>)

* 새 파이프 라인을 스텝별로 구성하고, 아래 그림과 shell script를 참고하여 각 단계별로 Shell Script가 수행되도록 작성을 진행합니다.

<img src="../../.gitbook/assets/스크린샷 2022-07-08 오전 11.26.12.png" alt="" data-size="original">

```bash
pipeline {
  agent any
  stages {

    /* 각 빌드 툴에 알맞는 빌드를 수행합니다. */
    stage('build') {
      steps {        
        sh './gradlew clean build'
      }
    }
    /* S3 upload 수행 */
    stage('upload') {
      steps {
        sh 'aws s3 cp build/libs/application.war s3://jenkins-deploy-application/application_${BUILD_TAG}.war --region ap-northeast-2'
      }
    }

    stage('deploy') {
      steps {
        sh 'aws elasticbeanstalk create-application-version --region ap-northeast-2 --application-name SpringbootThymeleaf --version-label ${BUILD_TAG} --source-bundle S3Bucket="jenkins-deploy-application",S3Key="application_${BUILD_TAG}.war"'
        sh 'aws elasticbeanstalk update-environment --region ap-northeast-2 --environment-name springbootthymeleaf-sample --version-label ${BUILD_TAG}'
      }
    }

  }
}
```

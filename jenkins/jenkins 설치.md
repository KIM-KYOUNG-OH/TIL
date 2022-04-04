# Jenkins란?

- CI/CD를 제공하는 Tool

# CI/CD란?

- Continuous Integration(CI)
    - 지속적인 통합
- Continuous Delivery(CD)
    - 지속적인 배포
- CI/CD는 빌드, 테스트, 배포 프로세스를 자동화하여 소프트웨어 품질과 개발 생산성을 높이는 것을 말함

# 왜 Jenkins를 사용할까?

- Jenkins가 등장하기 전에는 일정 시간마다 개발자가 직접 빌드를 하는 비용이 존재했다(Nightly Build라고 불림)
- Jenkins는 Git과 연동하여 소스 커밋이 감지되면 자동으로 테스트와 빌드, 배포를 진행하여 개발 생산성을 높일 수 있다
- Jenkins는 오픈소스여서 다양한 플러그인을 지원한다

---

# Jenkins 설치 전제 조건

- 최소 256 MB RAM, 1GB Drive Space 필요
- Jenkins를 실행하기 위해선 OpenJDK@11이 필요하다

# Linux에서 Jenkins 사용하기

# 원격 서버 접속

```bash
sudo ssh root@45.119.144.249 -p 8080
```

# Jenkins 설치

- yum(패키지 매니저)를 사용

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
sudo yum upgrade
# Add required dependencies for the jenkins package
sudo yum install java-11-openjdk
sudo yum install jenkins
```

# Jenkins 실행

```bash
$ sudo systemctl enable jenkins
$ sudo systemctl start jenkins
$ sudo systemctl restart jenkins
```

# Jenkins 상태 확인

```bash
$ sudo systemctl status jenkins
$ ps -ef | grep jenkins
```

# Jenkins 포트 변경

```bash
$ sudo vi /lib/systemd/system/jenkins.service
$ systemctl daemon-reload
```

- JENKINS_PORT : 9100으로 수정

# 주의점

- 클라우드 서버를 사용할 경우 해당 서버로 들어오는 인바운드 트래픽에 대해서 IP 주소와 포트 단위로 접근을 제한할 수 있는데 해당 설정이 적절한지 확인해야 한다(NCP는 ‘ACG’, AWS는 ‘보안 그룹 설정’)

# 방화벽 설정

```bash
YOURPORT=9100
PERM="--permanent"
SERV="$PERM --service=jenkins"

firewall-cmd $PERM --new-service=jenkins
firewall-cmd $SERV --set-short="Jenkins ports"
firewall-cmd $SERV --set-description="Jenkins port exceptions"
firewall-cmd $SERV --add-port=$YOURPORT/tcp
firewall-cmd $PERM --add-service=jenkins
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload
```

# Jenkins initial setup

- 아래와 같다

# MacOS에서 Jenkins 사용하기

# Jenkins 설치

- homeBrew(패키지 매니저)를 사용

```bash
$ brew install jenkins
```

# Jenkins 실행

```bash
$ brew services start jenkins
```

# Jenkins initial setup

- 브라우저를 통해 Jenkins Setup Wizard 접속

    <img width="650" src="https://user-images.githubusercontent.com/66231761/161578797-b6ba6564-7218-4a27-8171-d9090487ad7b.png"/>
    
- Administrator password 찾기

```bash
$ sudo cat /Users/kimgyeong-o/.jenkins/secrets/initialAdminPassword
```

# 기본 플러그인 설치

<img width="650" src="https://user-images.githubusercontent.com/66231761/161579217-8dd7877c-8465-4e80-8376-14da81c2aa66.png"/>

# 관리자 계정 생성

id: ~

password: ~

# Jenkins 설치 완료

<img width="650" src="https://user-images.githubusercontent.com/66231761/161579309-dc735bdb-45f0-4188-9c32-346b9ab6abfe.png"/>

# Jenkins 종료

```bash
$ brew services stop jenkins
```

# Jenkins metadata 경로

```bash
$ cd ~/.jenkins
```

# Jenkins port 변경

- 아래의 경로로 이동

```bash
$ cd /usr/local/Cellar/jenkins/2.x.x/homebrew.mxcl.jenkins.plist
```

- port 변경

```bash
—-httpPort=9090
```

# Reference

- [https://narup.tistory.com/179](https://narup.tistory.com/179)
- [https://www.jenkins.io/doc/book/getting-started/](https://www.jenkins.io/doc/book/getting-started/)
- [https://memostack.tistory.com/74](https://memostack.tistory.com/74)
- [https://medium.com/@vishnuteja/install-jenkins-as-a-service-on-macos-and-change-port-number-9aa097e5cfbf](https://medium.com/@vishnuteja/install-jenkins-as-a-service-on-macos-and-change-port-number-9aa097e5cfbf)

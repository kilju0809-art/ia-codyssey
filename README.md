1. 추천 저장소 구조

text
development-workstation/
├── README.md
├── Dockerfile
├── .gitignore
├── app/
│   └── index.html
├── logs/
│   ├── terminal.log
│   └── docker.log
└── screenshots/
    ├── 01-terminal.png
    ├── 02-permissions.png
    ├── 03-docker-check.png
    ├── 04-container.png
    ├── 05-web-browser.png
    ├── 06-bind-mount.png
    ├── 07-volume-persistence.png
    └── 08-github-vscode.png
logs/와 screenshots/는 실습 결과를 정리하기 위한 추천 구조입니다. 실제 파일명은 자유롭게 변경해도 됩니다.

2. README.md 템플릿
아래 문서를 복사해 저장소의 README.md에 붙여 넣으세요. 대괄호로 표시한 항목은 본인의 환경과 실제 실행 결과로 바꾸면 됩니다.

작성


개발 워크스테이션 구축 미션
1. 프로젝트 개요
미션 목표
개발에 필요한 터미널, Docker, Git/GitHub 환경을 구축하고, 컨테이너 기반 웹 서버를 실행하여 재현 가능한 개발 환경을 구성한다.

주요 수행 내용
터미널을 활용한 디렉토리 및 파일 관리
Linux 파일 권한 확인 및 변경
WSL2 및 Docker Desktop 환경 점검
Docker 이미지 다운로드 및 컨테이너 운영
Dockerfile을 이용한 커스텀 웹 서버 이미지 제작
포트 매핑 및 웹 서버 접속 검증
바인드 마운트의 변경 사항 반영 검증
Docker 볼륨의 데이터 영속성 검증
Git 설정 및 GitHub 저장소 연동
실습 결과와 트러블슈팅 문서화
2. 실행 환경

항목	사용 환경
호스트 OS	Windows [버전]
Linux 환경	WSL2 / [Ubuntu 등 배포판 및 버전]
Shell	[bash 등]
터미널	[Windows Terminal 등]
Docker 실행 환경	Docker Desktop
Docker CLI	[실제 버전]
Git	[실제 버전]
에디터	Visual Studio Code
저장소	[GitHub 저장소 URL]
환경 확인 명령어

bash
pwd
uname -a
cat /etc/os-release
git --version
docker --version
docker info
실제 실행 결과:


text
[여기에 실제 출력 결과 기록]
docker info에서 오류가 발생했다면 오류 메시지와 해결 방법을 기록한다. 인증 정보, 사용자명 등 불필요한 개인 정보는 공개하지 않는다.
3. 수행 항목 체크리스트
터미널 기본 명령어 실습
절대 경로와 상대 경로 확인
파일 및 디렉토리 권한 확인과 변경
Docker CLI 및 엔진 동작 확인
hello-world 실행
Ubuntu 컨테이너 진입 및 명령어 실행
이미지 및 컨테이너 관리
컨테이너 로그 및 리소스 확인
Dockerfile 작성 및 커스텀 이미지 빌드
포트 매핑 및 웹 서버 접속 확인
바인드 마운트 변경 반영 확인
Docker 볼륨 데이터 영속성 확인
Git 사용자 정보 및 기본 브랜치 설정
GitHub 저장소 생성 및 푸시
VS Code와 GitHub 연동
트러블슈팅 2건 이상 작성
민감정보 점검 및 제출 자료 정리
4. 터미널 기본 조작
4.1 현재 위치 및 파일 목록 확인
사용 명령어:


bash
pwd
ls -la
실행 결과:


text
[실제 출력 결과]
4.2 디렉토리 생성 및 이동

bash
mkdir -p ~/development-workstation/practice
cd ~/development-workstation/practice
pwd
실행 결과:


text
[실제 출력 결과]
4.3 파일 생성 및 내용 확인

bash
touch sample.txt
echo "Terminal practice" > sample.txt
cat sample.txt
실행 결과:


text
[실제 출력 결과]
4.4 복사, 이동, 이름 변경 및 삭제

bash
cp sample.txt sample-copy.txt
mv sample-copy.txt renamed.txt
ls -la
rm renamed.txt
ls -la
실행 결과:


text
[실제 출력 결과]
4.5 절대 경로와 상대 경로
절대 경로: 루트 디렉토리(/)부터 시작하는 경로
상대 경로: 현재 작업 디렉토리를 기준으로 지정하는 경로
예시:


bash
pwd
cat ~/development-workstation/practice/sample.txt
cat sample.txt
설명:

[실제 경로를 기준으로 두 경로의 차이와 실행 결과 작성]

4.6 터미널 실습 증거
로그: [logs/terminal.log 링크]
스크린샷: [screenshots/01-terminal.png 링크]
5. 파일 권한 실습
5.1 파일 권한 확인 및 변경

bash
touch permission-test.sh
ls -l permission-test.sh
chmod 755 permission-test.sh
ls -l permission-test.sh
실행 결과:


text
[변경 전후의 실제 출력 결과]
5.2 디렉토리 권한 확인 및 변경

bash
mkdir permission-dir
ls -ld permission-dir
chmod 700 permission-dir
ls -ld permission-dir
실행 결과:


text
[변경 전후의 실제 출력 결과]
5.3 권한 해석
r: 읽기 권한
w: 쓰기 권한
x: 실행 권한. 디렉토리에서는 탐색 권한을 의미한다.
755: 소유자 rwx, 그룹 r-x, 기타 사용자 r-x
644: 소유자 rw-, 그룹 r--, 기타 사용자 r--
파일 권한과 디렉토리 권한의 차이:

[실습 결과를 바탕으로 작성]

증거: [screenshots/02-permissions.png 링크]

6. Docker 설치 및 기본 점검
6.1 Docker 버전 확인

bash
docker --version
실행 결과:


text
[실제 출력 결과]
6.2 Docker 엔진 상태 확인

bash
docker info
실행 결과:


text
[실제 출력 결과 또는 오류와 해결 방법]
6.3 hello-world 실행

bash
docker run --rm hello-world
검증 결과:

[실제 실행 결과와 성공 여부 기록]

6.4 이미지 목록 확인

bash
docker images
실행 결과:


text
[실제 출력 결과]
6.5 Docker 점검 증거
로그: [logs/docker.log 링크]
스크린샷: [screenshots/03-docker-check.png 링크]
7. 컨테이너 실행 및 운영
7.1 Ubuntu 컨테이너 실행

bash
docker run -it --name ubuntu-practice ubuntu bash
컨테이너 내부에서 실행:


bash
pwd
ls -la
echo "Hello from Ubuntu container"
exit
실행 결과:


text
[실제 출력 결과]
7.2 컨테이너 상태 확인

bash
docker ps
docker ps -a
실행 결과:


text
[실제 출력 결과]
7.3 컨테이너 시작, 중지 및 로그 확인

bash
docker start ubuntu-practice
docker stop ubuntu-practice
docker ps -a
로그를 남기는 테스트 컨테이너 예시:


bash
docker run -d --name log-test ubuntu bash -c 'while true; do echo "container running"; sleep 5; done'
docker logs --tail 10 log-test
docker stats --no-stream log-test
docker stop log-test
docker rm log-test
실행 결과:


text
[실제 출력 결과]
7.4 attach와 exec의 차이
docker attach: 실행 중인 컨테이너의 주 프로세스 입출력에 연결한다.
docker exec: 실행 중인 컨테이너 안에서 별도의 명령이나 셸을 실행한다.
관찰 결과:

[실제 실행 결과와 두 명령의 차이 작성]

증거: [screenshots/04-container.png 링크]

8. Dockerfile 기반 커스텀 웹 서버
8.1 베이스 이미지 및 커스터마이징
베이스 이미지: nginx:alpine
커스터마이징: 직접 작성한 정적 웹 페이지 적용
목적: 이미지 빌드와 컨테이너 실행을 재현하고 웹 서비스 응답을 검증한다.
8.2 프로젝트 파일 구조

text
development-workstation/
├── Dockerfile
└── app/
    └── index.html
8.3 웹 페이지 작성
app/index.html:

<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Development Workstation</title>
</head>
<body>
  <h1>Development Workstation</h1>
  <p>Docker custom image is running.</p>
</body>
</html>
8.4 Dockerfile 작성
Dockerfile:


dockerfile
FROM nginx:alpine

LABEL org.opencontainers.image.title="development-workstation"

ENV APP_ENV=development

COPY app/ /usr/share/nginx/html/
커스터마이징 설명:

FROM: NGINX 기반 이미지 사용
LABEL: 이미지의 목적을 메타데이터로 기록
ENV: 실행 환경 설정 예시
COPY: 직접 작성한 정적 웹 페이지를 이미지에 포함
8.5 이미지 빌드

bash
docker build -t workstation-web:1.0 .
docker images
실행 결과:


text
[실제 빌드 로그 및 이미지 목록]
8.6 컨테이너 실행 및 포트 매핑

bash
docker run -d --name workstation-web -p 8080:80 workstation-web:1.0
docker ps
호스트 포트: 8080
컨테이너 포트: 80
포트 매핑은 호스트에서 접근한 요청을 컨테이너의 웹 서버 포트로 전달하기 위해 사용한다.

8.7 웹 서버 접속 검증

bash
curl -i http://localhost:8080
실행 결과:


text
[실제 HTTP 응답 및 페이지 내용]
브라우저에서도 http://localhost:8080에 접속하여 주소창과 웹 페이지가 함께 보이도록 캡처한다.

브라우저 증거: [screenshots/05-web-browser.png 링크]
9. 바인드 마운트 검증
9.1 호스트 디렉토리 연결
PowerShell이 아닌 WSL 터미널에서 저장소 루트 디렉토리로 이동한 뒤 실행한다.


bash
docker run -d --name bind-test -p 8081:80 \
  --mount type=bind,source="$(pwd)/app",target=/usr/share/nginx/html,readonly \
  nginx:alpine
9.2 변경 전 내용 확인

bash
curl -s http://localhost:8081
실행 결과:


text
[변경 전 실제 페이지 내용]
9.3 호스트 파일 수정 후 확인
app/index.html의 문구를 변경한 뒤 실행한다.


bash
curl -s http://localhost:8081
실행 결과:


text
[변경 후 실제 페이지 내용]
9.4 검증 결과
[호스트 파일의 변경 사항이 컨테이너에 반영되었는지 설명]

증거: [screenshots/06-bind-mount.png 링크]

10. Docker 볼륨 영속성 검증
10.1 볼륨 생성

bash
docker volume create workstation-data
docker volume ls
실행 결과:


text
[실제 출력 결과]
10.2 볼륨 연결 및 데이터 기록

bash
docker run -d --name volume-test \
  --mount source=workstation-data,target=/data \
  ubuntu sleep infinity

docker exec volume-test sh -c 'echo "persistent data" > /data/result.txt'
docker exec volume-test cat /data/result.txt
실행 결과:


text
[실제 데이터 출력]
10.3 컨테이너 삭제

bash
docker rm -f volume-test
docker ps -a
docker volume ls
실행 결과:


text
[컨테이너 삭제 및 볼륨 확인 결과]
10.4 새 컨테이너에서 데이터 확인

bash
docker run -d --name volume-test-2 \
  --mount source=workstation-data,target=/data \
  ubuntu sleep infinity

docker exec volume-test-2 cat /data/result.txt
실행 결과:


text
[이전과 동일한 데이터가 출력된 실제 결과]
10.5 검증 결과
컨테이너를 삭제한 후 동일한 볼륨을 새 컨테이너에 연결하여 데이터를 확인했다.

[실제 관찰 결과와 볼륨의 역할 작성]

증거: [screenshots/07-volume-persistence.png 링크]

11. Git 설정 및 GitHub 연동
11.1 Git 사용자 정보 설정
아래 값은 본인의 GitHub 계정에 맞게 변경한다.


bash
git config --global user.name "YOUR_NAME"
git config --global user.email "YOUR_EMAIL"
git config --global init.defaultBranch main
설정 확인:


bash
git config --list
git --version
실행 결과:


text
[실제 출력 결과. 이메일 등 공개가 불필요한 개인정보는 마스킹]
11.2 저장소 초기화 및 커밋
GitHub에서 저장소를 생성한 후 로컬 프로젝트에서 실행한다.


bash
git init
git branch -M main
git add README.md Dockerfile app .gitignore
git commit -m "docs: add workstation mission"
git status
실행 결과:


text
[실제 커밋 및 상태 확인 결과]
11.3 GitHub 원격 저장소 연결

bash
git remote add origin YOUR_REPOSITORY_URL
git remote -v
git push -u origin main
이미 origin이 등록되어 있다면 git remote add를 반복하지 말고 기존 원격 주소를 확인한다.

저장소 URL: [GitHub 저장소 URL]
VS Code 연동 증거: [screenshots/08-github-vscode.png 링크]
액세스 토큰, 비밀번호, 개인키, 인증 코드가 포함된 화면은 캡처하거나 저장소에 업로드하지 않는다.
12. 트러블슈팅
사례 1. Docker 엔진 연결 실패
문제: [실제 오류 메시지]
원인 가설: Docker Desktop 미실행, WSL 통합 설정 또는 엔진 연결 문제
확인: docker info, Docker Desktop 상태, WSL 설정 확인
해결: [실제로 수행한 조치]
검증: [재실행 결과]
사례 2. 웹 서버 접속 실패
문제: [실제 접속 오류]
원인 가설: 포트 매핑 오류, 컨테이너 종료 또는 웹 서버 오류
확인: docker ps -a, docker logs workstation-web, curl -i http://localhost:8080
해결: [실제로 수행한 조치]
검증: [재실행 결과]
13. 핵심 개념 정리
이미지와 컨테이너
이미지: 애플리케이션과 실행에 필요한 파일 및 설정을 포함하는 템플릿
컨테이너: 이미지를 기반으로 실행하는 격리된 프로세스 환경
포트 매핑
호스트 포트와 컨테이너 포트를 연결하여 호스트에서 컨테이너 내부 서비스에 접근할 수 있게 한다.

바인드 마운트와 볼륨
바인드 마운트: 호스트의 특정 경로를 컨테이너 내부 경로와 연결한다.
볼륨: Docker가 관리하는 저장 공간으로, 컨테이너를 삭제해도 별도로 삭제하지 않는 한 데이터가 유지된다.
Git과 GitHub
Git: 로컬 소스 코드의 변경 이력을 관리하는 버전 관리 도구
GitHub: 원격 저장소, 협업 및 코드 리뷰 기능을 제공하는 플랫폼
14. 검증 결과 요약

검증 항목	확인 방법	결과
터미널 기본 명령	pwd, ls -la, cp, mv, rm	[결과]
파일 권한	ls -l, chmod	[결과]
Docker 환경	docker --version, docker info	[결과]
hello-world	docker run --rm hello-world	[결과]
컨테이너 운영	docker ps -a, docker logs, docker stats	[결과]
커스텀 이미지	docker build	[결과]
포트 매핑	curl -i http://localhost:8080	[결과]
바인드 마운트	호스트 파일 변경 후 curl	[결과]
볼륨 영속성	컨테이너 삭제 후 데이터 조회	[결과]
Git/GitHub	git status, git remote -v, git push	[결과]
VS Code 연동	저장소 연결 화면	[결과]
15. 회고
이번 미션을 통해 배운 점:

[직접 수행한 작업 중 이해도가 높아진 개념과 새롭게 알게 된 점 작성]

어려웠던 점과 해결 방법:

[실제 트러블슈팅 경험 작성]

추가 학습 계획:

[Docker Compose, CI/CD, GitHub Actions 등 다음 학습 목표 작성]

3. README를 채우는 데 필요한 실제 실행 순서
템플릿을 한 번에 채우기보다는 아래 순서로 실습하면 편합니다.

순서

먼저 실행할 작업

README에 기록할 곳

1

pwd, ls -la, 파일 조작

4번

2

chmod, ls -l

5번

3

Docker 버전 및 엔진 확인

6번

4

Ubuntu 컨테이너와 운영 명령

7번

5

Dockerfile 작성 및 이미지 빌드

8번

6

바인드 마운트 및 웹 페이지 변경

9번

7

볼륨 생성 및 데이터 재확인

10번

8

Git 설정 및 GitHub 푸시

11번

9

실제 오류와 해결 과정 정리

12번

4. Windows + WSL2에서 주의할 점
명령어 실행 위치: Docker 관련 명령은 Docker Desktop의 WSL 통합이 활성화된 Linux 배포판에서 실행하는 것을 권장합니다.

경로: WSL 내부의 ~/development-workstation 경로와 Windows의 C:\Users\... 경로는 서로 다릅니다. README에는 다른 평가자도 사용할 수 있는 상대 경로와 대체 방법을 설명하세요.

포트 충돌: 8080 포트가 이미 사용 중이면 8082처럼 다른 호스트 포트를 선택하고 실행 명령과 접속 주소를 모두 일치시키세요.

바인드 마운트: --mount의 source 경로가 실제 디렉토리인지 확인하세요. 위 예시는 저장소 루트에서 실행하는 것을 전제로 합니다.

권한: WSL에서는 파일 권한이 Linux와 Windows 파일 시스템에서 다르게 동작할 수 있으므로, 권한 실습은 WSL의 Linux 홈 디렉토리에서 진행하는 것이 좋습니다.

민감정보: git config --list를 그대로 공개하기 전에 이메일과 기타 개인 정보를 확인하세요. 토큰과 비밀번호는 절대 기록하지 마세요.

5. 제출 직전 체크
제출 점검표
9/9 완료


README의 모든 필수 섹션 작성

명령어와 출력 결과가 함께 기록됨

Dockerfile 및 웹 페이지 파일 업로드

포트 매핑 접속 증거에 주소창이 보임

바인드 마운트 변경 전후 비교 기록

컨테이너 삭제 전후 볼륨 데이터 비교

트러블슈팅 사례 2건 이상 작성

GitHub 저장소에서 파일과 이미지가 열림

토큰·비밀번호·개인키 등 민감정보 확인

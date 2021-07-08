## Docker

#### Docker 내 같은 환경에서 개발할 수 있다!

dockerfile : 다운 받은 이미지를 커스텀   
필요한 라이브러리 등을 RUN 명령어를 통해 이미지 생성 시 설치   
컨테이너 생성 시 실행은 CMD 명령어   
#### Dockerfile과 docker-compose.yml이 변경됐을때 다시 이미지를 생성해야한다
### 도커 명령어
```
docker ps 
-> 내 docker의 실행 중인 컨테이너 목록을 보여줌 ( -a 옵션을 붙이면 모든 컨테이너 )
```
```
docker images
-> 내 docker의 이미지 목록을 보여줌
```
```
docker run {옵션} {이미지명}:{태그}
ex) docker run -it node 
-> 내 docker에서 node 라는 이미지를 찾고 없으면 docker hub에서 다운 받아 컨테이너 실행 후 진입
```
```
docker build -t {이미지명}
-> 현재dir/ dockerfile의 명령어들을 토대로 빌드 한 image를 생성
```
옵션 ㄱ   
* docker-compose.yml 을 사용해 편하게 빌드 가능
```
docker-compose build
-> 이미지 생성
```
```
docker-compose up
-> 빌드, 컨테이너 생성 후 실행
```


참고 : https://www.yalco.kr/36_docker/

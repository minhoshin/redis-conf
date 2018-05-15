# redis-conf

```
* redis master 
redis master 설정(설정 파일 참고) 완료 후
$ redis-server ./6379.conf 로 실행

* redis slave
redis slave 설정 의 경우 master 설정과 동일하지만 아래 save 와 appendonly 그리고 slaveof 설정 변경 필요 

#save 900 1
#save 300 10
#save 60 10000
첫 번째로 위의 #을 풀고

두 번째로 appendonly yes 를 no 로 변경하고

세번째로 slaveof 10.5.0.7 6379 // 마스터 아이피로 설정

* sentinel 설정
port 26379
sentinel monitor mymaster 10.5.0.7(master ip) 6379 2
sentinel monitor mymasterSe 10.5.0.7(master ip) 6380 2

> redis-sentinel sengtinel.conf 실행

* sentinel 접속
redis-cli -p 26379

* log 확인
```

```
redis 복구

redis master는 aof 사용
redis slave는 rdb 사용

복구시 redis slave의 rdb, aof 삭제 후 slave 가동
redis-server *.conf

서버의 rdb, aof 모두 복구
```


```
Docker 컨테이너에서 레디스 컴파일 하기

docker 에서 local-centos 이름으로 centOS 6.8 컨테이너 띄우기
docker run -itd --name local-centos centos:6.8 bash

docker 에서 local-centos 컨테이너 쉘로 들어가기
docker attach local-centos

docker 컨테이너 쉘에서 redis 디렉토리 만들기
mkdir redis

docker 컨테이너에서 잠시 나오기
ctrl 을 누른 상태에서 p 1번 q 1번 누르기

docker 컨테이너로 파일 복사하기
docker cp redis-4.0.6.tar.gz local-centos:/redis/

다시 docker 에서 local-centos 컨테이너 쉘로 들어가기
docker attach local-centos

docker 컨테이너에서 redis 폴더 들어가서 압축 해제
tar -xvzf redis-4.0.9.tar.gz

docker 컨테이너에서 redis 압축 해제 폴더로 이동
cd redis-4.0.9

docker 컨테이너에서 gcc 컴파일러 설치
yum install gcc

docker 컨테이너 redis-4.0.9 폴더에서
make distclean 실행 후
make 로 컴파일 실행
src 폴더 생성 완료

docker 컨테이너 redis-4.0.9 폴더에서 src 폴더를 압축
tar -cvzf redisCompile.tar.gz ./src 로 압축

docker 컨테이너에서 잠시 나온 후 현재 폴더로 가지고 오기
docker cp local-centos:/redis/redisCompile.tar.gz ./

설치 서버로 sftp 로 접속 및 파일 업로드
put redisCompile.tar.gz

설치 서버에서 접속 해제 src 폴더로 압축 해제
tar -xvzf redis-4.0.9.tar.gz

압축 해제된 src 폴더를 bin 으로 이름 변경 및 redis 폴더 생성 후 하위 폴더로 이동

루트폴더에서
vi .bashrc 입력 후 아래 내용 입력
PATH=$PATH:/home/partner/redis/bin
위 내용 입력 후 빠져 나오기

source .bashrc 로 등록
```
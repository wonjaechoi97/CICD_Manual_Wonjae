
```python
version: '3'

services:                                               # 컨테이너내의 서비스
    jenkins:                                            # 서비스명: jenkins
        image: jenkins/jenkins:lts                      # 컨테이너 생성 시 jenkins/jenkins:lts 이미지를 기반 이미지로 사용 
        container_name: jenkins                         # 컨테이너 이름
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock # /var/run/docker.sock와 컨테이너 내부의 /var/run/docker.sock를 연결
            - /jenkins:/var/jenkins_home                # /jenkins 폴더와 /var/jenkins_home 폴더를 연결
        ports:
            - "9090:8080"                               # 포트 매핑, Aws의 9090 포트와 컨테이너의 8080 포트를 연결
        privileged: true                                # 컨테이너 시스템의 주요 자원에 연결할 수 있게 설정(기본적으로 false)
        user: root # 젠킨스에 접속할 유저 계정 (root로 할 경우 관리자)
    db:                                                 # 서비스 명: db
        image: mysql:8.0.29                             # 컨테이너 생성 시 mysql:8.0.29 이미지를 기반 이미지로 사용 
        container_name: mysql 
        command: # 아래의 4줄 : mysql 설정 부분. 인증 방식, encoding 방식 설정
            - --default-authentication-plugin=mysql_native_password
            - --character-set-server=utf8mb4
            - --collation-server=utf8mb4_unicode_ci
            - --range_optimizer_max_mem_size=16777216
        restart: always # 컨테이너 항상 재시작
        environment:
            MYSQL_DATABASE: *******                     # 사용하는 db
	    MYSQL_USER: ******                          # root 패스워드
            MYSQL_PASSWORD: ********                    # 접속하는 user 
            MYSQL_ROOT_PASSWORD: *********              # user 패스워드
            TZ: Asia/Seoul
        volumes:        
            - ./mysql/data:/var/lib/mysql               # 데이터 마운트 용도, ./mysql/data와 컨테이너 내의 /var/lib/mysql연결
        ports:
            - 3306:3306                                 # 포트 매핑, Aws의 3306 포트와 컨테이너의 3306 포트 연결
```

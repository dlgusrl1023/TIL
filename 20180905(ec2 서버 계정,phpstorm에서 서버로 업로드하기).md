# ec2 서버 계정
---
## User 생성하기 / Passwd 설정하기 / 삭제하기
```shell
# user를 생성한다. 
[root@ip-172-31-27-144 ec2-user] adduser [계정명]

# password를 설정한다. 
[root@ip-172-31-27-144 ec2-user] passwd [계정명]
```
비밀번호 설정 후 
```shell
[root@ip-172-31-27-144 ec2-user]# visudo
```
아래와 같은 코드를 찾아 수정한다.

```shell
root            ALL=(ALL)       ALL       # 찾을 코드
[계정명]        ALL=(ALL)       ALL       # 계정명을 추가
```
같은 key pair로 로그인 할 수 있또록 새 생성한 사용자 계정으로 ec2-user의 것을 복사
```shell
[root@ip-172-31-27-144 ec2-user] mkdir /home/[계정명]/.ssh
[root@ip-172-31-27-144 ec2-user] cp /home/ec2-user/.ssh/ /home/[계정명]/.ssh
```
복사한 key pair의 소유자를 [계정명]으로 변경
```shell
[root@ip-172-31-27-144 ec2-user] chown -R [계정명]:[계정명] /home/[계정명]/.ssh

# user 삭제 
[root@ip-172-31-27-144 ec2-user] userdel [계정명]

# user + home directory 삭제 
[root@ip-172-31-27-144 ec2-user] userdel -r [계정명]
```
---
## User 조회하기
```shell
# 모든 계정 조회
[ec2-user@ip-172-31-27-144 ~]$ cat /etc/passwd

# bash 사용 권한 (접속 권한) 있는 계정 조회
[ec2-user@ip-172-31-27-144 ~]$ grep /bin/bash /etc/passwd
```
---
## group 생성 / 소속 관리 / 삭제 / 조회하기
```shell
# 그룹 생성
[root@ip-172-31-27-144 ec2-user]# groupadd [그룹명]

# 그룹에 유저 소속시키기
[root@ip-172-31-27-144 ec2-user]# usermod -a -G [그룹명] [사용자명]

# 그룹 삭제
[root@ip-172-31-27-144 ec2-user]# groupdel [그룹명]

# 그룹 조회
[root@ip-172-31-27-144 ec2-user]# cat /etc/group
```
---
## 서버 password 접속을 활성화 / 비활성화
```shell
[root@ip-172-31-27-144 ec2-user]# vim /etc/ssh/sshd_config
```
아래와 같은 코드를 찾아서 수정한다.
```shell
#PasswordAuthentication no
PasswordAuthentication yes
```
---
# phpstorm 에서 서버에 업로드하기

* ec2 서버의 /var/www/html/Work/ 에 만약에 파일이 있다고 했을 경우
* ec2-user 로 phpstrom에 로그인 한 경우
* ec2-user 에게 권한을 줌
```shell
[root@ip-172-31-27-144 ec2-user] chown -R root:ec2-user /var/www/html/Work/
[root@ip-172-31-27-144 ec2-user] chmod -R 775 /var/html/Work/
[root@ip-172-31-27-144 ec2-user] chown root:ec2-user /var/www/html
[root@ip-172-31-27-144 ec2-user] chmod  775 /var/html/
```
* **중요** : 파일을 올릴 디렉토리안에서 ls -al를 했을 때에 '..' 부분이 [root : root] 가 아니라
[root : ec2-user]가 되게 만들어줘야한다.



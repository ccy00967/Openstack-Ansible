 
 ssh 연결 구성하기 - 컨트롤러, 컴퓨트 전부
키 생성
~~~
ssh-keygen -t rsa -b 4096 -N "" -f ~/.ssh/id_rsa
~~~

키 등록
~~~
ssh-copy-id -i ~/.ssh/id_rsa.pub root@172.29.236.12
~~~

~~~
sudo passwd root # 비밀번호 설정하기

sudo vim /etc/ssh/sshd_config # 안에 들어가서 다음 항목 수정
----
PermitRootLogin yes
PasswordAuthentication yes
----
~~~

ssh 재시작
~~~
sudo systemctl restart ssh
~~~

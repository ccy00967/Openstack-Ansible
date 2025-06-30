
horizon에서 이미지가 업로드 되지 않기에 여기에 왔을 것이다! - 이걸 작성하는 이유이다! -

직접 명령어로 이미지를 생성하는 방법은 먼저, 아래 명령어를 통해 유틸리티 컨테이너에 접속한다.

```
lxc-attach -n `lxc-ls -1 | grep utility | head -n 1`
```
그리고 아래 명령어를 통해 환경변수를 가져온다.
```
. ~/openrc
```

그 후 필요한 운영체제 이미지를 wget으로 다운받고, 아래의 glance 명령어로 openstack과 동일하게 생성한다.
```
glance image-create --name foo --disk-format=qcow2 \
                    --container-format=bare --visibility=public \
                    --file /tmp/foo.img
```


```
apt update
apt install wget -y
```

```
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img -O ubuntu-22.04.qcow2
```


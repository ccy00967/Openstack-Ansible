
cinder LVM 할당 확인
```
vgs

또는

losetup -a
```

```
mkdir -p /var/lib/cinder
```

```
dd if=/dev/zero of=/var/lib/cinder/cinder-volumes.img bs=1M count=51200
```

```
losetup --find --show /var/lib/cinder/cinder-volumes.img
```

```
pvcreate --metadatasize 2048 /dev/loop12
```

```
vgcreate cinder-volumes /dev/loop12
```

생성확인
```
vgs
```

서비스 재실행
```
systemctl restart cinder-volume
```




아래 설정들은 무시해도 좋다. just 혹시 몰라서 남겨둔다.

`openstack_user_config.yml` 파일 수정
```
storage_hosts:
  infra1:
    ip: 172.29.236.11
    container_vars:
      cinder_backends:
        lvm:
          volume_group: cinder-volumes
          volume_driver: cinder.volume.drivers.lvm.LVMVolumeDriver
          volume_backend_name: LVM_iSCSI
          iscsi_ip_address: "172.29.236.11"

```



`user_variables.yml` 파일 수정
```
cinder_volumes_container:
  is_metal: true

```

LVM 필터 설정
호스트의 LVM이 Cinder의 볼륨을 스캔하지 않도록 하려면, `/etc/lvm/lvm.conf` 파일에서 필터를 설정해야 합니다. 예를 들어, 다음과 같이 설정할 수 있습니다:
```
filter = [ "a|/dev/nvme0n1p3|", "r|.*|" ]

```

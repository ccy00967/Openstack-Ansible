뇬컨트롤러 호스트에서 검색!
```
systemctl status cinder-volume
```


/etc/openstack_deploy/user_variables.yml 에 추가하라!
```
# Cinder Backend 설정
cinder_backends:
  lvm:
    volume_driver: cinder.volume.drivers.lvm.LVMVolumeDriver
    volume_group: cinder-volumes
    volume_backend_name: lvm
    iscsi_protocol: iscsi
    target_helper: tgtadm

# Backend 활성화
cinder_backends_enabled:
  - lvm

```


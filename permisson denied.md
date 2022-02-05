# 문제
github actions로 CI/CD ~~Test는 없지만~~ 를 시도하던 중 빌드할때 이런 오류가 났다.
```
rsync: delete_file: unlink(/__pycache__/viewsets.cpython-39.pyc) failed: Permission denied (13)
```
workflow중에서 소스코드를 복사 하는 중에 Permission denied가 뜨는 것을 보고 바로 remote_path directory을 들어가봤다.

![](https://images.velog.io/images/jun-k0/post/5207782b-bbd6-45ca-b580-671bc1a74df7/image.png)

root 권한만 접근 가능하다.

# 해결

소유권은 root지만, actions로 접근하는 host가 달라서 permission 오류가 나나보다. 바로 chmod로 권한을 재설정 해주자.
```
sudo chown -R ubuntu:ubuntu ubuntu
```

![](https://images.velog.io/images/jun-k0/post/4c8c45d8-deeb-4600-9206-a059311a9bfd/image.png)

해결!


# 학습 환경 준비

<br>

## 스왑 공간 마운팅

docker 컨테이너 밖에서 다음을 실행

```bash
$ sudo systemctl disable nvzramconfig
$ sudo fallocate -l 4G /mnt/4GB.swap
$ sudo mkswap /mnt/4GB.swap
$ sudo swapon /mnt/4GB.swap
```

그리고 다음을 실행하여 /etc/fstab에 다음줄을 추가

```
$ sudo chmod 666 /etc/fstab 
$ sudo echo "/mnt/4GB.swap  none  swap  sw 0  0" >> /etc/fstab
$ sudo chmod 644 /etc/fstab
```

다음을 실행하여 /etc/fstab 파일에 위 사항이 반영되었는지 확인한다.
```
$ cat /etc/fstab
```

그리고 리부팅.

<br>

## PyTorch 준비

docker 안에서 다음을 실행하여 결과 확인.

```bash
$ python

>>> import torch
>>> print(torch.__version__)
>>> print('CUDA available: ' + str(torch.cuda.is_available()))
>>> a = torch.cuda.FloatTensor(2).zero_()
>>> print('Tensor a = ' + str(a))
>>> b = torch.randn(2).cuda()
>>> print('Tensor b = ' + str(b))
>>> c = a + b
>>> print('Tensor c = ' + str(c))
```

만약 설치가 안되어 있다면 다음으로 설치.

```bash
$ cd /jetson-inference/build
$ ./install-pytorch.sh
```

<br>


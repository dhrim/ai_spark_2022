
-----
# Jetson-Inference 프로젝트 설치 - docker 사용
<br><br><br><br>


# Docker로 환경 준비

https://github.com/dusty-nv/jetson-inference/blob/master/docs/aux-docker.md 를 기반으로 함.

docker를 사용하여 셋업하는 방법으로 [소스에서 빌드](setup_from_source.md)와 결과는 같다. 


<br>

## 프로젝트 다운로드

```bash
$ cd
$ git clone --recursive https://github.com/dusty-nv/jetson-inference

```

<br>

## docker 실행

```bash
$ cd ~/jetson-inference
$ docker/run.sh
```
10여분 소요된다.

소스에서 빌드한 build/aarch64/bin의 내용이 존재한다.

그리고 docker 컨테이너외부와 파일 공유를 위해 다음의 디렉토리가 마운트 되었다.
```bash
/jetson-inference/
	data/
	python/
		training/
			classification/
				data/
				models/
			detection/
				data/
				models/
```
외부의 ~/jetson-inference 가 docker 내의 /jetson-inference로 마운트 되었다.


<br>

## 실행 후의 상황

run.sh를 실행하면 docker 컨테이너 안에서 root 유저로 실행된다.

컨테이너 안에서 실행된 결과를 보전하기 위해서는 위에 언급된 폴더들 아래에 파일을 두어야 컨테이너 밖에서 해당 파일을 접근할 수 있다.
- /jetson-inference/data/
- /jetson-inference/python/training/classifcation/data/
- /jetson-inference/python/training/classifcation/models/
- /jetson-inference/python/training/detection/data/
- /jetson-inference/python/training/detection/models/

<br>

## 동작 확인

```bash
$ cd build/aarch64/bin
$ ./imagenet-camera.py
```

<br>

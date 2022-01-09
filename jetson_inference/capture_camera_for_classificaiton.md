
-----
# 분류를 위한 카메라 영상 캡쳐
<br><br><br><br>


# 분류 데이터 새로운 데이터 생성

https://github.com/dusty-nv/jetson-inference/blob/master/docs/pytorch-collect.md 를 기반으로 함.

<br>

## 프로그램 위치

aarch64/bin/ 혹은  tools/ 혹은 /usr/local/bin/ 에 camera-capture가 있다.

<br>

## 실행 준비

실행하기 전에 다음을 준비한다.

```bash
$ cd /jetson-inference/python/training/classification/data
$ mkdir new_data
$ echo "class_A
class_B" > new_data/labels.txt
```

<br>

## 실행

```bash
$ camera-capture

# 또는
$ camera-capture csi://0
```

![Untitled](images/image3.png)

<br>

## 옵션 설정

다음 옵션을 설정한다.

- Dataset Type : 'Classification'
- Dataset Path : 'jetson-inference/python/training/classification/data/new_data'
- Class Labels : 'jetson-inference/python/training/classification/data/new_data/labels.txt'

<br>

## 캡쳐

옵션을 설정하고

- Current Set : 'train'
- Current Class : 'class_A'

버튼 'Capture (space)'를 클릭한다. 클릭하는데로 jpg파일이 생성된다.

<br>

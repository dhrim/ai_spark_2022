
-----
# 물체 탐지 학습
<br><br>


# 물체 탐지 학습

https://github.com/dusty-nv/jetson-inference/blob/master/docs/pytorch-ssd.md 를 기반으로 함.

[docker](setup_by_docker.md)환경을 사용함.

<br>


## 필요 라이브러리 설치

```bash
$ cd /jetson-inference/python/training/detection/ssd
$ pip3 install -v -r requirements.txt
```

<br>

## 데이터 다운로드

```
$ cd /jetson-inference/python/training/detection/ssd/data
$ wget https://github.com/dhrim/jetson_image/raw/master/data/fruit.tar.gz
$ tar xvfz fruit.tar.gz
```

다음은 위 데이터의 원본이다. 시간 절약을 위해 위의 것으로 대신한다. 기록을 위해 남겨 둔다.
```bash
$ python open_images_downloader.py --max-images=2500 --class-names "Apple,Orange,Banana,Strawberry,Grape,Pear,Pineapple,Watermelon" --data=data/fruit
```

파일 구조는 다음과 같다.

```bash
data/fruit
    train/
        xxx.jpg
        ...
    validation/
        xxx.jpg
        ...
    test/
        xxx.jpg
        ...
    class-descriptions-boxable.csv   # 601개 전체 클래스 이름
    sub-train-annotation-bbox.csv       # 사용할 train 서브데이터 레이블링
    sub-validation-annotation-bbox.csv  # 사용할 validation 서브데이터 레이블링
    sub-test-annotation-bbox.csv        # 사용할 test 서브데이터 레이블링
    
    train-annotations-bbox.csv          # 전체 데이터. 사용 안함
    validation-annotations-bbox.csv     # 전체 데이터. 사용 안함
    test-annotations-bbox.csv           # 전체 데이터. 사용 안함

```


## 학습 실행

```bash
$ cd /jetson-inference/python/training/detection/ssd/
$ python train_ssd.py --data=data/fruit --model-dir=models/fruit --batch-size=4 --epochs=2
```

epoch 당 7~8분 소요됨.

모델 저장 위치는 

```bash
/jetson-inference/python/training/detection/ssd/
	models/fruit/
		labels.txt
		mb1-ssd-Epoch-xx-Loss-x.xxx.pth
```

<br>

## ONNX 포멧으로 converting

```bash
$ python onnx_export.py --model-dir=models/fruit
```

/jetson-inference/python/training/detection/models/fruit/ 아래에 
ssd-mobilenet.onnx 파일이 생성된다.

container 밖에서는 ~/jetson-inference/python/training/detection/models/fruit/ 아래에 있다.

<br>

## 탐지 실행

```bash
$ mkdir -p data/fruit/result
$ detectnet.py --model=models/fruit/ssd-mobilenet.onnx --labels=models/fruit/labels.txt --input-blob=input_0 --output-cvg=scores --output-bbox=boxes data/fruit/test/ee8*.jpg data/fruit/result/result_%i.jpg
```

data/fruit/result 밑에 탐지 결과 파일들이 생성된다.

<br>

## 전체 폴더 구조

```bash
# docker 안에서
/usr/local/bin/
	imagenet.py
	detectnet.py
	posenet.py

/jetson-inference/python/training/detection/
	ssd/
		data/
			fruit/
				train/
					xxx.jpg
					...
				validation/
					xxx.jpg
					...
				test/
					xxx.jpg
					...
				class-descriptions-boxable.csv   # 601개 전체 클래스 이름
				sub-train-annotation-bbox.csv       # 사용할 train 서브데이터 레이블링
				sub-validation-annotation-bbox.csv  # 사용할 validation 서브데이터 레이블링
				sub-test-annotation-bbox.csv        # 사용할 test 서브데이터 레이블링
    
		models/
			fruit/
				labels.txt
				mb1-ssd-Epoch-xx-Loss-x.xxx.pth			
				ssd-mobilenet.onnx
```

<br>

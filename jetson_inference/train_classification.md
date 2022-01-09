

-----
# 분류 학습
<br><br>


# cat_dog 분류 학습

https://github.com/dusty-nv/jetson-inference/blob/master/docs/pytorch-cat-dog.md 를 기반으로 함.

[docker](setup_by_docker.md)환경을 사용함.

<br>

## 데이터 다운로드

```bash
$ cd /jetson-inference/python/training/classification/data
$ wget https://nvidia.box.com/shared/static/o577zd8yp3lmxf5zhm38svrbrv45am3y.gz -O cat_dog.tar.gz
$ tar xvzf cat_dog.tar.gz
```

파일 구조는 다음과 같다.

```bash
cat_dog/
	labels.txt
	train/
		cat/
		dog/
	val/
		cat/
		dog/
	test/
		cat/
		dog/
```

labels.txt의 내용은 다음과 같다.
```
cat
dog
```

## 학습 실행

```bash
$ cd /jetson-inference/python/training/classification
$ python train.py --model-dir=models/cat_dog data/cat_dog --epochs=2
```

epoch 당 7~8분 소요됨.

모델 저장 위치는 

```bash
# docker 밖에서 
~/jetson-inference/python/training/classification/
	models/cat_dog/
		checkpoint.pth.tar
		model_best.pth.tar
```

<br>

## ONNX 포멧으로 converting

```bash
$ python onnx_export.py --model-dir=models/cat_dog
```

/jetson-inference/python/training/classification/models/cat_dog/ 아래에 

resnet18.onnx 파일이 생성된다.

<br>

## 분류 실행

```bash
$ MODEL=models/cat_dog
$ DATASET=data/cat_dog

$ imagenet.py --model=$MODEL/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/cat/01.jpg data/cat.jpg
```

여기서 실행되는 imagenet.py는 /usr/local/bin/imagenet.py이다.

<br>

## 전체 폴더 구조

```bash
# docker 안에서
/usr/local/bin/
	imagenet.py
	detectnet.py
	posenet.py

/jetson-inference/python/training/classification/
	train.py
	onnx_export.py
	data/
		cat_dog.tar.gz
		cat_dog/
			labels.txt
			train/
				cat/
				dog/
			val/
				cat/
				dog/
			test/
				cat/
				dog/
	models/
		cat_dog/
			checkpoint.pth.tar
			model_best.pth.tar
			resnet18.onnx
```

data/cat_dog/labels.txt 내용

```bash
cat
dog
```

<br>

# Plant 분류 학습

[https://github.com/dusty-nv/jetson-inference/blob/master/docs/pytorch-plants.md](https://github.com/dusty-nv/jetson-inference/blob/master/docs/pytorch-plants.md)

<br>

## 데이터 다운로드

```bash
$ cd /jetson-inference/python/training/classification/data
$ wget https://nvidia.box.com/shared/static/vbsywpw5iqy7r38j78xs0ctalg7jrg79.gz -O PlantCLEF_Subset.tar.gz
$ tar xvzf PlantCLEF_Subset.tar.gz
```

데이터 폴더 구조

```bash
PlantCLEF_Subset/
	labels.txt
	train/
		ash/
		beech/
		...
	val/
		ash/
		beech/
		...
	test/
		...
```

<br>

## 학습 실행

```bash
$ cd /jetson-inference/python/training/classification
$ python3 train.py --model-dir=models/plants data/PlantCLEF_Subset
```

생성된 모델 파일

```bash
/jetson-inference/python/training/classification/
	models/plants/
		checkpoint.pth.tar
		model_best.pth.tar
```

<br>

## ONNX 포멧으로 converting

```bash
$ python3 onnx_export.py --model-dir=models/plants
```

/jetson-inference/python/training/classification/models/plants/ 아래에 

resnet18.onnx 파일이 생성된다.

<br>

## 분류 실행

```bash
$ imagenet.py --model=models/plants/resnet18.onnx --labels=data/PlantCLEF_Subset/labels.txt --input_blob=input_0 --output_blob=output_0 data/PlantCLEF_Subset/test/cattail.jpg data/cattail.jpg
```

<br>

## 전체 폴더 구조

```bash
/usr/local/bin/
	imagenet.py
	detectnet.py
	posenet.py

/jetson-inference/python/training/classification/
	train.py
	onnx_export.py
	data/
		PlantCLEF_Subset.tar.gz
		PlantCLEF_Subset/
			labels.txt
			train/
				ash/
				beech/
				...
			val/
				ash/
				beech/
				...
			test/
	models/
		plants/
			checkpoint.pth.tar
			model_best.pth.tar
			resnet18.onnx
```

data/PlantCLEF_Subset/labels.txt 내용

```bash
ash
beech
...
```

<br>


-----
# 영상 분류 실습
<br><br><br><br>


# 분류(classification)
https://github.com/dusty-nv/jetson-inference/blob/master/docs/imagenet-console-2.md 를 기반으로 함.

[docker](setup_by_docker.md)환경을 사용함.

## 실행 위치로 이동
```bash
$ cd /jetson-inference/build/aarch64/bin
```

<br>

## default 모델로 실행

default model로 실행

```bash
$ ./imagenet.py images/orange_0.jpg images/test/output_0.jpg
```

<br>

## 특정 모델로 실행

```bash
$ ./imagenet.py --network=resnet-18 images/jellyfish.jpg images/test/output_jellyfish.jpg
```

모델 다운로드는 tools/download-models.sh로 할 수 있다.

network CLI 값
| Network | CLI argument |
|---------|--------------|
| AlexNet | alexnet |
| GoogleNet | googlenet |
| GoogleNet-12 | googlenet-12 |
| ResNet-18 | resnet-18 |
| ResNet-50 | resnet-50 |
| ResNet-101 | resnet-101 |
| ResNet-152 | resnet-152 |
| VGG-16 | vgg-16 |
| VGG-19 | vgg-19 |
| Inception-v4 | inception-v4 |

<br>

## 카메라에 대하여 실행

https://github.com/dusty-nv/jetson-inference/blob/master/docs/imagenet-camera-2.md 를 기반으로 함.

```bash
$ ./imagenet.py csi://0
```

장치를 검색하면 /dev/video0으로 잡히지만 다음으로 실행하면 안되고 'csi://0'으로 해야 한다.

```bash
$ ./imagenet.py /dev/video0
```

<br>

## 동영상 파일에 대하여 실행

```bash
$ wget https://nvidia.box.com/shared/static/tlswont1jnyu3ix2tbf7utaekpzcx4rc.mkv -O jellyfish.mkv

$ ./imagenet.py --network=resnet-18 jellyfish.mkv images/test/jellyfish_resnet18.mkv
```

<br>

## 코드 파악

[imagenet.py](execute_code/imagenet.py) 에서 발췌

```python

# 모델 로딩. default는 'googlenet'
net = jetson.inference.imageNet(opt.network, sys.argv)

# 파일이든 동영상이든 카메라든 관계 없다.
input = jetson.utils.videoSource(opt.input_URI, argv=sys.argv)
output = jetson.utils.videoOutput(opt.output_URI, argv=sys.argv+is_headless)

# process frames until the user exits
while True:
	# capture the next image
	img = input.Capture()

	# 탐지 결과는 class id와 신뢰도(confidence)이다.
	# classify the image
	class_id, confidence = net.Classify(img)

	# class id에 대한 class 이름
	# find the object description
	class_desc = net.GetClassDesc(class_id)

	# overlay the result on the image	
	font.OverlayText(img, img.width, img.height, "{:05.2f}% {:s}".format(confidence * 100, class_desc), 5, 5, font.White, font.Gray40)
	
	# render the image
	output.Render(img)

	# update the title bar
	output.SetStatus("{:s} | Network {:.0f} FPS".format(net.GetNetworkName(), net.GetNetworkFPS()))

	# 입력이나 출력이 streaming이 아니면 종료한다.
	# exit on input/output EOS
	if not input.IsStreaming() or not output.IsStreaming():
		break
```

<br>

## 핵심 코드

```bash
#!/usr/bin/python3

import jetson.inference
import jetson.utils

img = jetson.utils.loadImage('bear.jpg')
net = jetson.inference.imageNet('googlenet')

# 탐지 결과는 class id와 신뢰도(confidence)이다.
class_idx, confidence = net.Classify(img)
class_desc = net.GetClassDesc(class_idx)

print("image is recognized as '{:s}' (class #{:d}) with {:f}% confidence".format(class_desc, class_idx, confidence * 100))

```

<br>
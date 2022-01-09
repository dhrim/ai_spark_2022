
-----
# 물체 탐지 실습
<br><br><br><br>


# 물체탐지(object detection)

https://github.com/dusty-nv/jetson-inference/blob/master/docs/detectnet-console-2.md 를 기반으로 함.

[docker](setup_by_docker.md)환경을 사용함.

## 실행 위치로 이동
```bash
$ cd /jetson-inference/build/aarch64/bin
```

<br>

## default 모델 사용

```bash
$ ./detectnet.py images/peds_0.jpg images/test/output.jpg
```

<br>

## 특정 모델 사용

```bash
$ ./detectnet.py --network=ssd-mobilenet-v2 images/peds_0.jpg images/test/output.jpg
```

network CLI 값
| model | CLI argument | dataset |
|-------|--------------|---------|
| SSD-Mobilenet-v1 | ssd-mobilenet-v1 | 91 (COCO classes) |
| SSD-Mobilenet-v2 | ssd-mobilenet-v2 | 91 (COCO classes) |
| SSD-Inception-v2 | ssd-inception-v2 | 91 (COCO classes) |
| DetectNet-COCO-Dog | coco-dog | dogs |
| DetectNet-COCO-Bottle | coco-bottle | bottles |
| DetectNet-COCO-Chair | coco-chair | chairs |
| DetectNet-COCO-Airplane | coco-airplane | airplanes |
| ped-100 | pednet | pedestrians |
| multiped-500 | multiped | pedestrians, luggage |
| facenet-120 | facenet | faces |

<br>

## 디렉토리 내 여러개 파일

```bash
$ ./detectnet.py "images/peds_*.jpg" images/test/peds_output_%i.jpg
```

<br>

## 동영상 파일

도커 밖에서 실행
```bash
$ cd ~/jetson-inference/data
$ cp /usr/share/visionworks/sources/data/pedestrians.mp4 images/
$ cp /usr/share/visionworks/sources/data/parking.avi images/
```

도커 안에서 실행
```bash
$ ./detectnet.py images/pedestrians.mp4 images/test/pedestrians_ssd.mp4

$ ./detectnet.py images/parking.avi images/test/parking_ssd.avi
```

<br>

## 카메라 사용

```bash
$ ./detectnet.py csi://0
```

<br>

## 코드 파악

[detectnet.py](execute_code/detectnet.py) 에서 발췌

```bash

...

net = jetson.inference.detectNet(opt.network, sys.argv, opt.threshold)

input = jetson.utils.videoSource(opt.input_URI, argv=sys.argv)
output = jetson.utils.videoOutput(opt.output_URI, argv=sys.argv)

# process frames until the user exits
while True:
	# capture the next image
	img = input.Capture()

	# 탐지 결과는 img에 그려져 있고, 개별 결과는 detections에 담겨 있다.
	detections = net.Detect(img, overlay=opt.overlay)

	# 콘솔에 출력하고
	print("detected {:d} objects in image".format(len(detections))
	for detection in detections:
		print(detection)

	# 결과를 출력하고
	output.Render(img)

	# update the title bar
	output.SetStatus("{:s} | Network {:.0f} FPS".format(opt.network, net.GetNetworkFPS()))

	# print out performance info
	net.PrintProfilerTimes()

	# 입력이나 출력이 streaming이 아니면 종료한다.
	# exit on input/output EOS
	if not input.IsStreaming() or not output.IsStreaming():
		break
```

<br>

## 핵심 코드

```bash
import jetson.inference
import jetson.utils

net = jetson.inference.detectNet("ssd-mobilenet-v2", threshold=0.5)
camera = jetson.utils.videoSource("csi://0")      # '/dev/video0' for V4L2
display = jetson.utils.videoOutput("display://0") # 'my_video.mp4' for file

while display.IsStreaming():
	img = camera.Capture()
	detections = net.Detect(img)
	display.Render(img)
	display.SetStatus("Object Detection | Network {:.0f} FPS".format(net.GetNetworkFPS()))
```

<br>

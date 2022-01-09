
-----
# 영역 분할 실습
<br><br><br><br>


# 영역분할(segmentation)

https://github.com/dusty-nv/jetson-inference/blob/master/docs/segnet-console-2.md 를 기반으로 함.

[docker](setup_by_docker.md)환경을 사용함.

## 실행 위치로 이동
```bash
$ cd /jetson-inference/build/aarch64/bin
```

<br>

## default 모델로

```bash
$ ./segnet.py images/city_0.jpg images/test/output.jpg
```

<br>

## 특정 모델 사용

```bash
$ ./segnet.py --network=fcn-resnet18-cityscapes images/city_0.jpg images/test/output.jpg
```

network CLI 값
| Dataset	| Resolution	| CLI Argument	| Accuracy	| FPS	|
|---------|-------------|---------------|-----------|-----|
| Cityscapes	| 512x256	| fcn-resnet18-cityscapes-512x256	| 83.3%	| 48 FPS	|
| Cityscapes	| 1024x512	| fcn-resnet18-cityscapes-1024x512	| 87.3%	| 12 FPS	|
| Cityscapes	| 2048x1024	| fcn-resnet18-cityscapes-2048x1024	| 89.6%	| 3 FPS	|
| DeepScene	| 576x320	| fcn-resnet18-deepscene-576x320	| 96.4%	| 26 FPS	|
| DeepScene	| 864x480	| fcn-resnet18-deepscene-864x480	| 96.9%	| 14 FPS	|
| Multi-Human	| 512x320	| fcn-resnet18-mhp-512x320	| 86.5%	| 34 FPS	|
| Multi-Human	| 640x360	| fcn-resnet18-mhp-640x360	| 87.1%	| 23 FPS	|
| Pascal VOC	| 320x320	| fcn-resnet18-voc-320x320	| 85.9%	| 45 FPS	|
| Pascal VOC	| 512x320	| fcn-resnet18-voc-512x320	| 88.5%	| 34 FPS	|
| SUN RGB-D	| 512x400	| fcn-resnet18-sun-512x400	| 64.3%	| 28 FPS	|
| SUN RGB-D	| 640x512	| fcn-resnet18-sun-640x512	| 65.1%	| 17 FPS	|

<br>

### 폴더 파일 전체에 대해

```bash
$ ./segnet.py --network=fcn-resnet18-sun "images/room_*.jpg" images/test/room_output_%i.jpg
```

<br>

## 카메라 동영상에 대해

```bash
$ ./senet.py csi://0
```

<br>

## 코드 파악

[segnet.py](execute_code/segnet.py) 에서 발췌

```python
#!/usr/bin/python3

...


# load the segmentation network
net = jetson.inference.segNet(opt.network, sys.argv)

input = jetson.utils.videoSource(opt.input_URI, argv=sys.argv)
output = jetson.utils.videoOutput(opt.output_URI, argv=sys.argv+is_headless)

buffers = segmentationBuffers(net, opt)


# process frames until user exits
while True:
	# capture the next image
	img_input = input.Capture()

	# allocate buffers for this size image
	buffers.Alloc(img_input.shape, img_input.format)

	# process the segmentation network
	net.Process(img_input, ignore_class=opt.ignore_class)

	# generate the overlay
	if buffers.overlay:
		net.Overlay(buffers.overlay, filter_mode=opt.filter_mode)

	# generate the mask
	if buffers.mask:
		net.Mask(buffers.mask, filter_mode=opt.filter_mode)

	# composite the images
	if buffers.composite:
		jetson.utils.cudaOverlay(buffers.overlay, buffers.composite, 0, 0)
		jetson.utils.cudaOverlay(buffers.mask, buffers.composite, buffers.overlay.width, 0)

	# render the output image
	output.Render(buffers.output)

	# update the title bar
	output.SetStatus("{:s} | Network {:.0f} FPS".format(opt.network, net.GetNetworkFPS()))

	# print out performance info
	jetson.utils.cudaDeviceSynchronize()
	net.PrintProfilerTimes()

    # compute segmentation class stats
	if opt.stats:
		buffers.ComputeStats()
    
	# exit on input/output EOS
	if not input.IsStreaming() or not output.IsStreaming():
		break


```


<br>

## 핵심 코드

```bash
import jetson.inference
import jetson.utils

net = jetson.inference.segNet('fcn-resnet18-voc')

input = jetson.utils.videoSource("csi://0")      # '/dev/video0' for V4L2
output = jetson.utils.videoOutput("display://0") # 'my_video.mp4' for file

while display.IsStreaming():

	img_input = input.Capture()

	net.Process(img_input, ignore_class=opt.ignore_class)

	# generate the overlay
	net.Overlay(buffers.overlay, filter_mode=opt.filter_mode)
	# generate the mask
	net.Mask(buffers.mask, filter_mode=opt.filter_mode)

	# composite the images
	jetson.utils.cudaOverlay(buffers.overlay, buffers.composite, 0, 0)
	jetson.utils.cudaOverlay(buffers.mask, buffers.composite, buffers.overlay.width, 0)

	output.Render(buffers.output)
```

<br>

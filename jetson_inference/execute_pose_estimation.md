
-----
# 포즈 추출 실습
<br><br><br><br>


# 포즈추출(pose estimation)

https://github.com/dusty-nv/jetson-inference/blob/master/docs/posenet.md 를 기반으로 함.

[docker](setup_by_docker.md)환경을 사용함.

## 실행 위치로 이동
```bash
$ cd /jetson-inference/build/aarch64/bin
```

<br>

## default 모델로

```bash
$ ./posenet.py images/humans_1.jpg images/test/human_1_pose.jpg
```

<br>

## 특정 모델로

```bash
$ ./posenet.py --network=resnet18-body images/humans_1.jpg images/test/human_1_pose.jpg
```

network CLI 값
| Model	| CLI argument	| NetworkType enum	| Keypoints |
|-------|---------------|-------------------|-----------|
| Pose-ResNet18-Body	| resnet18-body	| RESNET18_BODY	| 18 | 
| Pose-ResNet18-Hand	| resnet18-hand	| RESNET18_HAND	| 21 | 
| Pose-DenseNet121-Body	| densenet121-body	| DENSENET121_BODY	| 18 |

<br>

## 폴더 파일 전체에 대해

```bash
$ ./posenet.py "images/humans_*.jpg" images/test/pose_humans_%i.jpg
```

<br>

## 카메라 동영상에 대해

```bash
./posenet.py csi://0
```

<br>


## 코드 파악

[posenet.py](execute_code/posenet.py) 에서 발췌

```python

...

net = jetson.inference.poseNet(opt.network, sys.argv, opt.threshold)

input = jetson.utils.videoSource(opt.input_URI, argv=sys.argv)
output = jetson.utils.videoOutput(opt.output_URI, argv=sys.argv)

# process frames until the user exits
while True:
    # capture the next image
    img = input.Capture()

    # perform pose estimation (with overlay)
    poses = net.Process(img, overlay=opt.overlay)

    # print the pose results
    print("detected {:d} objects in image".format(len(poses)))

    for pose in poses:
        print(pose)
        print(pose.Keypoints)
        print('Links', pose.Links)

    # render the image
    output.Render(img)

    # update the title bar
    output.SetStatus("{:s} | Network {:.0f} FPS".format(opt.network, net.GetNetworkFPS()))

    # print out performance info
    net.PrintProfilerTimes()

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

net = jetson.inference.poseNet("resnet18-body", 0.15)

input = jetson.utils.videoSource("csi://0")      # '/dev/video0' for V4L2
display = jetson.utils.videoOutput("display://0") # 'my_video.mp4' for file

img = input.Capture()

# perform pose estimation (with overlay)
poses = net.Process(img, overlay=opt.overlay)

# print the pose results
print("detected {:d} objects in image".format(len(poses)))

for pose in poses:
    print(pose)
    print(pose.Keypoints)
    print('Links', pose.Links)

```

<br>



## 코드로 값 구하기

```bash
poses = net.Process(img)

for pose in poses:
    left_wrist_idx = pose.FindKeypoint('left_wrist')
    left_shoulder_idx = pose.FindKeypoint('left_shoulder')

    if left_wrist_idx < 0 or left_shoulder_idx < 0:
        continue
	
    left_wrist = pose.Keypoints[left_wrist_idx]
    left_shoulder = pose.Keypoints[left_shoulder_idx]

    point_x = left_shoulder.x - left_wrist.x
    point_y = left_shoulder.y - left_wrist.y

    print(f"person {pose.ID} is pointing towards ({point_x}, {point_y})")
```

<br>

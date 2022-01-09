# jetson-inference 프로젝트

- https://github.com/dusty-nv/jetson-inference 을 기반으로 함.
- NVIDIA의 공식 프로젝트. 
- 소스에서 빌드할 수도 있고, 이미 빌드된 docker를 사용할 수도 있다.
- 특정 모델을 선택하여 다운로드하여 실행함.
- 생성된 python 실행파일로 물체 분류(classification), 물체 탐지(object detection), 영역 분할(segmentation), 깊이 탐지(depth detection) 실행

<br>

# 프로젝트 전체 내용

- jetson 셋업. 소스 빌드하거나 docker를 사용
- 분류, 탐지, 분할, 포즈추출 할 수 있고
- 데이터 가져와서 분류 학습할 수 있고
- 혹은 분류, 탐지(SSD) 데이터 새로 만들어 학습

<br>

# 세부 항목

- Jetson 셋업
    - [Jetson 셋업](../jetson_setup.pdf)의 방법으로 Jetson을 부팅 상태로 한다.
    - [소스에서 빌드](setup_from_source.md)
    - [docker 사용](setup_by_docker.md)

- 기존 모델 실행
    - [분류(classificaiton)](execute_classification.md)
    - [물체 탐지(object detection)](execute_object_detection.md)
    - [영역 분할(segmentation)](execute_segmentation.md)
    - [포즈 추출(pose estimation)](execute_pose_estimation.md)

- 준비된 데이터로 학습
    - [학습 환경 준비](prepare_training.md)
    - [분류(classificaiton)](train_classification.md)
    - [물체 탐지(object detection)](train_object_detection.md)

- 새로운 데이터로 학습
    - [분류(classificaiton)](train_classification_with_custom_data.md)
    - [물체 탐지(object detection)](train_object_detection_with_custom_data.md)

# 스마트홈 IoT 기기 제어를 위한 객체 인식 모델 구현

Jetson Nano에서 딥러닝을 사용한 이미지 처리 교육

<br>

# 프로그램

## 1일차

- [Jetson 셋업](jetson_setup.pdf)
- [Linux 기본 명령어 실습](linux_commands.md)
- Jetson-Inference 프로젝트 설치
    - [소스에서 빌드](jetson_inference/setup_from_source.md)
    - [docker 사용](jetson_inference/setup_by_docker.md)

<br>

## 2일차
- 영상 분류 실습
    - [분류(classificaiton)](jetson_inference/execute_classification.md)

- 물체 탐지 실습
    - [물체 탐지(object detection)](jetson_inference/execute_object_detection.md)

- 영역 분할 실습
    - [영역 분할(segmentation)](jetson_inference/execute_segmentation.md)

- 포즈 추출 실습
    - [포즈 추출(pose estimation)](jetson_inference/execute_pose_estimation.md)

- 학습 실습
    - [학습 환경 준비](jetson_inference/prepare_training.md)
    - [분류(classification) 학습](jetson_inference/train_classification.md)
    - [물체 탐지(object detection) 학습](jetson_inference/train_object_detection.md)

<br>

## 3일차
- [딥러닝의 이해](deep_learning_intro.pptx)
- 영상 데이터 분류 실습
    - 속성 데이터 IRIS 분류 : [dnn_iris_classification.ipynb](./deep_learning/dnn_iris_classification.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dhrim/jetson_image/blob/master/deep_learning/dnn_iris_classification.ipynb)
    - 영상 데이터 MNIST 분류 : [dnn_mnist.ipynb](./deep_learning/dnn_mnist.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dhrim/jetson_image/blob/master/deep_learning/dnn_mnist.ipynb)
    - 영상 데이터 MNIST 분류 - CNN : [cnn_mnist.ipynb](./deep_learning/cnn_mnist.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dhrim/jetson_image/blob/master/deep_learning/cnn_mnist.ipynb)
    - 컬러 영상 데이터 CIFAR10 분류 - CNN : [cnn_cifar10.ipynb](./deep_learning/cnn_cifar10.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dhrim/jetson_image/blob/master/deep_learning/cnn_cifar10.ipynb)
    - 전이학습 영상 분류 Template : [template_image_data_transfer_learning_classification.ipynb](./deep_learning/template_image_data_transfer_learning_classification.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dhrim/jetson_image/blob/master/deep_learning/template_image_data_transfer_learning_classification.ipynb)

<br>

## 4일차

- 카메라 캡쳐한 데이터 학습
    - [영상 분류](jetson_inference/train_classification_thumb_up_down.md)
    - [물체 탐지](jetson_inference/train_object_detection_with_custom_data.md)    
- AWS 서버
    - 학습 환경 소개, Jupyter 소개
    - [AWS 서버에서 분류 학습](jetson_inference/train_classification_on_server.md)
    - [AWS 서버에서 물체 탐지 학습](jetson_inference/train_object_detection_on_server.md)

<br>

## 5일차
- 커스텀 데이터 AWS 서버에서 분류 학습 : [데이터](data/flowers.zip)
- 커스텀 데이터 AWS 서버에서 Keras 코드로 분류 학습
    - [데이터](data/flowers_prepared.zip)
    - [학습 실행 노트북](jetson_inference/train_classification_on_server_on_keras.ipynb)
    - [Jetson에 업로드와 분류 실행](jetson_inference/execute_classification_by_uploaded_model.md)

<br>

# 기타

- 흥미로운 딥러닝 결과들 [some_interesting_deep_learning.pptx](some_interesting_deep_learning.pptx)
- 영문 영화 평가 데이터 IMDB 분류 : [rnn_text_classification.ipynb](./deep_learning/rnn_text_classification.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dhrim/jetson_image/blob/master/data/rnn_text_classification.ipynb)
- 한글 영화 평가 데이터 분류 : [korean_word_sequence_classification.ipynb](./deep_learning/korean_word_sequence_classification.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dhrim/jetson_image/blob/master/data/korean_word_sequence_classification.ipynb)


<br>

# 교육에 필요한 장비
[requirements.md](requirements.md)
# NVIDIA Jetson을 사용한 물체 인식 예제

제2회 연구개발특구 인공지능 경진대회 AI SPARK 챌린지
http://aifactory.space/competition/data/1946

<br>

# 내용
- 1부. 셋업
    - [Jetson 셋업](jetson_setup.pdf)
    - [Jetson-Inference 프로젝트 설치](jetson_inference/setup_by_docker.md)
    - 영상 분류와 영상 탐지 동작 확인
        - [분류(classificaiton)](jetson_inference/execute_classification.md)
        - [물체 탐지(object detection)](jetson_inference/execute_object_detection.md)
- 2부. 영상 분류 학습
    - [영상분류 데이터 캡쳐와 학습](jetson_inference/train_classification_thumb_up_down.md)
- 3부. 물체 탐지 학습
    - [물체탐지 데이터 캡쳐와 학습](jetson_inference/train_object_detection_with_custom_data.md)    


<br>

# 셋업을 위한 필요 사항

- Jetson Nano (https://www.devicemart.co.kr/goods/view?no=12513656)
- Power Adapter. (https://www.devicemart.co.kr/goods/view?no=12240663) Jetson Nano에 비포함. 별도 구매 필요
- USB keyboard
- USB mouse
- Monitor
    - Monitor power 케이블
    - Monitor HDMI 케이블
- 인터넷에 연결된 RJ45 LAN 케이블. Jetson을 인터넷에 연결하기 위한.
- 카메라 모듈 (https://www.devicemart.co.kr/goods/view?no=13563285)
- SD 카드. 64G
- SD 카드 USB 리더
- 별도의 윈도우 컴퓨터. SD 카드 만들때 사용.
- 원할한 네트웤. 6G 파일 다운 받는데 수분 정도 소요되는.

<br>





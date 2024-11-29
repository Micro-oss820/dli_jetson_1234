# JETSON-NANO

1. SD Card Formatter
   
2. jetpack download
   
3. balena Etcher download

![jetson-nano-dev-kit-top-r6-HR-B01](https://github.com/user-attachments/assets/4ddf52bb-1ea8-4050-a5d2-1efd54d605ec)

4. sd b

## 2024-11-21

1. 메모리 부족하여서 영상이 사진처럼 나온다면 swap 18GB 설치하기
- 코드

sudo systemctl disable nvzramconfig

sudo systemctl set-default multi-user.target

sudo fallocate -l 18G /mnt/18GB.swap
sudo chmod 600 /mnt/18GB.swap
sudo mkswap /mnt/18GB.swap

sudo su
echo "/mnt/18GB.swap swap swap defaults 0 0" >> /etc/fstab
exit

sudo reboot

2. reboot 한 후, MICRO 5 PIN 연결 -> L4T 연결 된 표시가 오른쪽 하단에 나온다.

3. reboot 한 후 시스템을 GUI 모드로 설정한다.
- 코드

sudo systemctl set-default graphical.target

reboot

4. 해당 코드를 실행하여 컴퓨터 웹브라우저를 열고 주소창에 192.168.55.1:8888 를 실행한다. 비밀번호 : dlinano
- 코드
dli@dli-desktop:~$#!/bin/bash

sudo docker run --runtime nvidia -it --rm --network host \
    --memory=500M --memory-swap=4G \
    --volume ~/nvdli-data:/nvdli-nano/data \
    --volume /tmp/argus_socket:/tmp/argus_socket \
    --device /dev/video0 \
    nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1kr

5. 실행된 주피터에서 classification을 들어가서 코드를 실행한다.
6. thumbs_up / thumbs_down 별로 30개 정도의 데이터를 만들고 epochs 10개 정도로 학습한다. 그리고 정확성을 확인한다.
7. classification 실행한 모습
![KakaoTalk_20241121_205731559](https://github.com/user-attachments/assets/f4d4216c-53e6-442e-be15-051d61653f63)




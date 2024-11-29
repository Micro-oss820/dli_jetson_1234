# 주제 : JETSON-NANO

## JETSON-NANO 수업 전 준비

### 1. SD Card Formatter download
   - 링크 : https://sd-memory-card-formatter.en.softonic.com/download
   
### 2. jetpack download
   - 링크 : https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write

### 3. balena Etcher download
   - 링크 : https://www.balena.io/etcher/

### 4. NVIDIA / Github 계정 생성하기

## (2024-11-07) JETSON-NANO 환경 구축
   
### 참고 : JETSON-NANO 구조
![jetson-nano-dev-kit-top-r6-HR-B01](https://github.com/user-attachments/assets/4ddf52bb-1ea8-4050-a5d2-1efd54d605ec)

### 1. balena Etcher를 이용해서 GUI 이미지 굽기

### 2. 구운 이미지를 SD카드에 넣어 JETSON-NANO에 넣기

### 3. JETSON-NANO를 연결하여 우분투 설치하기
   - 참고 링크 : https://driz2le.tistory.com/253

3-1. ![image](https://github.com/user-attachments/assets/d2afb184-d3a9-4840-81b5-05f14e701634) Terminal에서 $ sudo apt-get update 입력
![image](https://github.com/user-attachments/assets/5c56489d-ade7-4758-b4a5-9b2a51193584)

3-2. $ sudo apt-get install fcitx-hangul 입력
![image](https://github.com/user-attachments/assets/f4224c6f-0020-49ba-aa2b-cd0e6683083a)

3-3. ![image](https://github.com/user-attachments/assets/ca6a581c-c263-4300-a0ac-024579674349) 설정에서 ![image](https://github.com/user-attachments/assets/041f44a7-9aee-41aa-a657-abfb3d5e7d39) Language Support 들어가기

![image](https://github.com/user-attachments/assets/76a17778-8cae-4bf0-b756-0b3b86b3d011)

3-4. 하단 Keyboard input method system 설정을 fcitx로 바꾼다.

![image](https://github.com/user-attachments/assets/26098391-4d62-4915-a387-22a7cc8d29f0)

3-5. 재부팅하기

3-6. 하단 + 누르고 hangul 찾기
![image](https://github.com/user-attachments/assets/dc448354-8149-4340-8731-7ff4a7c857a8)
![image](https://github.com/user-attachments/assets/b2885ce6-9819-4a99-83a6-6ccdc05a12f4)

3-7. 공백칸 누르고 '한/영'키 입력
![image](https://github.com/user-attachments/assets/7d2c22bf-4789-4031-9012-1d3b1bc2bcae)


## (2024-11-14) JETSON-NANO 카메라 연결 및 헤드리스 모드

### 1. 헤드리스 모드

1-1. 교육과정에 필요한 dir 추가하기
   - 코드

~$ mkdir -p ~/nvdli-data
~$ ls

1-2. Terminal 창에 입력
   - 코드
sudo docker run --runtime nvidia -it --rm --network host
--memory=500M --memory-swap=4G
--volume ~/nvdli-data:/nvdli-nano/data
--volume /tmp/argus_socket:/tmp/argus_socket
--device /dev/video0
nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1kr
![image](https://github.com/user-attachments/assets/d4f23277-45dd-4c17-9376-5a5eb782ec69)

1-3. allow 10 sec for JupyterLab to start @ http://192.168.176.16:8888 (password dlinano) JupterLab logging location: /var/log/jupyter.log (inside the container) root@ai-desktop:/nvdli-nano# 기억해두기
![image](https://github.com/user-attachments/assets/ebb6c6d0-870e-4b12-9ad4-4e352101d22b)

1-4. Terminal에 아래 내용 입력하기
  
   - 코드

sudo systemctl disable nvzramconfig

sudo systemctl set-default multi-user.target sudo fallocate -l 18G /mnt/18GB.swap

sudo chmod 600 /mnt/18GB.swap sudo mkswap /mnt/18GB.swap

sudo su echo "/mnt/18GB.swap swap swap defaults 0 0" >> /etc/fstab
exit

sudo reboot

1-5. 입력하고 리부트 후 아이디 비밀번호 입력한 뒤 시스템 GUI모드로 설정

   - 코드

sudo systemctl set-default graphical.target

reboot 입력

### 2. 카메라 인식 확인
![image](https://github.com/user-attachments/assets/515fbefc-41fb-44b3-a1a3-55c273ff4dcb)

### 번외 : 심심풀이 인증샷
![KakaoTalk_20241130_000734931_03](https://github.com/user-attachments/assets/ba0632a8-d310-445c-b688-89cba090de7d)

## (2024-11-21) Classification 

### 1. 메모리 부족하여서 영상이 사진처럼 나온다면 swap 18GB 설치하기
  
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

### 2. reboot 한 후, MICRO 5 PIN 연결 -> L4T 연결 된 표시가 오른쪽 하단에 나온다.

### 3. reboot 한 후 시스템을 GUI 모드로 설정한다.
   
   - 코드

sudo systemctl set-default graphical.target

reboot

### 4. 해당 코드를 실행하여 컴퓨터 웹브라우저를 열고 주소창에 192.168.55.1:8888 를 실행한다. 비밀번호 : dlinano
   
   - 코드

dli@dli-desktop:~$#!/bin/bash

sudo docker run --runtime nvidia -it --rm --network host \
    --memory=500M --memory-swap=4G \
    --volume ~/nvdli-data:/nvdli-nano/data \
    --volume /tmp/argus_socket:/tmp/argus_socket \
    --device /dev/video0 \
    nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1kr

### 5. 실행된 주피터에서 classification을 들어가서 코드를 실행한다.
![image](https://github.com/user-attachments/assets/5adccb5c-fbfb-4782-a257-c8ee13d5df26)

### 6. thumbs_up / thumbs_down 별로 30개 정도의 데이터를 만들고 epochs 10개 정도로 학습한다. 그리고 정확성을 확인한다.
![image](https://github.com/user-attachments/assets/97c11664-7160-4762-b35d-bdd11a3d89dd)

### 7. classification 실행 예시
![KakaoTalk_20241121_205731559](https://github.com/user-attachments/assets/f4d4216c-53e6-442e-be15-051d61653f63)




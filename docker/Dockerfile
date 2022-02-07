FROM fnndsc/ubuntu-python3:latest
MAINTAINER wuben
# use aliyun's mirror for faster download speed
RUN sed -i 's/ports.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list
# install all neccessary packages
RUN apt-get -y update
RUN apt-get install -y --fix-missing build-essential \
    wget git  python3-pyaudio sox ffmpeg && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /root/
# install pip3
RUN wget https://bootstrap.pypa.io/get-pip.py --no-check-certificate
RUN python3 get-pip.py
# clone main repo
RUN git clone https://github.com/wuben007/wukong-robot.git
# clone .wukong repo
RUN mkdir /root/.wukong
WORKDIR /root/.wukong
RUN git clone https://github.com/wzpan/wukong-contrib.git contrib
# update swig 4
WORKDIR /root
RUN apt-get -y update
RUN apt-get install -y libpcre3 libpcre3-dev python3-all-dev libatlas-base-dev python3-dev swig
RUN apt-get -y update
# build _snowboydetect.so
WORKDIR /root
RUN wget https://github.com/Kitt-AI/snowboy/archive/refs/tags/v1.3.0.tar.gz -O snowboy.tar.gz
RUN tar -zxf snowboy.tar.gz &&cp -R snowboy-1.3.0 snowboy && rm -rf snowboy-1.3.0
WORKDIR snowboy/swig/Python3
#RUN cp -R lib/aarch64-ubuntu1604/libsnowboy-detect.a  lib/ubuntu64/libsnowboy-detect.a
RUN  make
RUN cp _snowboydetect.so /root/wukong-robot/snowboy/
WORKDIR /root/wukong-robot
RUN git pull
RUN pip3 install -r requirements.txt
WORKDIR /root/.wukong/contrib
RUN git pull
RUN pip3 install -r requirements.txt
RUN apt-get install -y alsa-base pulseaudio libsox-fmt-all
WORKDIR /root/wukong-robot
EXPOSE 5000

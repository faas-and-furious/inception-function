FROM tensorflow/tensorflow:latest

RUN apt-get update -qy \
 && apt-get install -qy \
       python3-pip
RUN curl -sSL https://github.com/openfaas/faas/releases/download/0.7.0/fwatchdog > /usr/bin/fwatchdog \
    && chmod +x /usr/bin/fwatchdog

WORKDIR /root/

COPY requirements.txt ./

RUN pip3 install -r requirements.txt

ENV TF_CPP_MIN_LOG_LEVEL=3
COPY *.py ./

RUN python3 ./pre_download.py

COPY *.txt ./



ENV write_debug="true"
ENV fprocess="python3 index.py"

CMD ["fwatchdog"]

FROM openfaas/classic-watchdog:0.18.1 as watchdog

FROM tensorflow/tensorflow:1.15.0

RUN apt-get update -qy \
 && apt-get install -qy \
       python3-pip

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

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

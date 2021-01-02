---
title: Power Monitoring – NVIDIA Jetson TX2
author: hoony9x
date: 2017-07-07 19:05:54
image: assets/images/2017-07-07-power-monitoring-nvidia-jetson-tx2/graph_sample.jpeg
categories:
  - "Experience"
  - "UCI I-SURF 2017"
tags:
  - "United States"
  - "UCI"
  - "I-SURF 2017"
  - "NVIDIA Jetson TX2"
---

NVIDIA Jetson TX2 보드는 아래의 6가지 component 에 대해서 power monitoring 이 가능하다.

(출처: JETSON TX2 OEM PRODUCT | DESIGN GUIDE | 20170501)

- GPU Supply
- SoC Supply
- Wi-Fi Supply
- VDD_IN Supply
- CPU Supply
- DDR Supply

그리고 이 정보는 Linux 에서 sysfs 를 통해 확인할 수 있다.

~~아래 내용은 다음 링크를 참고하였다. (TX1에 관한 글이지만 TX2에서 확인 결과 같은 동작을 한다.)~~  
~~Power monitoring 은 다음 경로로 접근하면 된다. (2017년 8월 3일 기준 아래 경로가 존재하지 않는다.)~~

```text
/sys/devices/platform/7000c400.i2c/i2c-1/1-0042/iio_device/
/sys/devices/platform/7000c400.i2c/i2c-1/1-0043/iio_device/
```

2017년 8월 3일 기준으로는 [다음 글](https://devtalk.nvidia.com/default/topic/1000830/jetson-tx2/jetson-tx2-ina226-power-monitor-with-i2c-interface-/)을 참고하는 것이 좋으며 PMU 정보는 밑에 적어둔 경로로 접근해서 확인 가능하다.

```text
/sys/devices/3160000.i2c/i2c-0/0-0040/iio_device
/sys/devices/3160000.i2c/i2c-0/0-0041/iio_device
/sys/devices/3160000.i2c/i2c-0/0-0042/iio_device
/sys/devices/3160000.i2c/i2c-0/0-0043/iio_device
```

각 directory에는 3개의 rail (sensor와 연결된 일종의 정보 전달 경로 개념으로 이해하고 있다) 이 있다.  
어떤 sensor인지는 각 directory에서 rail_name_N 의 내용을 확인하면 된다. (여기서 N은 0, 1, 2 중 하나의 숫자)

각각의 sensor는 전압, 전류, 소비전력 정보를 제공해준다. (단위는 각각 mV, mA, mW 이다)  
이들 정보는 in_currentN_input, in_voltageN_input, in_powerN_input 를 통해 확인 가능하다. (N 은 0, 1, 2 중 하나의 숫자)

이 정보는 해당 파일을 읽는 순간의 값(Instant value) 을 표시하게 된다.

CPU의 각 코어의 On/Off 상태 및 Frequency값, GPU Frequency값 <> 소비전력 관계를 나타내는 데 유용하게 사용할 수 있을 것 같다.

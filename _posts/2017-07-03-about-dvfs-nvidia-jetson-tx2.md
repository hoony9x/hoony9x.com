---
title: Dynamic Voltage and Frequency Scaling (DVFS) 에 관하여
author: hoony9x
date: 2017-07-03 15:10:10
image: assets/images/2017-07-03-about-dvfs-nvidia-jetson-tx2/DFVS_Graph.001.jpeg
categories:
  - "Experience"
  - "UCI I-SURF 2017"
tags:
  - "United States"
  - "UCI"
  - "I-SURF 2017"
  - "NVIDIA Jetson TX2"
---

이번 글은 Dynamic Voltage and Frequency Scaling (DVFS) 에 관하여 다룬다.

(본 글은 아래의 문서를 참고하였으며 틀린 부분이 있다면 댓글 부탁드립니다.)

참고한 문서

- https://en.wikipedia.org/wiki/Dynamic_voltage_scaling
- https://en.wikipedia.org/wiki/Dynamic_frequency_scaling
- http://processors.wiki.ti.com/index.php/DVFS_User_Guide

아래 내용은 [여기](http://processors.wiki.ti.com/index.php/DVFS_User_Guide)의 내용을 대부분 그대로 가져왔다.

[여기](http://processors.wiki.ti.com/index.php/DVFS_User_Guide)에 있는 글을 읽어보면 DVFS 에 대해서 다음과 같이 적혀 있다.

> Dynamic Voltage and Frequency scaling is a framework to change the frequency and/or operating voltage of a processor(s) based on system performance requirements at the given point of time.

필요에 따라 코어의 전압과 주파수(클럭)을 조정하여 성능을 변화시킬 수 있는 방법인 듯 하다.

### Frequency Scaling

주파수(아마도 CPU clock)를 조절하여 성능의 조절을 하는 방법이다.

이 방법은 CPUFreq 라는 Linux framework 를 이용한다. 이 framework 는 processor 의 성능 요구 상황을 모니터링하고 이에 따라 frequency 를 조절하게 된다.

CPUFreq 는 다음 2가지 요소로 구성되어 있다.

1. Governor : 어떻게 조절할 지 결정(Decision)을 한다. System performance를 monitoring하면서 Frequency 조절이 필요할 경우 Policy를 통해 frequency limit 을 확인한 후 Driver 에 frequency 조절 요청을 하게 된다.
2. Driver : Governor의 결정에 따라 실제로 frequency 의 조절을 한다. 요청된 frequency 값을 OOP List 에서 확인하여 변경 가능한 값일 경우 해당 값으로 frequency 변경을 하게 된다.

각각의 CPU 코어는 특정 Frequency 에서 요구되는 Voltage 가 있다. 이 정보들이 담긴 목록을 Operating performance Point(OPP) List 라고 한다.

각각의 CPU 코어마다 선택 가능한 Frequency 목록이 있는데 이를 Frequency Table 이라고 한다. 이는 OOP List 에 기반하여 생성된다.

각각의 CPU 코어는 최저/최대 Frequency, 그리고 이 사이에서 선택 가능한 Frequency 값들이 존재한다. 이런 선택 가능한 Rule 들을 Policy 라고 하며 이는 Frequency Table 에 기반하여 생성된다.

CPUFreq framework 에는 다음과 같은 Governor 가 존재한다.

1. Performance
2. Powersave
3. Ondemand
4. Userspace
5. Conservative

### Voltage Scaling

전압을 조절한다.

Frequency 가 변화할 경우 OOP List에 기반하여 변화한 값에 맞게 Voltage도 자동으로 조절하는 듯 하다.

### Frequency 값을 직접 지정하기

각 CPU 코어마다 직접 지정이 가능한 듯 하다.

(확실하지는 않지만) Governor를 userspace로 지정하면 자신이 원하는 frequency 값을 고정할 수 있는 것 같다. (물론 가능한 frequency 값이어야 한다.)

```bash
[userspace governor 선택]
$ echo -n "userspace" > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
```

```bash
[선택 가능한 frequency 확인]
$ cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_frequencies
```

```bash
[frequency 지정]
$ echo -n "<new_frequency> > /sys/devices/system/cpu/cpu0/cpufreq/scaling_setspeed
```

NVIDIA Jetson TX2 에는 각 CPU 코어마다 frequency 미리 지정된 값으로 간편하게 바꿀 수 있도록 하는 명령어가 존재한다. 이는 다음 글에...

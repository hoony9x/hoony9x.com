---
title: NVPModel – NVIDIA Jetson TX2
author: hoony9x
date: 2017-07-03 19:48:17
image: assets/images/2017-07-03-nvpmodel-nvidia-jetson-tx2/3-1.jpeg
categories:
  - "Experience"
  - "UCI I-SURF 2017"
tags:
  - "United States"
  - "UCI"
  - "I-SURF 2017"
  - "NVIDIA Jetson TX2"
---

TX2 보드에는 CPU코어의 on/off 및 frequency 조절, GPU frequency 조절을 미리 설정된 값으로 쉽게 변경할 수 있도록 하는 명령어가 존재한다.

이 명령어는 NVPModel 라는 이름으로 존재한다.

![http://www.jetsonhacks.com/2017/03/25/nvpmodel-nvidia-jetson-tx2-development-kit/](/assets/images/2017-07-03-nvpmodel-nvidia-jetson-tx2/Screen-Shot-2018-10-26-at-2.00.11-PM.png)

기본적으로 5가지의 Preset 을 제공하며, 각 mode 마다 위와 같이 성능을 조절할 수 있다.

다음 command를 입력하여 mode를 선택 가능하다.

```bash
$ sudo nvpmodel -m [mode_number]
```

또한, 다음 command를 입력하여 현재 설정된 mode 를 확인 가능하다.

```bash
$ sudo nvpmodel -q –verbose
```

기본 설정 값 내용은 `/etc/nvpmodel.conf` 에 저장되어 있다.

이 파일의 내용은 다음과 같다.  
(2017년 8월 18일 기준 보드에 초기 세팅을 진행할 경우 아래 내용과 다른 내용이 들어가 있다.)

```txt
# FORMAT:
# < POWER_MODEL ID=id_num NAME=mode_name >
# This starts a section of POWER_MODEL configurations, followed by
# lines with sysfs nodes settings as the format below:
# sysfs_path sysfs_value
# sysfs_value is an integer, and -1 can be used as the INT_MAX.
# This file must contain at least one POWER_MODEL section.
# < PM_STATUS DEFAULT=default_mode >
# This is a mandatory section to specify one of the defined power
# model as the default.

# MAXN is the NONE power model to release all constraints
< POWER_MODEL ID=0 NAME=MAXN >
/sys/devices/system/cpu/cpu1/online 1
/sys/devices/system/cpu/cpu2/online 1
/sys/devices/system/cpu/cpu3/online 1
/sys/devices/system/cpu/cpu4/online 1
/sys/devices/system/cpu/cpu5/online 1

# cpu clock settings
# A57 cluster
/sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq 0
/sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq 2035200
# Denver cluster
/sys/devices/system/cpu/cpu1/cpufreq/scaling_min_freq 0
/sys/devices/system/cpu/cpu1/cpufreq/scaling_max_freq 2035200

# gpu clock settings
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/min_freq 0
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/max_freq 1300500000

# emc clock settings
/sys/kernel/nvpmodel_emc_cap/emc_iso_cap 0

< POWER_MODEL ID=1 NAME=MAXQ >
# cpu core settings
/sys/devices/system/cpu/cpu1/online 0
/sys/devices/system/cpu/cpu2/online 0
/sys/devices/system/cpu/cpu3/online 1
/sys/devices/system/cpu/cpu4/online 1
/sys/devices/system/cpu/cpu5/online 1

# cpu clock settings
# A57 cluster
/sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq 0
/sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq 1200000

# gpu clock settings
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/min_freq 0
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/max_freq 850000000

# emc clock settings
/sys/kernel/nvpmodel_emc_cap/emc_iso_cap 1331200000

< POWER_MODEL ID=2 NAME=MAXP_CORE_ALL >
# cpu core settings
/sys/devices/system/cpu/cpu1/online 1
/sys/devices/system/cpu/cpu2/online 1
/sys/devices/system/cpu/cpu3/online 1
/sys/devices/system/cpu/cpu4/online 1
/sys/devices/system/cpu/cpu5/online 1

# cpu clock settings
# A57 cluster
/sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq 0
/sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq 1400000
# Denver cluster
/sys/devices/system/cpu/cpu1/cpufreq/scaling_min_freq 0
/sys/devices/system/cpu/cpu1/cpufreq/scaling_max_freq 1400000

# gpu clock settings
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/min_freq 0
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/max_freq 1120000000

# emc clock settings
/sys/kernel/nvpmodel_emc_cap/emc_iso_cap 1600000000

< POWER_MODEL ID=3 NAME=MAXP_CORE_ARM >
# cpu core settings
/sys/devices/system/cpu/cpu1/online 0
/sys/devices/system/cpu/cpu2/online 0
/sys/devices/system/cpu/cpu3/online 1
/sys/devices/system/cpu/cpu4/online 1
/sys/devices/system/cpu/cpu5/online 1

# cpu clock settings
# A57 cluster
/sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq 0
/sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq 2000000

# gpu clock settings
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/min_freq 0
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/max_freq 1120000000

# emc clock settings
/sys/kernel/nvpmodel_emc_cap/emc_iso_cap 1600000000

< POWER_MODEL ID=4 NAME=MAXP_CORE_DENVER >
# cpu core settings
/sys/devices/system/cpu/cpu1/online 1
/sys/devices/system/cpu/cpu2/online 0
/sys/devices/system/cpu/cpu3/online 0
/sys/devices/system/cpu/cpu4/online 0
/sys/devices/system/cpu/cpu5/online 0

# cpu clock settings
# A57 cluster
/sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq 0
/sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq 345600
# Denver cluster
/sys/devices/system/cpu/cpu1/cpufreq/scaling_min_freq 0
/sys/devices/system/cpu/cpu1/cpufreq/scaling_max_freq 2035200

# gpu clock settings
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/min_freq 0
/sys/devices/17000000.gp10b/devfreq/17000000.gp10b/max_freq 1120000000

# emc clock settings
/sys/kernel/nvpmodel_emc_cap/emc_iso_cap 1600000000

# mandatory section to configure the default mode
< PM_CONFIG DEFAULT=3 >
```

내용을 대략 살펴보면 ARM A57 Core 인 CPU0 의 경우 항상 켜져 있는 것을 알 수 있다.

EMC Clock 이 뭔지는 잘 모르겠는데, 따로 알아봐야 할 것 같다.

2017년 8월 18일 기준 `/etc/nvpmodel.conf` 의 내용은 다음과 같다.

```txt
#
# Copyright (c) 2017, NVIDIA CORPORATION.  All rights reserved.
#
# NVIDIA CORPORATION and its licensors retain all intellectual property
# and proprietary rights in and to this software, related documentation
# and any modifications thereto.  Any use, reproduction, disclosure or
# distribution of this software and related documentation without an express
# license agreement from NVIDIA CORPORATION is strictly prohibited.
#
# FORMAT:
# < PARAM TYPE=PARAM_TYPE NAME=PARAM_NAME >
# ARG1_NAME ARG1_PATH_VAL
# ARG2_NAME ARG2_PATH_VAL
# ...
# This starts a section of PARAM definitions, in which each line
# has the syntax below:
# ARG_NAME ARG_PATH_VAL
# ARG_NAME is a macro name for argument value ARG_PATH_VAL.
# PARAM_TYPE can be FILE, or CLOCK.
#
# < POWER_MODEL ID=id_num NAME=mode_name >
# PARAM1_NAME ARG11_NAME ARG11_VAL
# PARAM1_NAME ARG12_NAME ARG12_VAL
# PARAM2_NAME ARG21_NAME ARG21_VAL
# ...
# This starts a section of POWER_MODEL configurations, followed by
# lines with parameter settings as the format below:
# PARAM_NAME ARG_NAME ARG_VAL
# PARAM_NAME and ARG_NAME are defined in PARAM definition sections.
# ARG_VAL is an integer for PARAM_TYPE of CLOCK, and -1 is taken
# as INT_MAX. ARG_VAL is a string for PARAM_TYPE of FILE.
# This file must contain at least one POWER_MODEL section.
#
# < PM_CONFIG DEFAULT=default_mode >
# This is a mandatory section to specify one of the defined power
# model as the default.

###########################
#                         #
# PARAM DEFINITIONS       #
#                         #
###########################

< PARAM TYPE=FILE NAME=CPU_ONLINE >
CORE_0 /sys/devices/system/cpu/cpu0/online
CORE_1 /sys/devices/system/cpu/cpu1/online
CORE_2 /sys/devices/system/cpu/cpu2/online
CORE_3 /sys/devices/system/cpu/cpu3/online
CORE_4 /sys/devices/system/cpu/cpu4/online
CORE_5 /sys/devices/system/cpu/cpu5/online

< PARAM TYPE=CLOCK NAME=CPU_A57 >
FREQ_TABLE /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_frequencies
MAX_FREQ /sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq
MIN_FREQ /sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq

< PARAM TYPE=CLOCK NAME=CPU_DENVER >
FREQ_TABLE /sys/devices/system/cpu/cpu1/cpufreq/scaling_available_frequencies
MAX_FREQ /sys/devices/system/cpu/cpu1/cpufreq/scaling_max_freq
MIN_FREQ /sys/devices/system/cpu/cpu1/cpufreq/scaling_min_freq

< PARAM TYPE=CLOCK NAME=GPU >
FREQ_TABLE /sys/devices/17000000.gp10b/devfreq/17000000.gp10b/available_frequencies
MAX_FREQ /sys/devices/17000000.gp10b/devfreq/17000000.gp10b/max_freq
MIN_FREQ /sys/devices/17000000.gp10b/devfreq/17000000.gp10b/min_freq

< PARAM TYPE=CLOCK NAME=EMC >
MAX_FREQ /sys/kernel/nvpmodel_emc_cap/emc_iso_cap

###########################
#                         #
# POWER_MODEL DEFINITIONS #
#                         #
###########################

# MAXN is the NONE power model to release all constraints
< POWER_MODEL ID=0 NAME=MAXN >
CPU_ONLINE CORE_1 1
CPU_ONLINE CORE_2 1
CPU_ONLINE CORE_3 1
CPU_ONLINE CORE_4 1
CPU_ONLINE CORE_5 1
CPU_A57 MIN_FREQ 0
CPU_A57 MAX_FREQ -1
CPU_DENVER MIN_FREQ 0
CPU_DENVER MAX_FREQ -1
GPU MIN_FREQ 0
GPU MAX_FREQ -1
EMC MAX_FREQ 0

< POWER_MODEL ID=1 NAME=MAXQ >
CPU_ONLINE CORE_1 0
CPU_ONLINE CORE_2 0
CPU_ONLINE CORE_3 1
CPU_ONLINE CORE_4 1
CPU_ONLINE CORE_5 1
CPU_A57 MIN_FREQ 0
CPU_A57 MAX_FREQ 1200000
GPU MIN_FREQ 0
GPU MAX_FREQ 850000000
EMC MAX_FREQ 1331200000

< POWER_MODEL ID=2 NAME=MAXP_CORE_ALL >
CPU_ONLINE CORE_1 1
CPU_ONLINE CORE_2 1
CPU_ONLINE CORE_3 1
CPU_ONLINE CORE_4 1
CPU_ONLINE CORE_5 1
CPU_A57 MIN_FREQ 0
CPU_A57 MAX_FREQ 1400000
CPU_DENVER MIN_FREQ 0
CPU_DENVER MAX_FREQ 1400000
GPU MIN_FREQ 0
GPU MAX_FREQ 1120000000
EMC MAX_FREQ 1600000000

< POWER_MODEL ID=3 NAME=MAXP_CORE_ARM >
CPU_ONLINE CORE_1 0
CPU_ONLINE CORE_2 0
CPU_ONLINE CORE_3 1
CPU_ONLINE CORE_4 1
CPU_ONLINE CORE_5 1
CPU_A57 MIN_FREQ 0
CPU_A57 MAX_FREQ 2000000
GPU MIN_FREQ 0
GPU MAX_FREQ 1120000000
EMC MAX_FREQ 1600000000

< POWER_MODEL ID=4 NAME=MAXP_CORE_DENVER >
CPU_ONLINE CORE_1 1
CPU_ONLINE CORE_2 0
CPU_ONLINE CORE_3 0
CPU_ONLINE CORE_4 0
CPU_ONLINE CORE_5 0
CPU_A57 MIN_FREQ 0
CPU_A57 MAX_FREQ 345600
CPU_DENVER MIN_FREQ 0
CPU_DENVER MAX_FREQ 2035200
GPU MAX_FREQ 1120000000
EMC MAX_FREQ 1600000000

# mandatory section to configure the default mode
< PM_CONFIG DEFAULT=3 >
```

다음 글은 Power Monitoring (소비전력 측정)에 관련된 내용이다.

---
layout: archive
lang: jp
ref: additional_sensors
read_time: true
share: true
author_profile: false
permalink: /docs/en/platform/turtlebot3/additional_sensors/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 19
---

<div style="counter-reset: h1 8"></div>
<div style="counter-reset: h2 3"></div>

<!--[dummy Header 1]>
  <h1 id="basic-operation"><a href="#basic-operation">Basic Operation</a></h1>
<![end dummy Header 1]-->

## [追加センサー](#additional-sensors)
TurtleBot3には、追加センサーを取り付けることができます。ここでは、TurtleBot3のOpenCRで、赤外線、超音波、スイッチなどの追加センサーを使用する方法を例として示しています。


### [バンパー](#Bumper)
 * デバイス - [Touch_sensor (TS-10)](http://emanual.robotis.com/docs/en/parts/sensor/ts-10/)


![](/assets/images/platform/turtlebot3/additional_sensors/touch_sensor.png)

(正面)

![](/assets/images/platform/turtlebot3/additional_sensors/touch_sensor_front.png)

(裏面)

![](/assets/images/platform/turtlebot3/additional_sensors/touch_sensor_back.png)

* デフォルトのピン      

| デバイス       | ピン             |
|:-------------|:----------------|
| Front sensor | ROBOTIS_5-ピン 3 |
| Back sensor  | ROBOTIS_5-ピン 4 |


**ヒント :** 他のピンを使用する場合には、[OpenCR PIN Map](http://emanual.robotis.com/docs/en/parts/controller/opencr10/)を参照してください。
{: .notice--info}

* Turtlebot3で実行

**警告** : Exampleサンプルを実行する前に必ず[Bringup](#bringup)コマンドを実行してください。
{: .notice--warning}

**[リモートPC]** bumper launch fileを起動します。
``` bash
$ roslaunch turtlebot3_example turtlebot3_bumper.launch
```


* Arduino IDEで実行

このサンプルは、[Arduino IDE](http://emanual.robotis.com/docs/en/parts/controller/opencr10/#arduino-ide)で開くことができます。

`File` -> `Examples` -> `ROS` -> `2. Sensors` -> `a_Bumper`を選択して、OpenCRにアップロードします。

**[リモートPC]** ros serial_nodeパッケージを実行します。

``` bash
$ rosrun rosserial_python serial_node.py __name:=turtlebot3_core _port:=/dev/ttyACM0 _baud:=115200
```

**警告** : OpenCRにサンプルをアップロードする場合は、 [turtlebot3_core](http://emanual.robotis.com/docs/en/platform/turtlebot3/opencr_setup/#opencr-setup)を再アップロードする必要があります。
{: .notice--warning}

### [IR](#IR)

* デバイス - [IR_sensor (IRSS-10)](http://emanual.robotis.com/docs/en/parts/sensor/irss-10/)


![](/assets/images/platform/turtlebot3/additional_sensors/IR_sensor.png)

![](/assets/images/platform/turtlebot3/additional_sensors/IR_sensor_front.png)

* デフォルトピン

| デバイス    | ピン             |
|:----------|:----------------|
| IRセンサ | ROBOTIS_5-PIN 2 |

**ヒント :** 他のピンを使用する場合には、[OpenCR PIN Map](http://emanual.robotis.com/docs/en/parts/controller/opencr10/)を参照してください。
{: .notice--info}

* Turtlebot3で実行

**警告** : サンプルを実行する前に必ず[Bringup](#bringup)コマンドを実行してください。
{: .notice--warning}

**[リモートPC]** cliff launch fileを起動します。
``` bash
$ roslaunch turtlebot3_example turtlebot3_cliff.launch
```


* Arduino IDEで実行

This example can be open [Arduino IDE](http://emanual.robotis.com/docs/en/parts/controller/opencr10/#arduino-ide).

`File` -> `Examples` -> `ROS` -> `2. Sensors` -> `b_Cliff`を選び、OpenCRへアップロードします・

**[リモートPC]** ros serial_nodeを実行します。

``` bash
$ rosrun rosserial_python serial_node.py __name:=turtlebot3_core _port:=/dev/ttyACM0 _baud:=115200
```



### [超音波センサ](#Ultrasonic)
* デバイス - 超音波センサ (HC-SR04)

![](/assets/images/platform/turtlebot3/additional_sensors/sonar.png)


* デフォルトピン:


| デバイス  | ピン          |
|:--------|:-------------|
| トリガ | BDPIN_GPIO_1 |
| エコー    | BDPIN_GPIO_2 |

**ヒント :** 他のピンを使用する場合には[OpenCR PIN Map](http://emanual.robotis.com/docs/en/parts/controller/opencr10/)を参照してください。
{: .notice--info}

* Turtlebot3にて実行

**警告** : サンプルを実行する前に必ず[Bringup](#bringup)コマンドを実行してください。
{: .notice--warning}

**[リモートPC]** sonar launch fileを起動します。
``` bash
$ roslaunch turtlebot3_example turtlebot3_sonar.launch
```

* Arduino IDEで実行

このサンプルは、[Arduino IDE](http://emanual.robotis.com/docs/en/parts/controller/opencr10/#arduino-ide)で開くことができます。

`File` -> `Examples` -> `ROS` -> `2. Sensors` -> `c_Ultrasonic`を選び、OpenCRへアップロードします。

**[リモートPC]** ros serial_node packageを実行します。

``` bash
$ rosrun rosserial_python serial_node.py __name:=turtlebot3_core _port:=/dev/ttyACM0 _baud:=115200
```


### [イルミネーション](#Illumination)
*  デバイス - LDR sensor (Flying-Fish MH-sensor)

![](/assets/images/platform/turtlebot3/additional_sensors/illumination.png)

![](/assets/images/platform/turtlebot3/additional_sensors/illumination_front.png)

*  デフォルトピン

| デバイス | ピン |
|:-------|:----|
| アナログ | A1  |

**ヒント :** 他のピンを使用する場合には、[OpenCR PIN Map](http://emanual.robotis.com/docs/en/parts/controller/opencr10/)を参照してください。
{: .notice--info}

* Turtlebot3で実行

**警告** : サンプルを実行する前に必ず[Bringup](#bringup)コマンドを実行してください。
{: .notice--warning}

**[リモートPC]** illumination launch fileを起動します。
``` bash
$ roslaunch turtlebot3_example turtlebot3_illumination.launch
```

* Arduino IDEで実行

このサンプルは、[Arduino IDE](http://emanual.robotis.com/docs/en/parts/controller/opencr10/#arduino-ide)で開くことができます。

`File` -> `Examples` -> `ROS` -> `2. Sensors` -> `d_Illumination`を選び、OpenCRへアップロードします。

**[リモートPC]** ros serial_node packageを実行します。

``` bash
$ rosrun rosserial_python serial_node.py __name:=turtlebot3_core _port:=/dev/ttyACM0 _baud:=115200
```

### [LED](#LED)
*  デバイス - led (led101)

![](/assets/images/platform/turtlebot3/additional_sensors/led.png)

![](/assets/images/platform/turtlebot3/additional_sensors/led_top.png)

*  デフォルトピン

| デバイス      | ピン           |
|:------------|:--------------|
| Front_left  | BDPIN_GPIO_4  |
| Front_right | BDPIN_GPIO_6  |
| Back_left   | BDPIN_GPIO_8  |
| Back_right  | BDPIN_GPIO_10 |

**ヒント :** 他のピンを使用する場合には、[OpenCR PIN Map](http://emanual.robotis.com/docs/en/parts/controller/opencr10/)を参照してください。
{: .notice--info}

*  実行

このサンプルでは、ledが接続されている場合は常にアクティブであり、ledはTurtlebot3の並進速度と角速度に依存して特定のパターンを示します。

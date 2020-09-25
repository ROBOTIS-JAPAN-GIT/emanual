---
layout: archive
lang: jp
ref: teleoperation
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/teleoperation/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 17
---

<div style="counter-reset: h1 8"></div>
<div style="counter-reset: h2 1"></div>

<!--[dummy Header 1]>
  <h1 id="basic-operation"><a href="#basic-operation">Basic Operation</a></h1>
<![end dummy Header 1]-->

## [遠隔操作](#ros-teleoperation)

![](/assets/images/platform/turtlebot3/software/remote_pc_and_turtlebot.png)

{% capture notice_01 %}
**注釈**：
- このコマンドは `Ubuntu 16.04` と `ROS Kinetic Kame` でテストをしました。
- この例は、リモートPCでの動作を想定しています。**リモートPC** の指示に従ってください。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

**警告**：この例を実行する前に、必ず[Bringup][bringup]コマンドを実行してください。また、テーブルの上でロボットをテストする際には、ロボットが落下してしまう可能性があるため、注意してください。
{: .notice--warning}

TurtleBot3は、様々な機器での遠隔操作が可能です。PS3、 XBOX 360、 ROBOTIS RC100などの無線機器で動作確認をしています。ここで紹介した例（LEAP Motionを除く）は、Raspberry Pi 3のUbuntu mate 16.04上のROSとDYNAMIXELを制御するOpenCRによって起動することができます。

<iframe width="640" height="360" src="https://www.youtube.com/embed/Z4s18hlazb4" frameborder="0" allowfullscreen></iframe>

e-Manualの内容は、予告なしで更新される場合があります。そのため、いくつかの動画はe-Manualの内容と異なる場合があります。
{: .notice--warning}

### [キーボード](#keyboard)
**ヒント**：ターミナルアプリは、画面左上のUbuntuの検索アイコンで見つけることができます。ターミナルのショートカットキーは `Ctrl`-`Alt`-`T` です。
{: .notice--info}

**ヒント**：このコマンドを実行する前に、TurtleBot3のモデル名を指定する必要があります。`${TB3_MODEL}` は `burger`、`waffle`、`waffle_pi` であり、使用するモデルの名前です。このエクスポート設定を今後も引き続き使用したい場合は、[Export TURTLEBOT3_MODEL][export_turtlebot3_model]{: .popup}のページを参照してください。
{: .notice--success}

**[リモートPC]** 簡易的な遠隔操作のテストを行うために`turtlebot3_teleop_key`ノードを起動します。

``` bash
$ export TURTLEBOT3_MODEL=%{TB3_MODEL}
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

ノードの起動に成功すると、ターミナルウィンドウに以下のようなコマンドが表示されます。

``` bash
  Control Your Turtlebot3!
  ---------------------------
  Moving around:
          w
     a    s    d
          x

  w/x : increase/decrease linear velocity
  a/d : increase/decrease angular velocity
  space key, s : force stop

  CTRL-C to quit
```

### [RC100](#rc100)

[ROBOTIS RC-100B][rc100]コントローラの設定は、TurtleBot3 Burger、Waffle、Waffle Pi用のOpenCRファームウェアに含まれています。このコントローラは、Bluetoothモジュール[BT410][BT410]と組み合わせて使用することができます。TurtleBot3 Waffle Piには、本コントローラとBluetoothモジュールが同梱されています。RC-100を使用する場合、`turtlebot_core`ノードはOpenCRに直結したファームウェア内に`/cmd_vel`トピックを作成するため、特定のノードを実行する必要はありません。

![](/assets/images/platform/turtlebot3/example/rc100b_with_bt410.png)

### [PS3ジョイステック](#ps3-joystick)

**[リモートPC]** PS3ジョイスティックとリモートPCをBluetoothまたはUSBケーブル経由で接続します。

**[リモートPC]** PS3ジョイスティックを使用した遠隔操作を行うためのパッケージをインストールします。

``` bash
$ sudo apt-get install ros-kinetic-joy ros-kinetic-joystick-drivers ros-kinetic-teleop-twist-joy
```

**[リモートPC]** PS3ジョイスティック用の遠隔操作パッケージを起動します。

``` bash
$ roslaunch teleop_twist_joy teleop.launch
```

### [XBOX 360ジョイステック](#xbox-360-joystick)

**[リモートPC]** 360ジョイスティックとリモートPCをワイヤレスアダプタまたはUSBケーブルによって接続します。

**[リモートPC]** XBOX 360ジョイスティックを使用した遠隔操作を行うためのパッケージをインストールします。

``` bash
$ sudo apt-get install xboxdrv ros-kinetic-joy ros-kinetic-joystick-drivers ros-kinetic-teleop-twist-joy
```

**[リモートPC]** XBOX 360ジョイスティック用の遠隔操作パッケージを起動します。

``` bash
$ sudo xboxdrv --silent
$ roslaunch teleop_twist_joy teleop.launch
```

### [Wiiリモコン](#wii-remote)

**[リモートPC]** WiiリモコンとリモートPCをBluetooth経由で接続します。

**[リモートPC]** Wiiリモコンを使用した遠隔操作を行うためのパッケージをインストールします。

``` bash
$ sudo apt-get install ros-kinetic-wiimote libbluetooth-dev libcwiid-dev
```

``` bash
$ cd ~/catkin_ws/src
$ git clone https://github.com/ros-drivers/joystick_drivers.git  
$ cd ~/catkin_ws && catkin_make
```

**[リモートPC]** Wiiリモコン用の遠隔操作パッケージを実行します。

``` bash
$ rosrun wiimote wiimote_node
$ rosrun wiimote teleop_wiimote
```

### [ヌンチャク](#nunchuk)

(TODO)

### [Androidアプリ](#android-app)

[ROS CONTROL][ros_control]をダウンロードし、アプリを起動します。

ROS_CONTROLアプリに`roscore`を接続したら、`Preferences`の`Topic`タブにトピック名を入力します。

![](/assets/images/platform/turtlebot3/example/ros_control.png)

ジョイスティックトピックでは `/cmd_vel`、レーザースキャントピックでは `/scan` のようにトピック名を変更する必要があります。
画像トピックでは `/image_raw/compressed`、オドメトリトピックでは `/odom` を使用します。

そして、`rqt_graph`コマンドを使うことで、ノードとトピックの接続状態を確認することができます。


![](/assets/images/platform/turtlebot3/example/ros_control_graph.png)

### [LEAP Motion](#leap-motion)

**[リモートPC]** LEAP motionとリモートPCをBluetooth経由で接続します。

**[リモートPC]** LEAP motionを使用した遠隔操作用のパッケージをインストールします。

- [LEAPのセットアップ][leap_setup]
- [SDKのダウンロード][leap_sdk]

``` bash
$ leapd
$ LeapCommandPanel
$ git clone git@github.com:warp1337/rosleapmotion.git
```

**[リモートPC]** LEAP motion用の遠隔操作パッケージを実行します。

``` bash
$ rosrun leap_motion sender.py
```

### [Myo](#myo)

コンテンツの開発を進めています。
{: .notice--info}

[bringup]: /docs/en/platform/turtlebot3/bringup/#bringup
[rc100]: /docs/en/parts/communication/rc-100/
[bt410]: /docs/en/parts/communication/bt-410/
[ros_control]: https://play.google.com/store/apps/details?id=com.robotca.ControlApp
[leap_setup]: https://www.leapmotion.com/setup
[leap_sdk]: https://developer.leapmotion.com/get-started/

---
layout: archive
lang: jp
ref: applications
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/applications/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 28
---

<div style="counter-reset: h1 14"></div>

# [[ROS 1] アプリケーション](#ros-1-アプリケーション)

{% capture notice_01 %}
**注釈**：
- この手順は、`Ubuntu16.04`と`ROSKineticKame`でテスト済みです。
- この命令は、リモートPCを想定しています。 **リモートPC**で以下の手順を実行してください。ただし、**[TurtleBot]**とマークされた部分は、TurtleBot3のSBCで実行されるコンテンツです。
- 命令を使用する前に、必ず[Bringup]（/docs/jp/platform/turtlebot3/bringup/＃bringup）の手順を実行してください。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

**ヒント**: ターミナルアプリは、画面左上のUbuntu検索アイコンで見つけることができます。ターミナルのショートカットキーは、`Ctrl`-`Alt`-`T`です。
{: .notice--success}

この章では、TurtleBot3を使用したいくつかのデモを示します。 これらのデモを実装するには、[turtlebot3_applications][turtlebot3_applications]および[turtlebot3_applications_msgs][turtlebot3_applications_msgs]パッケージをインストールする必要があります。

**[リモートPC]** `catkinworkspace`ディレクトリ（/home/(user_name)/catkin_ws/src）に移動し、turtlebot3_applicationsおよびturtlebot3_applications_msgsリポジトリのクローンを作成します。 次に、`catkin_make`を実行して新しいパッケージをビルドします。

``` bash
$ sudo apt-get install ros-kinetic-ar-track-alvar
$ sudo apt-get install ros-kinetic-ar-track-alvar-msgs
$ cd ~/catkin_ws/src
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_applications.git
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_applications_msgs.git
$ cd ~/catkin_ws && catkin_make
```

## [TurtleBotフォロワーデモ](#turtlebotフォロワーデモ)

{% capture notice_02 %}
**注釈**:
- フォロワーのデモは、360°レーザー距離センサ LDS-01だけを使用するように実装されました。 分類アルゴリズムは、アクションを実行するために人と障害物の位置サンプルを使用した直前のフィッティングに基づいて使用されます。 これは、50cmの範囲と140°の範囲内でロボットの前にいる誰かを追跡します。
- 障害物のあるエリアでフォロワーデモを実行すると、うまく機能しない場合があります。 したがって、障害物のないオープンエリアでデモを実行することを推奨します。
{% endcapture %}
<div class="notice--info">{{ notice_02 | markdownify }}</div>

**[TurtleBot]** このデモを実行するには、LIDAR起動ファイルのパラメーターを変更する必要があります。 以下の例では、Plumaを使用して起動ファイルを編集しています。 名前に `frame_id`が付いたparamタグで、` base_scan`を`odom`に置き換え、次の図に示すようにファイルを保存します。

``` bash
$ pluma ~/catkin_ws/src/turtlebot3/turtlebot3_bringup/launch/turtlebot3_lidar.launch
```

![](/assets/images/platform/turtlebot3/application/base_scan.png)


![](/assets/images/platform/turtlebot3/application/odom.png)

**注釈**：Turtlebot Follower Demoには、`scikit-learn`、`NumPy`、および `ScyPy`パッケージが必要です。
{: .notice--info}

**[リモートPC]** 以下のコマンドを使用して、`scikit-learn`、`NumPy`、および`ScyPy`パッケージをインストールします。

``` bash
$ sudo apt-get install python-pip
$ sudo pip install -U scikit-learn numpy scipy
$ sudo pip install --upgrade pip
```

**[リモートPC]** インストールが完了後、以下のコマンドを使用してリモートPCでroscoreを実行します。

``` bash
$ roscore
```

**[TurtleBot]** [bringup][bringup]を起動します。

``` bash
$ roslaunch turtlebot3_bringup turtlebot3_robot.launch
```

**[リモートPC]** 以下のコマンドで`turtlebot3_follow_filter`を起動します。

``` bash
$ roslaunch turtlebot3_follow_filter turtlebot3_follow_filter.launch
```

**[リモートPC]** 以下のコマンドで `turtlebot3_follower`を起動します。

``` bash
$ roslaunch turtlebot3_follower turtlebot3_follower.launch
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/w9YTxZVY6yQ" frameborder="0" allowfullscreen></iframe>

## [TurtleBotパノラマデモ](#TurtleBotパノラマデモ)

**注釈**：
- `turtlebot3_panorama`デモでは、`pano_ros`を使用してスナップショットを撮り、それらをつなぎ合わせてパノラマ画像を作成します。
- パノラマデモでは、 `raspicam_node`パッケージをインストールする必要があります。このパッケージのインストール手順は、[Gihubリンク](https://github.com/UbiquityRobotics/raspicam_node)にあります。
- パノラマデモでは、OpenCVおよびcvbridgeパッケージをインストールする必要があります。OpenCVのインストール手順は、[OpenCVチュートリアルリンク](http://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html)にあります。
{% endcapture %}
<div class="notice--info">{{ notice_03 | markdownify }}</div>

**[TurtleBot]** `turtlebot3_rpicamera`を起動します。

``` bash
$ roslaunch turtlebot3_bringup turtlebot3_rpicamera.launch
```

**[リモートPC]** 以下のコマンドで、`panorama`を起動します。

``` bash
$ roslaunch turtlebot3_panorama panorama.launch
```

**[リモートPC]** パノラマデモを開始するには、以下のコマンドを入力してください。

``` bash
$ rosservice call turtlebot3_panorama/take_pano 0 360.0 30.0 0.3
```

パノラマ画像を取得するためにrosserviceに送信できるパラメータは次のとおりです。

- 写真を撮るためのモード。

    - 0 : スナップ＆回転（つまり、回転、停止、スナップショット、回転、停止、スナップショットなど）
    - 1 : 継続的（つまり、スナップショットを撮りながら回転し続ける）
    - 2 : 写真の撮影をやめてパノラマ画像を作成する

- パノラマ画像の全角度（度単位）
- スナップ＆回転モードでパノラマ画像を作成する場合の角度間隔（度単位）、それ以外の場合は時間間隔（秒単位）
- 回転速度（ラジアン/秒）

**[リモートPC]** 結果画像を表示するには、以下のコマンドを入力してください。

``` bash
$ rqt_image_view image:=/turtlebot3_panorama/panorama
```

![](/assets/images/platform/turtlebot3/application/panorama_view.png)

## [自動駐車](#自動駐車)

{% capture notice_04 %}
**注釈**:
- `turtlebot3_automatic_parking`デモは、360°レーザー距離センサ LDS-01と反射テープを使用しました。 LaserScanトピックには、LDSからの強度と距離のデータがあります。 TurtleBot3は、これを使用して反射テープを見つけます。
- `turtlebot3_automatic_parking`デモには、`NumPy`パッケージが必要です。
{% endcapture %}
<div class="notice--info">{{ notice_04 | markdownify }}</div>

**[リモートPC]** 以下のコマンドで `NumPy`パッケージをインストールします。 すでにnumpyをインストールしている場合は、以下のコマンドを**スキップ**できます。

``` bash
$ sudo apt-get install python-pip
$ sudo pip install -U numpy
$ sudo pip install --upgrade pip
```

**[リモートPC]** roscoreを実行します。

```bash
$ roscore
```

**[TurtleBot]** TurtleBot3アプリケーションを起動するための基本パッケージを起動します。

```bash
$ roslaunch turtlebot3_bringup turtlebot3_robot.launch
```

**[リモートPC]** TurtleBot3 Burgerを使用する場合は、以下のコマンドの用にTurtleBot3のモデルを設定します。

**ヒント**： このコマンドを実行する前に、TurtleBot3のモデル名を指定する必要があります。 `$ {TB3_MODEL}`は、 `burger`、`waffle`、`waffle_pi`で使用しているモデルの名前です。エクスポート設定を永続的に設定する場合は、[Export TURTLEBOT3_MODEL][export_turtlebot3_model]{: .popup}を参照してください。
{: .notice--success}

```bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
```

**[リモートPC]** Rvizを起動します。

```bash
$ roslaunch turtlebot3_bringup turtlebot3_remote.launch
$ rosrun rviz rviz -d `rospack find turtlebot3_automatic_parking`/rviz/turtlebot3_automatic_parking.rviz
```

**[Remote PC]** 自動駐車ファイルを起動します。

``` bash
$ roslaunch turtlebot3_automatic_parking turtlebot3_automatic_parking.launch  
```

- RVizでLaserScanトピックを選択できます。

- `/scan`

![](/assets/images/platform/turtlebot3/application/scan.png)

- `/scan_spot`

![](/assets/images/platform/turtlebot3/application/scan_spot.png)

<iframe width="560" height="315" src="https://www.youtube.com/embed/IRtdxoPo8Y8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


## [ビジョンベースの自動駐車](#ビジョンベースの自動駐車)

{% capture notice_05 %}
**注釈**：
- `turtlebot3_automatic_parking_vision`はラズベリーパイカメラを使用しているため、このデモで使用されるデフォルトのプラットフォームであるロボットはTurtleBot3 Waffle Piです。ARマーカを見つけて駐車するため、壁に印刷したARマーカーを用意する必要があります。 プロセス全体でカメラから取得した画像が使用されるため、プロセスが適切に行われていない場合は、明るさ、コントラストなどのパラメーターを構成します。
- `turtlebot3_automatic_parking_vision`は、 `image_proc`ノードに基づいて修正された画像を使用します。 修正された画像を取得するには、ロボットはラズベリーパイカメラの光学キャリブレーションデータを取得する必要があります。(ダウンロードされたすべてのturtlebot3パッケージには、ラズベリーパイカメラv2のデフォルトとしてカメラキャリブレーションデータがすでに含まれています。)
- `turtlebot3_automatic_parking_vision`パッケージには `ar_track_alvar`パッケージが必要です。
{% endcapture %}
<div class="notice--info">{{ notice_05 | markdownify }}</div>


**[リモートPC]** roscoreを実行します。

```bash
$ roscore
```

**[TurtleBot]** 基本的なパッケージを起動して、TurtleBot3アプリケーションを起動します。

```bash
$ roslaunch turtlebot3_bringup turtlebot3_robot.launch
```

**[TurtleBot]** ラズベリーパイカメラノードを起動します。

```bash
$ roslaunch turtlebot3_bringup turtlebot3_rpicamera.launch
```

**[リモートPC]** ラズベリーパイのパッケージは、高速通信のために圧縮タイプのイメージを公開します。 ただし、 `image_proc`ノードでの画像修正に必要なのはrawタイプの画像です。 したがって、圧縮された画像は生の画像に変換する必要があります。

```bash
$ rosrun image_transport republish compressed in:=raspicam_node/image raw out:=raspicam_node/image
```

**[リモートPC]** 次に、画像の修正を実行する必要があります。

```bash
$ ROS_NAMESPACE=raspicam_node rosrun image_proc image_proc image_raw:=image _approximate_s=true _queue_size:=20
```

**[リモートPC]** これでARマーカーの検出が開始されます。 関連する起動ファイルを実行する前に、このサンプルコードで使用されるモデルをエクスポートする必要があります。 起動ファイルを実行した後、RVizは事前に設定された環境で自動的に実行されます。

```bash
$ export TURTLEBOT3_MODEL=waffle_pi
$ roslaunch turtlebot3_automatic_parking_vision turtlebot3_automatic_parking_vision.launch
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/dvpWdrD3bVs" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

e-Manualの内容は、予告なしに更新される場合があります。 そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

## [複数のTurtlebBot3をロードする](複数のTurtlebBot3をロードする)

**注釈**：このアプリケーションは、ファームウェアバージョン `1.2.1`以降に設定する必要があります。
{: .notice--info}

**[リモートPC]** roscoreを実行します。

```bash
$ roscore
```

名前空間が異なる複数のturtlebot3を起動します。 名前空間には、`tb3_0`、`tb3_1`または `my_robot_0`、`my_robot_1`などの一般的な単語を含めることを推奨します。

**[TurtleBot（tb3_0）]** ノードに`ROS NAMESPACE`、tfプレフィックスに` multi_robot_name`、LIDARフレームIDに`set_lidar_frame_id`を含む基本パッケージを起動します。 このパラメータは同じである必要があります。

```bash
$ ROS_NAMESPACE=tb3_0 roslaunch turtlebot3_bringup turtlebot3_robot.launch multi_robot_name:="tb3_0" set_lidar_frame_id:="tb3_0/base_scan"
```

**[TurtleBot（tb3_1）]** ノードに`ROS NAMESPACE`、tfプレフィックスに` multi_robot_name`、LIDARフレームIDに`set_lidar_frame_id`を含む基本パッケージを起動します。 このパラメータは同じである必要がありますが、他のロボットは異なります。

```bash
$ ROS_NAMESPACE=tb3_1 roslaunch turtlebot3_bringup turtlebot3_robot.launch multi_robot_name:="tb3_1" set_lidar_frame_id:="tb3_1/base_scan"
```

次に、`tb3_0`を起動した端末が以下のメッセージを表します。 TFメッセージのプレフィックスが `tb3_0`となっている事が確認できます。

```
SUMMARY
========

PARAMETERS
 * /rosdistro: kinetic
 * /rosversion: 1.12.13
 * /tb3_0/turtlebot3_core/baud: 115200
 * /tb3_0/turtlebot3_core/port: /dev/ttyACM0
 * /tb3_0/turtlebot3_core/tf_prefix: tb3_0
 * /tb3_0/turtlebot3_lds/frame_id: tb3_0/base_scan
 * /tb3_0/turtlebot3_lds/port: /dev/ttyUSB0

NODES
  /tb3_0/
    turtlebot3_core (rosserial_python/serial_node.py)
    turtlebot3_diagnostics (turtlebot3_bringup/turtlebot3_diagnostics)
    turtlebot3_lds (hls_lfcd_lds_driver/hlds_laser_publisher)

ROS_MASTER_URI=http://192.168.1.2:11311

process[tb3_0/turtlebot3_core-1]: started with pid [1903]
process[tb3_0/turtlebot3_lds-2]: started with pid [1904]
process[tb3_0/turtlebot3_diagnostics-3]: started with pid [1905]
[INFO] [1531356275.722408]: ROS Serial Python Node
[INFO] [1531356275.796070]: Connecting to /dev/ttyACM0 at 115200 baud
[INFO] [1531356278.300310]: Note: publish buffer size is 1024 bytes
[INFO] [1531356278.303516]: Setup publisher on sensor_state [turtlebot3_msgs/SensorState]
[INFO] [1531356278.323360]: Setup publisher on version_info [turtlebot3_msgs/VersionInfo]
[INFO] [1531356278.392212]: Setup publisher on imu [sensor_msgs/Imu]
[INFO] [1531356278.414980]: Setup publisher on cmd_vel_rc100 [geometry_msgs/Twist]
[INFO] [1531356278.449703]: Setup publisher on odom [nav_msgs/Odometry]
[INFO] [1531356278.466352]: Setup publisher on joint_states [sensor_msgs/JointState]
[INFO] [1531356278.485605]: Setup publisher on battery_state [sensor_msgs/BatteryState]
[INFO] [1531356278.500973]: Setup publisher on magnetic_field [sensor_msgs/MagneticField]
[INFO] [1531356280.545840]: Setup publisher on /tf [tf/tfMessage]
[INFO] [1531356280.582609]: Note: subscribe buffer size is 1024 bytes
[INFO] [1531356280.584645]: Setup subscriber on cmd_vel [geometry_msgs/Twist]
[INFO] [1531356280.620330]: Setup subscriber on sound [turtlebot3_msgs/Sound]
[INFO] [1531356280.649508]: Setup subscriber on motor_power [std_msgs/Bool]
[INFO] [1531356280.688276]: Setup subscriber on reset [std_msgs/Empty]
[INFO] [1531356282.022709]: Setup TF on Odometry [tb3_0/odom]
[INFO] [1531356282.026863]: Setup TF on IMU [tb3_0/imu_link]
[INFO] [1531356282.030138]: Setup TF on MagneticField [tb3_0/mag_link]
[INFO] [1531356282.033628]: Setup TF on JointState [tb3_0/base_link]
[INFO] [1531356282.041117]: --------------------------
[INFO] [1531356282.044421]: Connected to OpenCR board!
[INFO] [1531356282.047700]: This core(v1.2.1) is compatible with TB3 Burger
[INFO] [1531356282.051355]: --------------------------
[INFO] [1531356282.054785]: Start Calibration of Gyro
[INFO] [1531356284.585490]: Calibration End
```
**[リモートPC]** 同一の名前空間でロボットステートパブリッシャーを起動します。

```bash
$ ROS_NAMESPACE=tb3_0 roslaunch turtlebot3_bringup turtlebot3_remote.launch multi_robot_name:=tb3_0
```

```bash
$ ROS_NAMESPACE=tb3_1 roslaunch turtlebot3_bringup turtlebot3_remote.launch multi_robot_name:=tb3_1
```

別のアプリケーションを起動する前に、rqtを開きTFツリーとトピックを確認します。

```bash
$ rqt
```

![](/assets/images/platform/turtlebot3/application/multi_turtlebot_rqt.png)

この設定を使用するために、各TurtleBot3はSLAMを使用してマップを作成し、これらのマップは[multi_map_merge][multi_map_merge]パッケージによって同時にマージされます。 詳細については、[複数のTurtleBot3による仮想SLAM][複数のTurtleBot3による仮想SLAM]セクションを参照してください。

[turtlebot3_applications]: https://github.com/ROBOTIS-GIT/turtlebot3_applications
[turtlebot3_applications_msgs]: https://github.com/ROBOTIS-GIT/turtlebot3_applications_msgs
[bringup]: /docs/en/platform/turtlebot3/bringup/#bringup
[teleoperation]: /docs/en/platform/turtlebot3/teleoperation/#teleoperation
[export_turtlebot3_model]: /docs/en/platform/turtlebot3/export_turtlebot3_model
[ar_track_alvar]: http://wiki.ros.org/ar_track_alvar
[multi_map_merge]: http://wiki.ros.org/multirobot_map_merge
[複数のTurtleBot3による仮想SLAM]]: /docs/en/platform/turtlebot3/simulation/#2-excute-slam

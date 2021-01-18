---
layout: archive
lang: jp
ref: manipulation
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/manipulation/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 23
---

<div style="counter-reset: h1 11"></div>

# [[ROS 1] マニピュレーション](#ros-1-マニピュレーション)

{% capture notice_01 %}
**注釈**:
- このコマンドは `Ubuntu 16.04` と `ROS Kinetic Kame` でテストをしました。
- OpenMANIPULATORの詳細については、[OpenMANIPULATOR e-Manual](/docs/en/platform/openmanipulator/)を参照してください。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

e-Manualの内容は、予告なしに更新される場合があります。そのため、一部の動画はe-Manualの内容と異なる場合があります。

{: .notice--warning} 

{% capture notice_02 %}
{% include en/platform/turtlebot3/ros_book_info.md %}
{% endcapture %}
<div class="notice--success">{{ notice_02 | markdownify }}</div>

**ヒント**: ターミナルアプリは、画面左上のUbuntu検索アイコンで見つけることができます。ターミナルのショートカットキーは、`Ctrl`-`Alt`-`T`です。
{: .notice--success}

## [TurtleBot3 with OpenMANIPULATOR](#turtlebot3-with-openmanipulator)

![](/assets/images/platform/turtlebot3/manipulation/tb3_with_opm_logo.png)

ROBOTIS社のOpenMANIPULATORは、ROSをサポートするマニピュレーターの1つであり、3Dプリント部品を備えたDYNAMIXELアクチュエーターを使用することで、低コストかつ簡単に製造できるという利点があります。

OpenMANIPULATORには、TurtleBot3 WaffleおよびWaffle Piと互換性があるという利点があります。この互換性により自由の欠如を補い、TurtleBot3が持つSLAMおよびナビゲーション機能を備えたサービスロボットとしての完全性を高めることができます。TurtleBot3とOpenMANIPULATORは`モバイルマニピュレータ`として使用でき、次のビデオのようなことを行うことができます。

<iframe width="560" height="315" src="https://www.youtube.com/embed/Qhvk5cnX2hM" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/P82pZsqpBg0" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/DLOq8yNcCoE" frameborder="0" allowfullscreen></iframe>

e-Manualの内容は、予告なしに更新される場合があります。そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

## [ソフトウェアセットアップ](#ソフトウェアセットアップ)

**注釈**: `open_manipulator_with_tb3`パッケージをインストールする前に、事前に`turtlebot3`および`open_manipulator`パッケージがRemotePCにインストールされ、セットアップ済みなことを確認してください。[Raspberry Pi 3](/docs/en/platform/turtlebot3/raspberry_pi_3_setup/#raspberry-pi-3-setup).
{: .notice--info}

- OpenMANIPULATOR with TurtleBot3の依存パッケージをインストールします。

**[リモートPC]**
```bash
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/open_manipulator_with_tb3.git
$ git clone https://github.com/ROBOTIS-GIT/open_manipulator_with_tb3_msgs.git
$ git clone https://github.com/ROBOTIS-GIT/open_manipulator_with_tb3_simulations.git
$ git clone https://github.com/ROBOTIS-GIT/open_manipulator_perceptions.git
$ sudo apt-get install ros-kinetic-smach* ros-kinetic-ar-track-alvar ros-kinetic-ar-track-alvar-msgs
$ cd ~/catkin_ws && catkin_make
```

- `catkin_make`コマンドがエラーなしで完了すると、OpenMANIPULATORの準備が完了します。次に、RVizにOpenMANIPULATOR with TurtleBot3 WaffleまたはWaffle Piをロードします。

**ヒント**: このコマンドを実行する前に、TurtleBot3のモデル名を指定する必要があります。 `${TB3_MODEL}`は、 `waffle`、`waffle_pi`で使用しているモデルの名前です。 エクスポート設定を永続的に設定する場合は、[Export TURTLEBOT3_MODEL][export_turtlebot3_model]を参照してください。{: .popup} page.
{: .notice--success}

**[リモートPC]**
```bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch open_manipulator_with_tb3_description open_manipulator_with_tb3_rviz.launch
```

![](/assets/images/platform/turtlebot3/manipulation/TurtleBot3_with_Open_Manipulator.png)

## [ハードウェアセットアップ](#hardware-setup)

- [CADファイル](http://www.robotis.com/service/download.php?no=767) (TurtleBot3 Waffle Pi + OpenMANIPULATOR)

![](/assets/images/platform/turtlebot3/manipulation/hardware_setup.png)

- まず、LIDARセンサーを取り外し、TurtleBot3の前に移動します（赤い円はボルトの位置を表します）。
- 次に、OpenMANIPULATORをTurtleBot3に取り付けます（黄色の円はボルトの位置を表します）。

![](/assets/images/platform/turtlebot3/manipulation/assemble_points.png)

![](/assets/images/platform/turtlebot3/manipulation/assemble.png)

## [OpenCR セットアップ](#opencr-setup)

{% capture notice_01 %}
**注釈**: ファームウェアのアップロード方法の1つを選択できます。 ただし、**シェルスクリプト**を使用することを強く推奨します。 TurtleBot3のファームウェアを変更する必要がある場合は、2番目の方法を使用できます。
- 方法 #1: [**シェルスクリプト**](#シェルスクリプト)、 シェルスクリプトを使用して、ビルド済みのバイナリファイルをアップロードします。
- 方法 #2: [**Arduino IDE**](#arduino-ide), 提供されたソースコードをビルドし、ArduinoIDEを使用して生成されたバイナリファイルをアップロードします。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

**警告** : **OpenCRのセットアップへ進む前に、すべてのDYNAMIXELがOpenCRに接続されていることを確認してください**。 そうしないと場合、ラズベリーパイボードに**予期しない問題**が発生する可能性があります。
{: .notice--warning}


### [シェルスクリプト](#シェルスクリプト)
**[TurtleBot3]**
```bash
$ export OPENCR_PORT=/dev/ttyACM0
$ export OPENCR_MODEL=om_with_tb3
$ rm -rf ./opencr_update.tar.bz2
$ wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS1/latest/opencr_update.tar.bz2 && tar -xvf opencr_update.tar.bz2 && cd ./opencr_update && ./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr && cd ..
```

ファームウェアのアップロードが完了すると、`jump_to_fw`というテキスト文字列が端末に出力されます。

### [Arduino IDE](#arduino-ide)
**[リモートPC]**
- 手順を実行する前に、ArduinoIDEをセットアップしてください。

- [OpenCRを使用するためのArduinoIDE](/docs/en/parts/controller/opencr10/#arduino-ide)

- OpenMANIPULATOR　with TurtleBot3のOpenCRファームウェア（またはソース）は、ROSでDYNAMIXELとセンサーを制御するためのものです。 ファームウェアは、ボードマネージャーによってダウンロードされるOpenCRの例にあります。

- `ファイル` → `例` → `TurtleBot3` → `turtlebot3_with_open_manipulator` → `turtlebot3_with_open_manipulator_core`を選択します。

![](/assets/images/platform/turtlebot3/manipulation/upload_core.png)

- `アップロード`ボタンを選択すると、ファームウェアをOpenCRへアップロードします。

![](/assets/images/platform/turtlebot3/manipulation/upload_core_1.png)

**注釈**: ファームウェアのアップロード中にエラーが発生した場合は、`ツール`→`ポート`に移動し、正しいポートが選択されているかどうかを確認してください。 OpenCRの`リセット`ボタンを押して、ファームウェアのアップロードを再試行してください。
{: .notice--info}

**ヒント**: DYNAMIXELのIDは、turtlebot3_with_open_manipulatorフォルダーの[`open_manipulator_driver.h`][manipulator_id]で変更できます。
{: .notice--success}

- ファームウェアのアップロードが完了すると、`jump_to_fw`というテキスト文字列が端末に出力されます。

## [Bringup](#bringup)

**注釈**: [turtlebot3_core.launch][turtlebot3_core]のOpenCRのUSBポート名を再確認してください
{: .notice--info}

**ヒント**: このコマンドを実行する前に、TurtleBot3のモデル名を指定する必要があります。 `${TB3_MODEL}`は、 `waffle`、`waffle_pi`で使用しているモデルの名前です。 エクスポート設定を永続的に設定する場合は、[Export TURTLEBOT3_MODEL][export_turtlebot3_model]を参照してください。{: .popup} page.
{: .notice--success}

**[TurtleBot3]** rosserialとlidarノードを起動します。
```bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ ROS_NAMESPACE=om_with_tb3 roslaunch turtlebot3_bringup turtlebot3_robot.launch multi_robot_name:=om_with_tb3 set_lidar_frame_id:=om_with_tb3/base_scan
```

**[TurtleBot3]** rpicameraノードを起動します。
```bash
$ ROS_NAMESPACE=om_with_tb3 roslaunch turtlebot3_bringup turtlebot3_rpicamera.launch
```

**[Remote PC]** robot_state_publisherノードを起動します。
```bash
$ ROS_NAMESPACE=om_with_tb3 roslaunch open_manipulator_with_tb3_tools om_with_tb3_remote.launch
```

## [SLAM](#slam)

**[Remote PC]** slamノードを起動します。
```bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch open_manipulator_with_tb3_tools slam.launch use_platform:=true
```

**[Remote PC]** teleopノードを起動します。
```bash
$ ROS_NAMESPACE=om_with_tb3 roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

**[Remote PC]** map_saverノードを起動します。
```bash
$ ROS_NAMESPACE=om_with_tb3 rosrun map_server map_saver -f ~/map
```

![](/assets/images/platform/turtlebot3/manipulation/open_manipulator_slam.png)

## [ナビゲーション](#ナビゲーション)

```bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch open_manipulator_with_tb3_tools navigation.launch use_platform:=true
```

![](/assets/images/platform/turtlebot3/manipulation/open_manipulator_navigation.png)

## [MoveIt!](#moveit)

- [MoveIt!](https://moveit.ros.org/)を起動するためには、新しターミナルを開き、以下のコマンドを入力します。

  ```bash
  $ export TURTLEBOT3_MODEL=${TB3_MODEL}
  $ roslaunch open_manipulator_with_tb3_tools manipulation.launch use_platform:=true
  ```

  ![](/assets/images/platform/turtlebot3/manipulation/open_manipulator_moveit_sim_1.jpg)

  ![](/assets/images/platform/turtlebot3/manipulation/open_manipulator_moveit_sim_2.jpg)

- 以下のrqtプラグインは、OpenMANIPULATORをコントロールする例を示しています。

  ![](/assets/images/platform/turtlebot3/manipulation/joint_position_service.png)

  ![](/assets/images/platform/turtlebot3/manipulation/kinematic_position_service.png)

- rosserviceメッセージでロボットの状態を取得します。

```bash
$ rosservice call /arm/moveit/get_joint_position "planning_group: 'arm'" 
joint_position: 
  joint_name: [joint1, joint2, joint3, joint4]
  position: [-0.003067961661145091, -0.42644667625427246, 1.3084856271743774, -0.8452234268188477]
  max_accelerations_scaling_factor: 0.0
  max_velocity_scaling_factor: 0.0
```

```bash
$ rosservice call /arm/moveit/get_kinematics_pose "planning_group: 'arm'
end_effector_name: ''" 
header: 
  seq: 0
  stamp: 
    secs: 1550714737
    nsecs: 317547871
  frame_id: "/base_footprint"
kinematics_pose: 
  pose: 
    position: 
      x: 0.0918695085861
      y: -0.000263644738325
      z: 0.218597669468
    orientation: 
      x: 1.82347658316e-05
      y: 0.023774433021
      z: -0.000766773548775
      w: 0.999717054001
  max_accelerations_scaling_factor: 0.0
  max_velocity_scaling_factor: 0.0
  tolerance: 0.0
```

- OpenMANIPULATORのグリッパー(**範囲は、-0.01~0.01**)を制御するには、以下のサービスコマンドが役立ちます。

```bash
$ rosservice call /om_with_tb3/gripper "planning_group: ''
joint_position:
  joint_name:
  - ''
  position:
  - 0.15
  max_accelerations_scaling_factor: 0.0
  max_velocity_scaling_factor: 0.0
path_time: 0.0"
```

![](/assets/images/platform/turtlebot3/manipulation/open_manipulator_gripper.png)


## [ピックアンドプレース](#ピックアンドプレース)

モバイルマニピュレーションのピックアンドプレースの例を提供します。この例では[smach][smach](タスクレベルのアーキテクチャ)を使用してロボットにアクションを送信します。

### gazeboシミュレータの起動

```bash
$ roslaunch open_manipulator_with_tb3_gazebo rooms.launch use_platform:=false
```

### navigation、moveIt!の起動

```bash
$ roslaunch open_manipulator_with_tb3_tools rooms.launch use_platform:=false
```

### タスクコントローラーの起動

```bash
$ roslaunch open_manipulator_with_tb3_tools task_controller.launch 
```
**ヒント**: Smachは状態グラフを提供します。 スマックビューアを実行して、ロボットがピックアンドプレースする方法を試してください。`rosrun smach_viewer smach_viewer.py`
{: .notice--success}

![](/assets/images/platform/turtlebot3/manipulation/open_manipulator_pick.png)

![](/assets/images/platform/turtlebot3/manipulation/open_manipulator_place.png)

## [シミュレーション](#simulation)

- GazeboシミュレータにTurtleBot3 with OpenMANIPULATORを読み込み、`play`ボタンを選択してください。

```bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch open_manipulator_with_tb3_gazebo empty_world.launch
```

![](/assets/images/platform/turtlebot3/manipulation/open_manipulator_gazebo_1.png)

- `rostopic list`と入力して、アクティブ化されているトピックを確認します。

``` bash
$ rostopic list
```

``` bash
/clock
/gazebo/link_states
/gazebo/model_states
/gazebo/parameter_descriptions
/gazebo/parameter_updates
/gazebo/set_link_state
/gazebo/set_model_state
/gazebo_gui/parameter_descriptions
/gazebo_gui/parameter_updates
/gazebo_ros_control/pid_gains/gripper/parameter_descriptions
/gazebo_ros_control/pid_gains/gripper/parameter_updates
/gazebo_ros_control/pid_gains/gripper/state
/gazebo_ros_control/pid_gains/gripper_sub/parameter_descriptions
/gazebo_ros_control/pid_gains/gripper_sub/parameter_updates
/gazebo_ros_control/pid_gains/gripper_sub/state
/gazebo_ros_control/pid_gains/joint1/parameter_descriptions
/gazebo_ros_control/pid_gains/joint1/parameter_updates
/gazebo_ros_control/pid_gains/joint1/state
/gazebo_ros_control/pid_gains/joint2/parameter_descriptions
/gazebo_ros_control/pid_gains/joint2/parameter_updates
/gazebo_ros_control/pid_gains/joint2/state
/gazebo_ros_control/pid_gains/joint3/parameter_descriptions
/gazebo_ros_control/pid_gains/joint3/parameter_updates
/gazebo_ros_control/pid_gains/joint3/state
/gazebo_ros_control/pid_gains/joint4/parameter_descriptions
/gazebo_ros_control/pid_gains/joint4/parameter_updates
/gazebo_ros_control/pid_gains/joint4/state
/om_with_tb3/camera/parameter_descriptions
/om_with_tb3/camera/parameter_updates
/om_with_tb3/camera/rgb/camera_info
/om_with_tb3/camera/rgb/image_raw
/om_with_tb3/camera/rgb/image_raw/compressed
/om_with_tb3/camera/rgb/image_raw/compressed/parameter_descriptions
/om_with_tb3/camera/rgb/image_raw/compressed/parameter_updates
/om_with_tb3/camera/rgb/image_raw/compressedDepth
/om_with_tb3/camera/rgb/image_raw/compressedDepth/parameter_descriptions
/om_with_tb3/camera/rgb/image_raw/compressedDepth/parameter_updates
/om_with_tb3/camera/rgb/image_raw/theora
/om_with_tb3/camera/rgb/image_raw/theora/parameter_descriptions
/om_with_tb3/camera/rgb/image_raw/theora/parameter_updates
/om_with_tb3/cmd_vel
/om_with_tb3/gripper_position/command
/om_with_tb3/gripper_sub_position/command
/om_with_tb3/imu
/om_with_tb3/joint1_position/command
/om_with_tb3/joint2_position/command
/om_with_tb3/joint3_position/command
/om_with_tb3/joint4_position/command
/om_with_tb3/joint_states
/om_with_tb3/odom
/om_with_tb3/scan
/rosout
/rosout_agg
/tf
/tf_static
```

- GazeboのOpenMANIPULATORは、ROSメッセージによって制御されます。 たとえば、以下のコマンドを使用するには、ジョイントポジション（ラジアン）を発行します。

```bash
$ rostopic pub /om_with_tb3/joint4_position/command std_msgs/Float64 "data: -0.21" --once
```

[export_turtlebot3_model]: /docs/en/platform/turtlebot3/export_turtlebot3_model
[manipulator_id]: https://github.com/ROBOTIS-GIT/OpenCR/blob/ef4e71be84dd899b03e359703be93c5000c5954a/arduino/opencr_arduino/opencr/libraries/turtlebot3/examples/turtlebot3_with_open_manipulator/turtlebot3_with_open_manipulator_core/open_manipulator_driver.h#L27
[turtlebot3_core]: https://github.com/ROBOTIS-GIT/turtlebot3/blob/467c76bc4fa2e34162f57107388839d82d3bcc0e/turtlebot3_bringup/launch/turtlebot3_core.launch#L5
[smach]: http://wiki.ros.org/smach

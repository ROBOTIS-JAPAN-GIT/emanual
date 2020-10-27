---
layout: archive
lang: jp
ref: simulation
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/simulation/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 22
---

<div style="counter-reset: h1 10"></div>

# [[ROS 1] シミュレーション](#ros-1-シミュレーション)

{% capture notice_01 %}
**注釈**:
- この手順は、`Ubuntu16.04`と`ROSKineticKame`でテスト済みです
- この手順は、リモートPCを想定しています。**リモートPC**で以下の手順を実行してください。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

{% capture notice_02 %}
{% include en/platform/turtlebot3/ros_book_info.md %}
{% endcapture %}
<div class="notice--success">{{ notice_02 | markdownify }}</div>

TurtleBot3は、シミュレーションで仮想ロボットを使用してプログラムおよび開発できる開発環境をサポートします。 これを行うための2つの開発環境があります。1つはフェイクノードと3D視覚化ツールRVizを使用し、もう1つは3DロボットシミュレーターGazeboを使用します。

フェイクノード方式は、ロボットのモデルや動きのテストには適していますが、センサーは使用できません。 SLAMとナビゲーションをテストする必要がある場合は、シミュレーションでIMU、LDS、カメラなどのセンサを使用できるGazeboを使用することを推奨します。

## [フェイクノードを使用したTurtleBot3シミュレーション](#フェイクノードを使用したTurtleBot3シミュレーション)

<iframe width="640" height="360" src="https://www.youtube.com/embed/iHXZSLBJHMg" frameborder="0" allowfullscreen></iframe>

e-Manualの内容は、予告なしに更新される場合があります。 そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

`turtlebot3_fake_node`を使用するには、`turtlebot3_simulation`メタパッケージが必要です。次のコマンドdで示すようにパッケージをインストールします。

**ヒント**：ターミナルアプリケーションは、画面左上のUbuntuの検索アイコンで見つけることができます。ターミナルのショートカットキーは `Ctrl`-`Alt`-`T` です。
{: .notice--success}

**注釈**： `turtlebot3_simulation`メタパッケージには、前提条件として`turtlebot3`メタパッケージと `turtlebot3_msgs`パッケージが必要です。 [PCセットアップのROS1依存パッケージのインストール][PCセットアップ]セクションにインストールしなかった場合は、最初にインストールしてください。
{: .notice--info}

``` bash
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
$ cd ~/catkin_ws && catkin_make
```

仮想ロボットを起動するには、以下に示すように、 `turtlebot3_fake`パッケージ内の` turtlebot3_fake.launch`ファイルを実行します。 `turtlebot3_fake`は、実際のロボットがなくても実行できる非常に単純なシミュレーションノードです。遠隔操作ノードを使用して、RVizの仮想TurtleBot3を制御することもできます。

**ヒント**：このコマンドを実行する前に、TurtleBot3のモデル名を指定する必要があります。`${TB3_MODEL}`は、`burger`、`waffle`、`waffle_pi`で使用しているモデルの名前です。エクスポート設定を永続的に設定する場合は、[Export TURTLEBOT3_MODEL][export_turtlebot3_model]{: .popup}を参照してください。
{: .notice--success}

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_fake turtlebot3_fake.launch
```

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

## [Gazeboを使用したTurtleBot3シミュレーション](#Gazeboを使用したTurtleBot3シミュレーション)

<iframe width="560" height="315" src="https://www.youtube.com/embed/UzOoJ6a_mOg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

e-Manualの内容は、予告なしに更新される場合があります。 そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

Gazeboを使用してシミュレートする方法は2つあります。 1つ目の方法は`turtlebot3_gazebo`パッケージを介してROSで使用する方法であり、2つ目の方法はROSを使用せずにgazeboと` turtlebot3_gazebo_plugin`プラグインのみを使用する方法です。

1つめの方法を使用する場合は、[以下の手順](#Gazebo用のROSパッケージ)を参照してください。2つめの方法の場合は、[次の手順](#standalone-gazebo-plugin))を参照してください。

### [Gazebo用のROSパッケージ](#Gazebo用のROSパッケージ)

**注釈**：`リモートPC`で初めてGazeboを実行する場合は、通常より少し起動に時間がかかります。
{: .notice--info}

#### [様々なワールドでシミュレーション](#様々なワールドでシミュレーション)

##### 1) エンプティーワールド

次のコマンドを使用して、gazeboのデフォルト環境の`empty world`で仮想TurtleBot3をテストできます。

**ヒント**：このコマンドを実行する前に、TurtleBot3のモデル名を指定する必要があります。`${TB3_MODEL}`は、`burger`、`waffle`、`waffle_pi`で使用しているモデルの名前です。エクスポート設定を永続的に設定する場合は、[Export TURTLEBOT3_MODEL][export_turtlebot3_model]{: .popup}を参照してください。
{: .notice--success}

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch
```

![](/assets/images/platform/turtlebot3/simulation/turtlebot3_empty_world.png)

##### 2) TurtleBot3ワールド

`TurtleBot3 world`は、TurtleBot3のシンボルの形の単純なオブジェクトで構成されるマップです。 TurtleBotワールドは、主にSLAMやNavigationなどのテストに使用されます。
 
``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

![](/assets/images/platform/turtlebot3/simulation/turtlebot3_world_bugger.png)

![](/assets/images/platform/turtlebot3/simulation/turtlebot3_world_waffle.png)

##### 3) TurtleBot3ハウス

`TurtleBot3 House`は住居の図面で作った地図です。 より複雑なタスクパフォーマンス関連のテストに適しています。

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_gazebo turtlebot3_house.launch
```

**注釈**：`TurtleBot3 House`を初めて実行する場合、ダウンロード速度にもよりますがマップファイルのダウンロードには数分以上かかります。
{: .notice}

![](/assets/images/platform/turtlebot3/simulation/turtlebot3_house.png)

![](/assets/images/platform/turtlebot3/simulation/turtlebot3_house1.png)

#### [TurtleBot3の駆動](#TurtleBot3の駆動)

##### 1) Gazeboでの遠隔操作

キーボードでTurtleBot3を制御するには、新しいターミナルウィンドウで以下のコマンドを使用して遠隔操作機能を起動してください。

``` bash
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

##### 2) 衝突回避

In order to autonomously drive a TurtleBot3 around the **TurtleBot3 world**, open a new terminal window and enter below command.
**TurtleBot3ワールド**の周りでTurtleBot3を自律駆動するには、新しいターミナルウィンドウを開き、以下のコマンドを入力します。

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

Open a new terminal window and enter below command.
新しいターミナルウィンドウを開き、以下のコマンドを入力します。

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_gazebo turtlebot3_simulation.launch
```

#### [RVizの実行](#rvizの実行)

Rvizは、シミュレーションの実行中に公開されたトピックを可視化します。以下のコマンドを入力し、新しいターミナルウィンドウでRvizを起動できます。

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_gazebo turtlebot3_gazebo_rviz.launch
```

![](/assets/images/platform/turtlebot3/simulation/turtlebot3_gazebo_rviz.png)

#### [TurtleBot3を使用した仮想SLAM](#turtlebot3を使用した仮想slam)

Gazeboの仮想SLAMの場合、実際のロボットを実行する代わりに上記のさまざまな環境とロボットモデルを選択でき、SLAM関連のコマンドは[SLAM][slam]の章で使用されるROSパッケージを使用します。

##### Virtual SLAM Execution Procedure
##### 仮想SLAM実行手順

次のコマンドは、TurtleBot3 Waffle Piモデルとturtlebot3_world環境の使用例です。

- Gazeboの起動
``` bash
$ export TURTLEBOT3_MODEL=waffle_pi
$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

- SLAMの起動
``` bash
$ export TURTLEBOT3_MODEL=waffle_pi
$ roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping
```

- TurtleBot3のリモートコントロール
``` bash
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

- マップの保存
``` bash
$ rosrun map_server map_saver -f ~/map
```

依存パッケージを実行してロボットを仮想空間で移動し、以下のようにマップを作成すると次の図に示すようにマップを作成できます。

![](/assets/images/platform/turtlebot3/simulation/virtual_slam.png)

![](/assets/images/platform/turtlebot3/simulation/map.png)

#### [TurtleBot3を使用した仮想ナビゲーション](#turtlebot3を使用した仮想ナビゲーション)

Gazeboの仮想ナビゲーションでは、実際のロボットを実行する代わりに上記のさまざまな環境とロボットモデルを選択できます。ナビゲーション関連のコマンドは、[ナビゲーション][ナビゲーション]の章で使用されるROSパッケージを使用します。

##### 仮想ナビゲーション実行手順

仮想SLAMの練習中に実行されたすべてのアプリケーションを終了し、実行します
次の手順の関連パッケージでは、ロボットは事前に生成されたマップに表示されます。 地図上でロボットの初期位置を設定した後、下図のようにナビゲーションを実行する目的地を設定します。 一度だけ初期位置を設定する必要があります。

- Gazeboの実行
``` bash
$ export TURTLEBOT3_MODEL=waffle_pi
$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

- ナビゲーションの実行
``` bash
$ export TURTLEBOT3_MODEL=waffle_pi
$ roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/map.yaml
```

![](/assets/images/platform/turtlebot3/simulation/virtual_navigation.png)

#### [複数TurtleBot3の仮想SLAM](複数turtlebot3の仮想slam)

##### 1) TurtleBot3 ハウスで３つのTurtleBot3を呼び出す

``` bash
$ roslaunch turtlebot3_gazebo multi_turtlebot3.launch
```

これらの読み込まれたTurtleBot3は、初期位置と方向が設定されています。

##### 2) SLAMの実行

``` bash
$ ROS_NAMESPACE=tb3_0 roslaunch turtlebot3_slam turtlebot3_gmapping.launch set_base_frame:=tb3_0/base_footprint set_odom_frame:=tb3_0/odom set_map_frame:=tb3_0/map
$ ROS_NAMESPACE=tb3_1 roslaunch turtlebot3_slam turtlebot3_gmapping.launch set_base_frame:=tb3_1/base_footprint set_odom_frame:=tb3_1/odom set_map_frame:=tb3_1/map
$ ROS_NAMESPACE=tb3_2 roslaunch turtlebot3_slam turtlebot3_gmapping.launch set_base_frame:=tb3_2/base_footprint set_odom_frame:=tb3_2/odom set_map_frame:=tb3_2/map

```

##### 3) 各TurtleBot3のマップデータからのマップデータのマージ

このノードを起動する前に、TurtleBot3の位置と方向についての引数を確認してください

``` bash
$ sudo apt-get install ros-kinetic-multirobot-map-merge
$ roslaunch turtlebot3_gazebo multi_map_merge.launch
```

##### 4) Rvizの実行

``` bash
$ rosrun rviz rviz -d `rospack find turtlebot3_gazebo`/rviz/multi_turtlebot3_slam.rviz
```

##### 5) 遠隔操作

``` bash
$ ROS_NAMESPACE=tb3_0 rosrun turtlebot3_teleop turtlebot3_teleop_key
$ ROS_NAMESPACE=tb3_1 rosrun turtlebot3_teleop turtlebot3_teleop_key
$ ROS_NAMESPACE=tb3_2 rosrun turtlebot3_teleop turtlebot3_teleop_key
```

##### 6) Mapの保存

``` bash
$ rosrun map_server map_saver -f ~/map
```

![](/assets/images/platform/turtlebot3/simulation/turtlebot3_house_slam.png)

![](/assets/images/platform/turtlebot3/simulation/turtlebot3_house_slam1.png)

#### [GazeboでのTurtleBot3 AutoRace ](#gazeboでのturtlebot3-autorace)
[AutoRace with Gazebo](/docs/en/platform/turtlebot3/autonomous_driving/#autorace-with-gazebo)を確認してください。

#### [Turtlebot3 with OpenMANIPULATOR](#turtlebot3-with-openmanipulator)
[OpenMANIPULATOR with Gazebo](/docs/en/platform/turtlebot3/manipulation/#simulation)を確認してください。

### [スタンドアローンGazeboプラグイン](#スタンドアローンgazeboプラグイン)

**注釈**：このチュートリアルは、 `ROS`を使用せずにTurtleBot3をシミュレートしたいユーザーのみを対象としています。 しかしながら、`ROS`を使用してロボットをシミュレートすることを強く推奨します。
{: .notice--info}

#### [Gazeboプラグインの使用方法](#gazeboプラグインの使用方法)

##### 1) Gazebo7ライブラリのインストール

``` bash
$ sudo apt-get install libgazebo7-dev
```

##### 2) Githubからソースコードをダウンロードする

``` bash
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_gazebo_plugin
```

##### 3) `.bashrc`ファイルへGazeboプラグインパスの追加

``` bash
$ nano ~/.bashrc
```

**ヒント**: turtlebot3_gazebo_plugin path = ~/turtlebot3_gazebo_plugin
{: .notice--info}

``` bash
export GAZEBO_PLUGIN_PATH=$GAZEBO_PLUGIN_PATH:${turtlebot3_gazebo_plugin path}/build
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:${turtlebot3_gazebo_plugin path}/models
```

##### 4) メイクとビルド

``` bash
$ cd ${turtlebot3_gazebo_plugin path}
$ mkdir build
$ cd build
$ cmake ..
$ make
```

##### 5) プラグインの実行

**TIP**: Before executing this command, you have to specify the model name of TurtleBot3. The `${TB3_MODEL}` is the name of the model you are using in `burger`, `waffle`, `waffle_pi`. If you want to permanently set the export settings, please refer to [Export TURTLEBOT3_MODEL][export_turtlebot3_model]{: .popup} page.
{: .notice--success}

``` bash
$ cd ${turtlebot3_gazebo_plugin}
$ gazebo worlds/turtlebot3_${TB3_MODEL}.world
```
![](/assets/images/platform/turtlebot3/simulation/gazebo_waffle_pi.png)

![](/assets/images/platform/turtlebot3/simulation/gazebo_burger.png)

##### 6) キーボードでの遠隔操作

```
w - set linear velocity up
x - set linear velocity down
d - set angular velocity up
a - set angular velocity down
s - set all velocity to zero
```

##### 7) トピックのサブスクライブコマンド

- 全てのトピックを表示

``` bash
$ gz topic -l
```

- スキャンデータのサブスクライブ

``` bash
$ gz topic -e /gazebo/default/user/turtlebot3_${TB3_MODEL}/lidar/hls_lfcd_lds/scan
```

- 画像データのサブスクライブ

**Waffle**

``` bash
$ gz topic -e /gazebo/default/user/turtlebot3_waffle/image/intel_realsense_r200/image
```

**Waffle Pi**

``` bash
$ gz topic -e /gazebo/default/user/turtlebot3_waffle_pi/image/raspberry_pi_cam/image
```

##### 8) リスナーの実行

``` bash
$ cd ${turtlebot3_gazebo_plugin}/build
$ ./lidar_listener ${TB3_MODEL}
```

新しいターミナルを開き、以下のコマンドを入力します。
``` bash
$ cd ${turtlebot3_gazebo_plugin}/build
$ ./image_listener ${TB3_MODEL}
```

##### 参考

  - [Gazebo API](http://osrf-distributions.s3.amazonaws.com/gazebo/api/dev/index.html)
  - [How to contribute model](http://gazebosim.org/tutorials?tut=model_contrib&cat=build_robot)
  - [How to make model](http://gazebosim.org/tutorials?tut=build_model&cat=build_robot)
  - [Tutorial for making Hello World plugin](http://gazebosim.org/tutorials?tut=plugins_hello_world&cat=write_plugin)
  - [Tutorial for making model plugin](http://gazebosim.org/tutorials?cat=guided_i&tut=guided_i5)
  - [Tutorial for making sensor plugin](http://gazebosim.org/tutorials?tut=contact_sensor)
  - [Tutorial for topic subscription](http://gazebosim.org/tutorials?tut=topics_subscribed)

[pc_setup]: /docs/jp/platform/turtlebot3/pc_setup/#ROS1依存パッケージのインストール
[export_turtlebot3_model]: /docs/en/platform/turtlebot3/export_turtlebot3_model

[slam]: /docs/en/platform/turtlebot3/slam/#slam
[navigation]: /docs/en/platform/turtlebot3/navigation/#navigation

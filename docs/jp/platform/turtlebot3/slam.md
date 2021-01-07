---
layout: archive
lang: jp
ref: slam
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/slam/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 20
---

<div style="counter-reset: h1 8"></div>

# [[ROS 1] SLAM](#ros-1-slam)

**警告**: テーブルの上でロボットを動かすときは、ロボットが落下する可能性があるので注意してください。
{: .notice--warning}

{% capture notice_01 %}
**注釈**:
- このコマンドは `Ubuntu 16.04` と `ROS Kinetic Kame` でテストをしました。
- この例は、リモートPCでの動作を想定しています。**リモートPC** の指示にしたがってください。
- ターミナルアプリケーションは、画面左上のUbuntuの検索アイコンで見つけることができます。ターミナルのショートカットキーは `Ctrl`-`Alt`-`T` です。
- コマンドを使用する前に、必ず[Bringup](/docs/jp/platform/turtlebot3/bringup/#bringup)の指示を実行してください。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

{% capture notice_02 %}
{% include jp/platform/turtlebot3/ros_book_info.md %}
{% endcapture %}
<div class="notice--success">{{ notice_02 | markdownify }}</div>

**ヒント**: 簡単に制御をするために、キーボードの代わりにジョイパッドを使用することを推奨します。 リモコンの詳細については、[遠隔操作][teleoperation]ページを参照してください。
{: .notice--success}

**SLAM（Simultaneous Localization and Mapping）**は、任意の空間の現在位置を推定して地図を描く手法です。 SLAMは、TurtleBotの前身からよく知られている機能です。 ここのビデオでは、TurtleBot3がコンパクトかつ手頃なプラットフォームで地図をどれだけ正確に描くことができるかを示しています。

<iframe width="640" height="360" src="https://www.youtube.com/embed/lkW4-dG2BCY" frameborder="0" allowfullscreen></iframe>
e-Manualの内容は、予告なしに更新される場合があります。 そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

* Date: 2016.11.29
* Robot: TurtleBot3 Burger
* Sensor: Laser Distance Sensor
* Packages: Gmapping / Cartographer
* Place: ROBOTIS Labs & HQ, 15th-floor corridor
* Duration: 55 minutes
* Distance: Total 351 meters

<iframe width="640" height="360" src="https://www.youtube.com/embed/7mEKrT_cKWI" frameborder="0" allowfullscreen></iframe>
e-Manualの内容は、予告なしに更新される場合があります。 そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

* Date: 2017.04.20
* Robot: TurtleBot3 Burger and Waffle
* Sensor: 360 Laser Distance Sensor LDS-01
* Packages: Gmapping
* Place: ROBOTIS HQ Education Room
* Duration: About 4 minutes
* Distance: Total 15 meters

## [SLAMノードの実行](#slamノードの実行)

**[リモートPC]** roscoreを実行します。

``` bash
$ roscore
```

**[TurtleBot]** 基本パッケージを起動し、TurtleBot3のアプリケーションをスタートします。

``` bash
$ roslaunch turtlebot3_bringup turtlebot3_robot.launch
```

**[リモートPC]** 新しいターミナルを開き、SLAMファイルを起動します。

**ヒント**: このコマンドを実行する前に、TurtleBot3のモデル名を指定する必要があります。 `$ {TB3_MODEL}`は、 `burger`、` waffle`、 `waffle_pi`で使用しているモデルの名前です。 エクスポート設定を恒久的に設定する場合は、[Export TURTLEBOT3_MODEL][export_turtlebot3_model]を参照してください。{: .popup} page.
{: .notice--success}

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping
```

{% capture slam_tip %}
**ヒント**: 上記のコマンドを実行すると、視覚化ツールRVizも実行されます。 RVizを個別に実行する場合は、次のいずれかのコマンドを使用します。

  - $ rviz -d \`rospack find turtlebot3_slam\`/rviz/turtlebot3_gmapping.rviz
  - $ rviz -d \`rospack find turtlebot3_slam\`/rviz/turtlebot3_cartographer.rviz
  - $ rviz -d \`rospack find turtlebot3_slam\`/rviz/turtlebot3_hector.rviz
  - $ rviz -d \`rospack find turtlebot3_slam\`/rviz/turtlebot3_karto.rviz
  - $ rviz -d \`rospack find turtlebot3_slam\`/rviz/turtlebot3_frontier_exploration.rviz

{% endcapture %}

<div class="notice--info">{{ slam_tip | markdownify }}</div>

{% capture notice_03 %}
**注釈**: さまざまなSLAMメソッドをサポートしています
- TurtleBot3は、さまざまなSLAMメソッドの中で、Gmapping、Cartographer、Hector、およびKartoをサポートしています。 これを行うには、 `slam_methods：= xxxxx`オプションを変更します。
- `slam_methods`オプションには` gmapping`、 `cartographer`、` hector`、 `karto`、` frontier_exploration`が含まれ、それらの1つを選択できます。
- たとえば、kartoを使用するには、次のようにします:
- $ roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=karto
{% endcapture %}
<div class="notice--info">{{ notice_03 | markdownify }}</div>

{% capture notice_04 %}
**注釈**: SLAMパッケージの依存関係パッケージをインストールします
- `Gmapping`の場合:
- Gmappingに関連するパッケージは、[PC セットアップ](/docs/jp/platform/turtlebot3/pc_setup/#install-dependent-ros-packages)ページですでにインストールされています。
- `Cartographer`の場合:
- sudo apt-get install ros-kinetic-cartographer ros-kinetic-cartographer-ros ros-kinetic-cartographer-ros-msgs ros-kinetic-cartographer-rviz
- `Hector Mapping`の場合:
- sudo apt-get install ros-kinetic-hector-mapping
- `Karto`の場合:
- sudo apt-get install ros-kinetic-slam-karto
- `Frontier Exploration`の場合:
- Frontier Explorationはgmappingを使用しており、次のパッケージをインストールする必要があります。
- sudo apt-get install ros-kinetic-frontier-exploration ros-kinetic-navigation-stage
{% endcapture %}
<div class="notice--info">{{ notice_04 | markdownify }}</div>


**ヒント**: cartographerは0.3.0バージョンでテストしました。 Googleが開発したCartographerパッケージは、ROS Melodicでは0.3.0バージョンをサポートしていますが、ROS Kineticでは0.2.0バージョンをサポートしています。 したがって、ROS Kineticで作業する必要がある場合は、バイナリファイルをダウンロードする代わりに、次のようにソースコードをダウンロードしてビルドする必要があります。 インストール手順の詳細については、[公式wikiページ](https://google-cartographer-ros.readthedocs.io/en/latest/#building-installation)を参照してください。
{: .notice--success}

```sh
$ sudo apt-get install ninja-build libceres-dev libprotobuf-dev protobuf-compiler libprotoc-dev
$ cd ~/catkin_ws/src
$ git clone https://github.com/googlecartographer/cartographer.git
$ git clone https://github.com/googlecartographer/cartographer_ros.git
$ cd ~/catkin_ws
$ src/cartographer/scripts/install_proto3.sh
$ rm -rf protobuf/
$ rosdep install --from-paths src --ignore-src -r -y --os=ubuntu:xenial
$ catkin_make_isolated --install --use-ninja
```

```sh
$ source ~/catkin_ws/install_isolated/setup.bash
$ roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=cartographer
```

## [遠隔操作ノードの実行](#遠隔操作ノードの実行)


**[リモートPC]** 新しい端末を開き、遠隔操作ノードを実行します。 次のコマンドを使用すると、ユーザーはロボットを制御してSLAM操作を手動で実行できます。 速度の変更が速すぎたり、回転が速すぎたりするなどの激しい動きを避けることが重要です。 ロボットを使用して地図を作成する場合、ロボットは測定対象の環境の隅々までスキャンする必要があります。 きれいな地図を作成するにはある程度の経験が必要なので、SLAMを複数回練習してノウハウを作成しましょう。 マッピングプロセスを次の図に示します。

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

``` bash
  Control Your TurtleBot3!
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

![](/assets/images/platform/turtlebot3/slam/slam_running_for_mapping.png)

## [チューニングガイド](#チューニングガイド)

Gmappingには、さまざまな環境のパフォーマンスを変更するための多くのパラメーターがあります。 パラメーター全体に関する情報は、[ROS WiKi](http://wiki.ros.org/gmapping)で入手するか、[ROS Robot Programming](https://community.robotsource.org/t/download-the-ros-robot-programming-book-for-free/51)の第11章を参照してください。

このチューニングガイドでは、重要なパラメーターを設定するためのヒントをいくつか紹介します。 環境に応じてパフォーマンスを変更したい場合は、このヒントが役立つ可能性があり、時間を節約できます。

_**maxUrange**_ 
- `turtlebot3_slam/launch/turtlebot3_gmapping.launch`
- このパラメーターは、LIDARセンサーの最大使用可能範囲を設定します。

_**map_update_interval**_
- `turtlebot3_slam/launch/turtlebot3_gmapping.launch`
- マップの更新間の時間（秒単位）。 これを低く設定すると、マップがより頻繁に更新されます。 ただし、より大きな計算負荷が必要になります。 このパラメーターの設定は、環境によって異なります。

![](/assets/images/platform/turtlebot3/slam/tuning_map_update_interval.png)

_**minimumScore**_ 
- `turtlebot3_slam/launch/turtlebot3_gmapping.launch`
- スキャンマッチングの結果を考慮するための最小スコア。 このパラメーターにより、ポーズ推定のジャンプを回避できます。
   これが適切に設定されている場合は、以下の情報を見ることができます。

  ```
  Average Scan Matching Score=278.965
  neff= 100
  Registering Scans:Done
  update frame 6
  update ld=2.95935e-05 ad=0.000302522
  Laser Pose= -0.0320253 -5.36882e-06 -3.14142
  ```

  この設定が高すぎる場合は、以下の警告が表示されます。

  ```
  Scan Matching Failed, using odometry. Likelihood=0
  lp:-0.0306155 5.75314e-06 -3.14151
  op:-0.0306156 5.90277e-06 -3.14151
  ```

_**linearUpdate**_ 
- `turtlebot3_slam/launch/turtlebot3_gmapping.launch`
- ロボットが移動すると、毎回スキャン処理が行われます。

_**angularUpdate**_ 
- `turtlebot3_slam/launch/turtlebot3_gmapping.launch`
- ロボットが回転すると、毎回スキャン処理が行われます。 これをlinearUpdateよりも小さく設定することを推奨します。

## [地図の保存](#地図の保存)

**[リモートPC]** すべての作業が完了したので、`map_saver`ノードを実行してマップファイルを作成します。 マップは、ロボットのオドメトリ、tf情報、およびロボットが移動したときのセンサーのスキャン情報に基づいて描画されます。これらのデータは、前のサンプルビデオのRVizで見ることができます。作成されたマップは、`map_saver`が実行されているディレクトリに保存されます。ファイル名を指定しない限り、マップ情報を含む`map.pgm`および`map.yaml`ファイルとして保存されます。

``` bash
$ rosrun map_server map_saver -f ~/map
```

`-f`オプションは、マップファイルが保存されているフォルダーとファイル名を参照します。`~/map`をオプションとして使用すると、`map.pgm`と `map.yaml`がユーザーのホームフォルダー`~/`（$HOME：`/home/<username>`）のmapフォルダーに保存されます。

## [地図](#地図)

ROSコミュニティでよく使用されている2次元の `Occupancy Grid Map（OGM）`を使用します。 下の図に示すように、前の[地図の保存](#地図の保存)セクションから取得した地図。**白色**はロボットが移動可能な空き領域、**黒色**はロボットが動作できない占有領域です。**灰色**は未知の領域です。 この地図は[ナビゲーション][ナビゲーション]で使用されます。

![](/assets/images/platform/turtlebot3/slam/map.png)

次の図は、TurtleBot3を使用して大きなマップを作成した結果を示しています。 移動距離が約350メートルの地図を作成するのに約1時間かかりました。

![](/assets/images/platform/turtlebot3/slam/large_map.png)

[ナビゲーション]: /docs/jp/platform/turtlebot3/navigation/#ナビゲーション
[遠隔操作]: /docs/jp/platform/turtlebot3/teleoperation/#遠隔操作
[export_turtlebot3_model]: /docs/jp/platform/turtlebot3/export_turtlebot3_model

## [参考文献](#参考文献)

- gmapping
  - [ROS WIKI](http://wiki.ros.org/gmapping), [Github](https://github.com/ros-perception/slam_gmapping)

- cartographer 
  - [ROS WIKI](http://wiki.ros.org/cartographer), [Github](https://github.com/googlecartographer/cartographer)

- hector
  - [ROS WIKI](http://wiki.ros.org/hector_slam), [Github](https://github.com/tu-darmstadt-ros-pkg/hector_slam)

- karto
  - [ROS WIKI](http://wiki.ros.org/slam_karto), [Github](https://github.com/ros-perception/slam_karto)

- frontier_exploration 
  - [ROS WIKI](http://wiki.ros.org/frontier_exploration), [Github](https://github.com/paulbovbel/frontier_exploration)

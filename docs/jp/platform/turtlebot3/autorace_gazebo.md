---
layout: archive
lang: jp
ref: autorace_gazebo
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/autonomous_gazebo/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 26
---

<div style="counter-reset: h1 13"></div>
<div style="counter-reset: h2 7"></div>

<!--[dummy Header 1]>
  <h1 id="dummy"><a href="#dummy">Dummy</a></h1>
<![end dummy Header 1]-->

## [GazeboでAutoRace](#gazeboでAutoRace)
AutoRaceはGazeboによって提供されます。 R-BIZチャレンジのTurtleBot3 AutoRace 2017という環境を作成しました。

- 推奨スペック

| CPU     | Intel Core i5 / 2 GHz Dual Core Processor      |
| RAM     | 4GB                                            |
| Storage | 20Gb of free hard drive space                  |
| GPU     | NVIDIA GeForce GTX 9 series                    |

**警告**: 実際のカメラキャリブレーション構成ファイルとGazeboキャリブレーション構成ファイルを混同しないでください。
{: .notice--warning}

**注釈**: `turtlebot3_autorace`パッケージには、前提条件として`turtlebot3_simulations`パッケージが必要です。 [TurtleBot3 シミュレーションのインストール](＃シミュレーション)でインストールしなかった場合は、最初にインストールしてください。
{: .notice--info}

1. `Remote PC`でAutoRaceの実行を起動します。AutoRace 2017の地図がGazeboで見えます。

    ``` bash
    $ roslaunch turtlebot3_gazebo turtlebot3_autorace.launch
    ```

    ![](/assets/images/platform/turtlebot3/autonomous_driving/autorace_map.png)

2. `Remote PC`でミッションの実行を起動します。Gazeboで`Traffic Light`、`Parked TurtleBot3`、`Toll Gate`が見えます。TurtleBot3がミッションエリアに近づくと、自動的に動作します。

    ``` bash
    $ roslaunch turtlebot3_gazebo turtlebot3_autorace_mission.launch
    ```

    ![](/assets/images/platform/turtlebot3/autonomous_driving/autorace_map_mission.png)

3. `Remote PC`でAutoRaceの実行を起動します。現実世界でAutoRaceを起動したい場合は、カメラをキャリブレートする必要があります。

    ``` bash
    $ export GAZEBO_MODE=true
    $ export AUTO_IN_CALIB=action
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
    ```

4. `Remote PC`で新しい端末を開き、次のように入力します。

    ``` bash
    $ export AUTO_EX_CALIB=action
    $ export AUTO_DT_CALIB=action
    $ export TURTLEBOT3_MODEL=burger
    $ roslaunch turtlebot3_autorace_core turtlebot3_autorace_core.launch
    ```

5. `Remote PC`で新しい端末を開き、次のように入力します。

    ``` bash
    $ rostopic pub -1 /core/decided_mode std_msgs/UInt8 "data: 2"
    ```

- ビデオ : GazeboでのAutoRace

  <iframe width="640" height="360" src="https://www.youtube.com/embed/5fZmuPxMZz0" frameborder="0" allowfullscreen></iframe>

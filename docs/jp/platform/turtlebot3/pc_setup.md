---
layout: archive
lang: jp
ref: pc_setup
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/pc_setup/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 7
---

<div style="counter-reset: h1 6"></div>
<div style="counter-reset: h2 0"></div>

<!--[dummy Header 1]>
  <h1 id="pc-setup"><a href="#pc-setup">PC Setup</a></h1>
<![end dummy Header 1]-->

## [PC セットアップ](#pc-setup)

![](/assets/images/platform/turtlebot3/software/remote_pc_and_turtlebot.png)

**警告**: この章の内容は、TurtleBot3を制御する`リモートPC`(デスクトップまたは、ラップトップPC)に対応しています。 この手順は、TurtleBot3で実施しないでください。
{: .notice--warning}

**注釈**: この手順は、`Ubuntu 16.04` and `ROS Kinetic Kame`でテスト済みです。
{: .notice--info}

### [リモートPCへのUbuntuのインストール](#install-ubuntu-on-remote)

下記のリンクから`Ubuntu 16.04`をダウンロードし、`リモートPC(デスクトップまたは、ラップトップPC)`にインストールします。

- [ダウンロードリンク][ubuntu_download_link]

Ubuntuのインストールについて、さらに助けが必要な場合は下記リンクのステップバイステップガイドを確認してください。

- [デスクトップ用Ubuntuのインストール](https://www.ubuntu.com/download/desktop/install-ubuntu-desktop)

### [リモートPCへのROS1のインストール](#install-ros-1-on-remote-pc)

![](/assets/images/platform/turtlebot3/logo_ros.png)

下記のスクリプトを使用すると、ROS1のインストール手順を簡略化できます。
ターミナルウィンドウでこのスクリプトを実行します。ターミナルアプリケーションは、画面の左上隅にあるUbuntu検索アイコンから起動できます。もしくは、ターミナルのショートカットキー(`Ctrl`-`Alt`-`t`)を使用して起動できます。 ROS1をインストールした後、リモートPCを再起動してください。

``` bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic.sh && chmod 755 ./install_ros_kinetic.sh && bash ./install_ros_kinetic.sh
```

手動インストールの場合は、下記のリンクに従ってください。

- [ROS1 Kineticの手動インストール](http://wiki.ros.org/kinetic/Installation/Ubuntu)  

**注釈**: インストールされるパッケージを確認するには、このリンクを確認してください。[install_ros_kinetic.sh](https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic.sh)
{: .notice--info}

{% capture info_01 %}
**注釈**:  
 - ROBOTISのROSパッケージはMelodic Moreniaをサポートしていますが、TurtleBot3にはROS Kinetic Kameを使用することを推奨します。
 - ROSをMelodic Moreniaにアップグレードする場合は、サードパーティのROSパッケージが完全にサポートされていることを確認してください。
{% endcapture %}
<div class ="notice--info">{{info_01 | markdownify}}</div>

### [ROS1 依存パッケージのインストール](#ROS1依存パッケージのインストール)

リモートPCにTurtleBot3を制御するための依存パッケージをインストールする手順です。

``` bash
$ sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation ros-kinetic-interactive-markers
```

``` bash
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
$ cd ~/catkin_ws && catkin_make
```

`catkin_make`コマンドがエラー無しで完了した場合、TurtleBot3の準備は完了です。

### [ネットワーク構成](#network-configuration)

![](/assets/images/platform/turtlebot3/software/network_configuration.png)

ROS1では、TurtleBot PCとリモートPCの間で通信をするためにIPアドレスが必要です。 リモートPCとTurtleBot PCは、同じwifiルーターに接続する必要があります。

リモートPCのターミナルウィンドウで次のコマンドを入力し、リモートPCのIPアドレスを確認します。

``` bash
$ ifconfig
```

長方形の赤い枠で囲っている文字列が、`リモートPC`のIPアドレスです。

![](/assets/images/platform/turtlebot3/software/network_configuration2.png)

以下のコマンドを入力します。

``` bash
$ nano ~/.bashrc
```

`Alt + /`を入力するとファイルの最終行へ移動します。

`ROS_MASTER_URI`と`ROS_HOSTNAME`の`localhost`のIPアドレスを、上記のターミナルウィンドウから取得したIPアドレスに変更します。


![](/assets/images/platform/turtlebot3/software/network_configuration3.png)

次に、以下のコマンドでbashrcを実行します。

``` bash
$ source ~/.bashrc
```

[ubuntu_download_link]: https://www.ubuntu.com/download/alternative-downloads

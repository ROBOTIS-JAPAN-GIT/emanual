---
layout: archive
lang: jp
ref: ros_2
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/ros2_setup/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 29
---

<div style="counter-reset: h1 15"></div>

# [[ROS 2]セットアップ](#ros-2-setup)

{% capture notice_01 %}
**注釈**:
- `Ubuntu 18.04`および`ROS Dashing Diademata`でテスト済みです。
- これらの手順は、TurtleBotのリモートPCとSBCで実行してください。
- 手順に不明な事があった場合は、[ROS Answers](https://answers.ros.org/questions/)で質問できます。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

## PCセットアップ

### Ubuntuのインストール

リモートPCにROS（ロボットオペレーティングシステム）をセットアップするには、**リモートPC**にUbuntu 18.04をインストールします。

- [Ubuntu 18.04のダウンロード](http://releases.ubuntu.com/18.04/)
- [チュートリアル - Ubuntuデスクトップのインストール](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop#0)

### ROS 2のインストール

![](/assets/images/platform/turtlebot3/logo_ros.png)

TurtleBot3はROSで動作するので、**リモートPC**にROS 2（Dashing Diademata）の `ros-dashing-desktop` debianパッケージをインストールする必要があります。
次のリンクは、ROS 2のインストールに関するガイドです。

- [ROS 2のインストールガイド](https://index.ros.org/doc/ros2/Installation/Dashing/)

### ROS 2の依存パッケージのインストール

**[リモートPC]**

1. **Remote PC**でターミナルを開きます。
2. 下記のコマンドを入力します。
```bash
# Install Colcon
$ sudo apt install python3-colcon-common-extensions
# Install Cartographer dependencies
$ sudo apt install -y \
    google-mock \
    libceres-dev \
    liblua5.3-dev \
    libboost-dev \
    libboost-iostreams-dev \
    libprotobuf-dev \
    protobuf-compiler \
    libcairo2-dev \
    libpcl-dev \
    python3-sphinx
# Install Gazebo9
$ curl -sSL http://get.gazebosim.org | sh
$ sudo apt install ros-dashing-gazebo-*
# Install Cartographer
$ sudo apt install ros-dashing-cartographer
$ sudo apt install ros-dashing-cartographer-ros
# Install Navigation2
$ sudo apt install ros-dashing-navigation2
$ sudo apt install ros-dashing-nav2-bringup
# Install vcstool
$ sudo apt install python3-vcstool
```

### TurtleBot3パッケージのインストール

**[リモートPC]**

```bash
$ mkdir -p ~/turtlebot3_ws/src
$ cd ~/turtlebot3_ws
$ wget https://raw.githubusercontent.com/ROBOTIS-GIT/turtlebot3/ros2/turtlebot3.repos
$ vcs import src < turtlebot3.repos
$ colcon build --symlink-install
```

### セットアップ用にBashコマンドを保存する

**[リモートPC]**

```bash
$ echo 'source ~/turtlebot3_ws/install/setup.bash' >> ~/.bashrc
$ echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
$ source ~/.bashrc
```

{% capture notice_02 %}
**NOTE**:
下記のドキュメントは、ファイルの依存関係によるビルドエラーや警告を修正するのに役立ちます。
- [Cartographer](https://google-cartographer.readthedocs.io/en/latest/)
- [Cartographer_ros](https://google-cartographer-ros.readthedocs.io/en/latest/)
- [Navigation2](https://github.com/ros-planning/navigation2/blob/master/README.md)
{% endcapture %}
<div class="notice--info">{{ notice_02 | markdownify }}</div>

## [SBCのセットアップ](#sbc-setup)

**警告** : この手順をリモートPCに適用しないでください。SBCに適用します。
{: .notice--warning}

### Ubuntuイメージのダウンロードとインストール

**[TurtleBot3]**

1. [Ubuntu リリース](http://cdimage.ubuntu.com/ubuntu/releases/bionic/release)へ行きます。
2. "リモートPC"に`Raspberry Pi 3 (64-bit ARM) preinstalled server image`をダウンロードします。
3. microSDカードにubuntuイメージを書き込みます。

**ヒント** : `GNOME Disks`を使用して、microSDに`Ubuntu Server 18.04 image`を書き込むこともできます。

{: .notice--success}

### Raspberry Pi 3の初期化プロセス

**リモートPC**と**TurtleBot3**間で通信するためには、Raspberry Pi 3 に`Ubuntu Server 18.04 image`をインストールする必要があります。

**[TurtleBot3]**

1. TurtleBot3のSBCのmicroSDカードスロットにOSイメージを書き込んだmicroSDカードを挿入した後、Raspberry Pi 3を起動します。（HDMIケーブル、キーボード、マウスをTurtleBot3に接続できます）
2. デフォルトのユーザー名（`ubuntu`）とパスワード（`ubuntu`）でログインします。（ログイン後、パスワードを変更するかどうか尋ねられます）
3. [netplan](https://netplan.io/)を使用してWiFiネットワーク設定を構成できます。 [ネットワーク設定の例](＃example-for-network-configuration)を参照してください。

**注釈** ： TurtleBot3は[SLAM](/docs/en/platform/turtlebot3/ros2_slam/#slam)および[Navigation2](/docs/en/platform/turtlebot3/ros2_navigation2/#navigation)を実行するモバイルロボットです。 ワイヤレスネットワークを使用するため、TurtleBot3をWifiに接続することを推奨します。
{：.notice--info}

### ネットワーク設定の例

**[TurtleBot3]**

1. SBCでターミナルを開きます。

2. ファイルを作成し、次のコマンドを使用して開きます。
```bash
$ sudo touch /etc/netplan/01-netcfg.yaml
$ sudo nano /etc/netplan/01-netcfg.yaml
```

3. ファイルを開き、ネットワークの設定を行います。下記のネットワーク設定を参照してください。
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: yes
      dhcp6: yes
      optional: true
  wifis:
    wlan0:
      dhcp4: yes
      dhcp6: yes
      access-points:
        "your-wifi-name":
          password: "your-wifi-password"
```

4. 設定後、以下の手順でリモートPCをTurtlebot3のSBCに接続できます。

5. 全ての設定が完了したら、Raspberry Pi 3を再起動します。
```bash
$ sudo netplan apply
$ reboot、
```

6. 起動時にネットワーク接続がない場合でも起動遅延が発生しないように、`systemd`を設定します。
以下のコマンドを実行して、`systemd`のプロセスにマスクを設定するには、次のコマンドを実行します。
```bash
$ systemctl mask systemd-networkd-wait-online.service
```

7. これにより、SSHを使用できます。[リモートPCをSBCに接続する](#connect-remote-pc-to-sbc)を参照してください

#### [リモートPCをSBCに接続する](#connect-remote-pc-to-sbc)

この手順に進む前に[ネットワーク設定の例](#example-for-network-configuration)を必ずお読みください。
{: .notice--warning}
　
**[リモートPC]**

1. **リモートPC**でターミナルを開きます。
2. 下記のコマンドを入力します。
```bash
$ ssh ubuntu@<Raspberry PIのネットワークIP>
```

### スワップ領域の追加

**[TurtleBot3]**

1. **SBC**のターミナルを開きます。

2. スワップに使用するファイルを作成します。
```bash
$ sudo fallocate -l 1G /swapfile
```

3. ファイルに`読み込み`と`書き込み`の権限を付与します。
```bash
$ sudo chmod 600 /swapfile
```

4. `mkswap`を使用し、Linuxのスワップ領域を設定します。
```bash
$ sudo mkswap /swapfile
```

5. ファイルの作成後、ファイルをアクティブにして使用を開始します。
```bash
$ sudo swapon /swapfile
```

6. Open `/etc/fstab` file with `nano` command, as swap areas are listed in `/etc/fastab`.
6. スワップ領域が`/etc/fstab`にリストされているので、`nano`コマンドで`/etc/fstab'ファイルを開きます
```bash
$ sudo nano /etc/fstab
```

7. 下記の文をコピーして、開いたファイルへ貼り付けます。
```
/swapfile swap swap defaults 0 0
```

8. スワップ領域が割り当てられているかどうかを、`free`コマンドで確認します。 6つの列と3つのデータ行で構成されるテーブルが表示されます。
```bash
$ sudo free -h
                 total        used        free      shared  buff/cache   available
Mem:           912M         97M        263M        4.4M        550M        795M
Swap:          1.0G          0B        1.0G
```

## ROS Dashing Diademataをインストールする

TurtleBot3はROSで動作するために、**TurtleBot3**のSBCにROS 2(Dashing Diademata)をインストールする必要があります。下記のリンクは、ROS 2をインストールするためのガイドです。

- [ROS 2 インストールガイド](https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Install-Debians/)

**[TurtleBot3]**

1. **SBC**のターミナルを開く。

2. ソフトウェアの更新とアップグレードをします。
```bash
$ sudo apt update && sudo apt upgrade
```

3. ロケールを設定する。
```bash
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
$ export LANG=en_US.UTF-8
```

4. ソースを設定する。
```bash
$ sudo apt update && sudo apt install curl gnupg2 lsb-release
$ curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
$ sudo sh -c 'echo "deb [arch=amd64,arm64] http://packages.ros.org/ros2/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'
```

5. ROS 2のパッケージのインストール
```bash
$ sudo apt update
$ sudo apt install ros-dashing-ros-base
```

### TurtleBot3パッケージのインストール

**[TurtleBot3]**

```bash
$ sudo apt install python3-argcomplete python3-colcon-common-extensions libboost-system-dev
$ mkdir -p ~/turtlebot3_ws/src && cd ~/turtlebot3_ws/src
$ git clone -b ros2 https://github.com/ROBOTIS-GIT/hls_lfcd_lds_driver.git
$ git clone -b ros2 https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
$ git clone -b ros2 https://github.com/ROBOTIS-GIT/turtlebot3.git
$ git clone -b ros2 https://github.com/ROBOTIS-GIT/DynamixelSDK.git
$ cd ~/turtlebot3_ws/src/turtlebot3
$ rm -r turtlebot3_cartographer turtlebot3_navigation2
$ cd ~/turtlebot3_ws/
$ echo 'source /opt/ros/dashing/setup.bash' >> ~/.bashrc
$ source ~/.bashrc
$ colcon build --symlink-install --parallel-workers 1
```

### 環境設定

#### ドメインIDの割り当て
DDS通信では、同じネットワーク環境でのワイヤレス通信のために、`ROS_DOMAIN_ID`を**リモートPC**と**TurtleBot3**の間で一致させる必要があります。 次のコマンドは、TurtleBot3のSBCに`ROS_DOMAIN_ID`を割り当てる方法を示しています。
- **TurtleBot3**のデフォルトIDは `0`に設定されています。
- TurtleBot3のリモートPCとSBCの`ROS_DOMAIN_ID`を`30`に設定することを推奨します。

**[TurtleBot3]**

1. **SBC**のターミナルを開きます。
2. 次のコマンドを入力します。
```bash
$ echo 'source ~/turtlebot3_ws/install/setup.bash' >> ~/.bashrc
$ echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
$ source ~/.bashrc
```

    **警告** : あなたと他のユーザー間でIDが**重複**しないようにしてください。同じネットワーク環境下でユーザー間の通信の競合が発生します。
    {: .notice--warning}

#### OpenCRのポートセットアップ

次のコマンドは、TurtleBot3にOpenCRのポート認証を割り当てる方法です。

**[TurtleBot3]**

1. **SBC**のターミナルを開きます。
2. 次のコマンドを入力します。
```bash
$ cd ~/turtlebot3_ws/src/turtlebot3/turtlebot3_bringup 
$ sudo cp ./99-turtlebot3-cdc.rules /etc/udev/rules.d/ 
$ sudo udevadm control --reload-rules 
$ sudo udevadm trigger
```

### [参考](#references)

[https://wiki.ubuntu.com/ARM/RaspberryPi](https://wiki.ubuntu.com/ARM/RaspberryPi)  
[https://discourse.ros.org/t/setting-up-the-turtlebot3-with-ros2-on-ubuntu-server-iot-18-04/10090](https://discourse.ros.org/t/setting-up-the-turtlebot3-with-ros2-on-ubuntu-server-iot-18-04/10090)  
[https://netplan.io](https://netplan.io)  
[https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Install-Debians/](https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Install-Debians/)  
[https://index.ros.org/doc/ros2/Tutorials/Colcon-Tutorial/](https://index.ros.org/doc/ros2/Tutorials/Colcon-Tutorial/)  
[https://linuxize.com/post/how-to-add-swap-space-on-ubuntu-18-04/](https://linuxize.com/post/how-to-add-swap-space-on-ubuntu-18-04/)

## [OpenCRのセットアップ](#opencr-setup)

### 依存関係をインストールし、32ビットの実行可能ファイルを実行します。

```bash
$ sudo dpkg --add-architecture armhf
$ sudo apt-get update
$ sudo apt-get install libc6:armhf
```

### アップロード用のOpenCRバイナリとツールをダウンロードします。

```bash
$ cd && rm -rf opencr_update.tar.bz2
$ wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2
$ tar -xjf ./opencr_update.tar.bz2
```

### TurtleBot3のROS 2ファームウェアをOpenCRにアップロードします。

#### TurtleBot3 Burgerの場合
32
```bash
# Set a port for OpenCR 
$ export OPENCR_PORT=/dev/ttyACM0
# Set a model of TurtleBot3 you are using
$ export OPENCR_MODEL=burger
$ cd ~/opencr_update && ./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr
```

次のウィンドウは、TurtleBot3 BurgerのファームウェアをOpenCRにアップロードした結果を示しています。
`jump_to_fw`メッセージがウィンドウの下部に表示されていることを確認してください。

```bash
aarch64
arm
OpenCR Update Start..
opencr_ld_shell ver 1.0.0
opencr_ld_main
[  ] file name   	: burger.opencr
[  ] file size   	: 168 KB
[  ] fw_name     	: burger
[  ] fw_ver      	: V180903R1
[OK] Open port   	: /dev/ttyACM0
[  ]
[  ] Board Name  	: OpenCR R1.0
[  ] Board Ver   	: 0x17020800
[  ] Board Rev   	: 0x00000000
[OK] flash_erase 	: 0.96s
[OK] flash_write 	: 1.92s
[OK] CRC Check   	: 10E28C8 10E28C8 , 0.006000 sec
[OK] Download
[OK] jump_to_fw
```

#### TurtleBot3 Waffle and Waffle Piの場合

```bash
# Set a port for OpenCR 
$ export OPENCR_PORT=/dev/ttyACM0
# Set a model of TurtleBot3 you are using
$ export OPENCR_MODEL=waffle
$ cd ~/opencr_update && ./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr
```

次のウィンドウは、TurtleBot3 BurgerのファームウェアをOpenCRにアップロードした結果を示しています。
`jump_to_fw`メッセージがウィンドウの下部に表示されていることを確認してください。

```bash
aarch64
arm
OpenCR Update Start..
opencr_ld_shell ver 1.0.0
opencr_ld_main
[  ] file name   	: waffle.opencr
[  ] file size   	: 168 KB
[  ] fw_name     	: waffle
[  ] fw_ver      	: V180903R1
[OK] Open port   	: /dev/ttyACM0
[  ]
[  ] Board Name  	: OpenCR R1.0
[  ] Board Ver   	: 0x17020800
[  ] Board Rev   	: 0x00000000
[OK] flash_erase 	: 0.96s
[OK] flash_write 	: 1.92s
[OK] CRC Check   	: 10E28C8 10E28C8 , 0.006000 sec
[OK] Download
[OK] jump_to_fw
```

RESETボタンを使用してOpenCRをリセットします。

![](/assets/images/parts/controller/opencr10/bootloader_19.png)

数秒後、特定のサウンドが再生されます。

## [ハードウェアセットアップ](#hardware-setup)

![](/assets/images/platform/turtlebot3/hardware_setup/turtlebot3_models.png)

### [パーツリスト](#part-list)

TurtleBot3 has three different models: `Burger`, `Waffle` and `Waffle Pi`. The following list shows their components. The big differences between three models are the Motor, the SBC (Single Board Computer) and the Sensors. The TurtleBot3 Waffle model is discontinued due to discontinuation of Intel® Joule™.

TurtleBot3には、`Burger`, `Waffle`、 `Waffle Pi`の3つのモデルがあります。 次のリストは、それらのコンポーネントを示しています。 3つのモデルの大きな違いは、モータ、SBC(シングルボードコンピュータ)、センサです。 TurtleBot3 Waffleモデルは、Intel®Joule™の販売終了により廃止されました。

|                        | パーツ名                         | Burger | Waffle | Waffle Pi |
| --------------------   | --------------------            | ----   | ----   | -------   |
| **シャーシ パーツ**      | Waffle プレート                   | 8      | 24     | 24        |
| .                      | プレート サポート M3x35mm          | 4      | 12     | 12        |
| .                      | プレート サポート M3x45mm          | 10     | 10     | 10        |
| .                      | PCB サポート                      | 12     | 12     | 12        |
| .                      | ホイール                          | 2      | 2      | 2         |
| .                      | タイヤ                            | 2      | 2      | 2         |
| .                      | ボールキャスタ                     | 1      | 2      | 2         |
| .                      | カメラブラケット                   | 0      | 0      | 1         |
| **モータ**              | DYNAMIXEL (XL430-W250-T)        | 2      | 0      | 0         |
| .                      | DYNAMIXEL (XM430-W210-T)        | 0      | 2      | 2         |
| **ボード**              | OpenCR1.0                       | 1      | 1      | 1         |
| .                      | Raspberry Pi 3                  | 1      | 0      | 1         |
| .                      | Intel® Joule™                   | 0      | 1      | 0         |
| .                      | USB2LDS                         | 1      | 1      | 1         |
| **リモート コントローラ** | BT-410 セット(Bluetooth 4, B LE) | 0      | 0      | 1         |
| .                      | RC-100B (リモートコントローラ)     | 0      | 0      | 1         |
| **センサ**              | LDS (HLS-LFCD2)                 | 1      | 1      | 1         |
| .                      | Intel® Realsense™ R200          | 0      | 1      | 0         |
| .                      | Raspberry Pi Camera Module v2.1 | 0      | 0      | 1         |
| **メモリ**              | MicroSD カード                   | 1      | 0      | 1         |
| **ケーブル**            | Raspberry Pi 3 電源ケーブル       | 1      | 0      | 1         |
| .                      | Intel® Joule™ 電源ケーブル        | 0      | 1      | 0         |
| .                      | Li-Po Battery 延長ケーブル        | 1      | 1      | 1         |
| .                      | DYNAMIXEL to OpenCR ケーブル     | 2      | 2      | 2         |
| .                      | USB ケーブル                     | 2      | 2      | 2         |
| .                      | カメラ ケーブル                   | 0      | 0      | 1         |
| **電源**               | SMPS 12V5A                      | 1      | 1      | 1         |
| .                      | A/C コード                       | 1      | 1      | 1         |
| .                      | LIPO バッテリ 11.1V 1,800mAh     | 1      | 1      | 1         |
| .                      | LIPO バッテリ チャージャ           | 1      | 1      | 1         |
| **ツール**              | ドライバ                         | 1      | 1      | 1         |
| .                      | リベットツール                    | 1      | 1      | 1         |
| .                      | USB3.0 ハブ                     | 0      | 1      | 0         |
| **その他**              | PH_M2x4mm_K                     | 8      | 8      | 8         |
| .                      | PH_T2x6mm_K                     | 4      | 8      | 8         |
| .                      | PH_M2x12mm_K                    | 0      | 4      | 4         |
| .                      | PH_M2.5x8mm_K                   | 16     | 12     | 16        |
| .                      | PH_M2.5x12mm_K                  | 0      | 18     | 20        |
| .                      | PH_T2.6x12mm_K                  | 16     | 0      | 0         |
| .                      | PH_M2.5x16mm_K                  | 4      | 4      | 4         |
| .                      | PH_M3x8mm_K                     | 44     | 140    | 140       |
| .                      | NUT_M2                          | 0      | 4      | 4         |
| .                      | NUT_M2.5                        | 20     | 18     | 24        |
| .                      | NUT_M3                          | 16     | 96     | 96        |
| .                      | Rivet_1                         | 14     | 20     | 22        |
| .                      | Rivet_2                         | 2      | 2      | 2         |
| .                      | スペーサ                         | 4      | 4      | 4         |
| .                      | シリコンスペーサ                   | 0      | 0      | 4         |
| .                      | ブラケット                        | 5      | 8      | 6         |
| .                      | アダプタプレート                   | 1      | 1      | 1         |

### [アセンブリマニュアル](#assembly-manual)

TurtleBots3は、組み立てられていない部品として箱に入れて提供されます。 指示に従いTurtleBot3をアセンブルします。

- `PDFダウンロード` [TurtleBot3 Burger アセンブリマニュアル](http://www.robotis.com/service/download.php?no=748)
- `PDFダウンロード` [TurtleBot3 Waffle アセンブリマニュアル](http://www.robotis.com/service/download.php?no=749)
- `PDFダウンロード` [TurtleBot3 Waffle Pi アセンブリマニュアル](http://www.robotis.com/service/download.php?no=750)

### [アセンブリビデオ](#assembly-video)

マニュアルでのアセンブリが難しい場合は、下記の動画をご覧ください。

#### [TurtleBot3 Burger](#turtlebot3-burger)

<iframe width="640" height="360" src="https://www.youtube.com/embed/rvm-m2ogrLA" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/5D9S_tcenL4" frameborder="0" allowfullscreen></iframe>

#### [TurtleBot3 Waffle](#turtlebot3-waffle)

<iframe width="640" height="360" src="https://www.youtube.com/embed/1nTMyr4ybi0" frameborder="0" allowfullscreen></iframe>

### [オープンソースハードウェア](#open-source-hardware)

Turtlebot3の主なコンポーネントは、シャーシ、モーター、ホイール、OpenCR、SBC、センサ、バッテリーです。 シャーシは、他のコンポーネントを保持するワッフルプレートです。 ワッフルプレートは手のひらほどのサイズですが、シャーシとして重要な役割を果たします。 ワッフルプレートは、製造コストを下げるために射出成形法で製造されています。 ただし、3Dプリント用のワッフルプレートのCADデータは、[Onshape][waffle_plate_on_onshape]からも入手できます。 Turtlebot3 Burgerは、二輪差動駆動タイプのプラットフォームですが、Segway、Tank、Bike、Trailerなど、さまざまな方法で構造的および機械的にカスタマイズできます。

CADデータは、フルクラウドの3D CADエディタであるOnshapeでリリースされています。 PCまたはポータブルデバイスからWebブラウザを介してアクセスします。 Onshapeを使用すると、仕事仲間と部品作りや組み立てができます。

- [TurtleBot3 Burger 3Dモデル](http://www.robotis.com/service/download.php?no=676)
- [TurtleBot3 Waffle 3Dモデル](http://www.robotis.com/service/download.php?no=677)
- [TurtleBot3 Waffle Pi 3Dモデル](http://www.robotis.com/service/download.php?no=678)

[waffle_plate_on_onshape]: https://cad.onshape.com/documents/2586c4659ef3e7078e91168b/w/14abf4cb615429a14a2732cc/e/6c94f199b347f8785a67b6f8
[Open Robotics]: http://www.osrfoundation.org/
[ROBOTIS]: http://www.robotis.com

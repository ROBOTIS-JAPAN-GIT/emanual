---
layout: archive
lang: jp
ref: autonomous_driving
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/autonomous_driving/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 24
---

<div style="counter-reset: h1 12"></div>

# [[ROS 1] 自動運転](#ros-1-自動運転)

{% capture notice_01 %}
**注釈**: この手順は、`Ubuntu16.04`と`ROSKineticKame`でテスト済みです。
{% endcapture %}
<div class="notice">{{ notice_01 | markdownify }}</div>

e-Manualの内容は、予告なしに更新される場合があります。 そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

次の手順では、AutoRaceパッケージを使用してROS上で自動運転のTurtleBot3を構築する方法について説明します。

## [自動運転に必要なもの](#自動運転に必要なもの)

`TurtleBot3 Burger`  
- これは、ROSでの自動運転にAutoRaceパッケージを使用するための基本モデルです。
- 提供されているソースコードであるAutoRaceパッケージは、TurtleBot3 Burgerに基づいて作成されています。

`Remote PC`  
- Turtlebot3上のシングルボードコンピューター(SBC)と通信します。
- ROS 1を搭載したラップトップ、デスクトップ、またはその他のデバイス。

`Raspberry Pi カメラモジュールとカメラマウント`  
- ROSがサポートしている場合は、別のモジュールを使用できます。
- カメラをキャリブレーションするために提供されるソースコードは、([Fisheye Lens](https://www.waveshare.com/rpi-camera-g.htm))モジュールに基づいて作成されます。

`AutoRaceのトラックとオブジェクト`  
- [ROBOTIS_GIT/autorace](https://github.com/ROBOTIS-GIT/autorace_track)で、オートレーストラック、交通標識、信号機、その他のオブジェクトの3DCADファイルをダウンロードします。
- [ROBOTIS-GIT/autorace_referee](https://github.com/ROBOTIS-GIT/autorace_referee)でレフリーシステムをダウンロードします。

## [Autoraceのパッケージインストール](#Autoraceのパッケージインストール)

次の手順では、パッケージのインストール方法とカメラのキャリブレーション方法について説明します。

1. `Remote PC`と`SBC`の両方にAutoRaceパッケージをインストールします。
``` bash
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_autorace_2020.git
$ cd ~/catkin_ws && catkin_make
```

2. `Remote PC`と`SBC`の両方に追加の依存パッケージをインストールします。
``` bash
$ sudo apt-get install ros-kinetic-image-transport ros-kinetic-cv-bridge ros-kinetic-vision-opencv python-opencv libopencv-dev ros-kinetic-image-proc
```

3. [SBCのカメラのキャリブレーション](#SBCのカメラのキャリブレーション)が必要です。

## [SBCでカメラをキャリブレーションする](#SBCでカメラをキャリブレーションする)

自動運転では、カメラのキャリブレーションが非常に重要です。 以下では、カメラを段階的に簡単に調整する方法について説明します。

### [カメライメージングキャリブレーション](#カメライメージングキャリブレーション)

1. `Remote PC`でroscoreを起動します。
``` bash
$ roscore
```

2. `SBC`でカメラをトリガーします。
``` bash
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_camera_pi.launch
```

    **警告**: 必ず`${Autorace_Misson}`を指定してください。(つまり、**roslaunch turtlebot3_autorace_traffic_light_camera turtlebot3_autorace_camera_pi.launch**)
    {: .notice--warning}

3. `Remote PC`でrqt_image_viewを実行します。
``` bash
$ rqt_image_view
```

4. トピックのチェックボックスで**/camera/image/compressed** (または、**/camera/image/**)を指定します。

    ![](/assets/images/platform/turtlebot3/autonomous_driving/tb3_click_compressed.png)

5. `Remote PC`でrqt_reconfigureを実行します。
``` bash
$ rosrun rqt_reconfigure rqt_reconfigure
```

6. **camera**をクリックし、パラメータ値を変更して、カメラから鮮明な画像を表示します。

    ![](/assets/images/platform/turtlebot3/autonomous_driving/rqt_reconfigure_camera_yaml_edit_01.png)

7. **turtlebot3_autorace_[Autorace Misson]_camera/calibration/camera_calibration**フォルダーにある**camera.yaml**ファイルを開きます。

8. 変更した値をファイルに書き込みます。

    ![](/assets/images/platform/turtlebot3/autonomous_driving/rqt_reconfigure_camera_yaml_edit_02.png)

### [カメラ固有のキャリブレーション](#カメラ固有のキャリブレーション)

A4サイズの用紙にチェッカーボードを印刷します。 チェッカーボードは、カメラ固有のキャリブレーションに使用されます。
- チェッカーボードは、**turtlebot3_autorace_camera/data/checkerboard_for_calibration.pdf**に保存されます。
- **turtlebot3_autorace_camera/launch/turtlebot3_autorace_intrinsic_camera_calibration.launch**のパラメータ値を変更します。
- カメラキャリブレーションについては、[Camera Calibration manual](http://wiki.ros.org/camera_calibration)のROS Wikiを参照してください。

  ![](/assets/images/platform/turtlebot3/autonomous_driving/autorace_checkerboard.png)
  > チェッカーボード

1. `Remote PC`でroscoreを実行します。
``` bash
$ roscore
```

2. `SBC`でカメラをトリガーします。
``` bash
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_camera_pi.launch
```

    **警告**: 必ず`${Autorace_Misson}`を指定してください。 (つまり、**roslaunch turtlebot3_autorace_traffic_light_camera turtlebot3_autorace_camera_pi.launch**)
    {: .notice--warning}

3. `Remote PC`で組み込みのカメラキャリブレーション起動ファイルを実行します。
``` bash
$ export AUTO_IN_CALIB=calibration
$ export GAZEBO_MODE=false
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

4. チェッカーボードを使用してカメラをキャリブレーションし、**CALIBRATE**をクリックします。

    ![](/assets/images/platform/turtlebot3/autonomous_driving/intrinsic_camera_calibration_test.png)

5. **Save**をクリックして、固有のキャリブレーションデータを保存します。

    ![](/assets/images/platform/turtlebot3/autonomous_driving/intrinsic_camera_calibration_capture.png)

6. **calibrationdata.tar.gz**フォルダーは、**/tmp**に作成されます。

    ![](/assets/images/platform/turtlebot3/autonomous_driving/camera_320_240_saved_path_01.png)

7. **calibrationdata.tar.gz**フォルダーを展開し、**ost.yaml**を開きます。
    
    ![](/assets/images/platform/turtlebot3/autonomous_driving/open_ost_yaml.png)  
    > ost.yaml  
    
    ![](/assets/images/platform/turtlebot3/autonomous_driving/ost_yaml.png)  
    > ost.yamlの固有のキャリブレーションデータ

8. **ost.yaml**から**camerav2_320x240_30fps.yaml**にデータをコピーして貼り付けます。
    
    ![](/assets/images/platform/turtlebot3/autonomous_driving/open_320_240_30fps.png)  
    > camerav2_320x240_30fps.yaml
    
    ![](/assets/images/platform/turtlebot3/autonomous_driving/camerav2_320_240_30fps.png)
    > camerav2_320x240_30fps.yamlの固有キャリブレーションデータ

### [外因性カメラのキャリブレーション](#外因性カメラのキャリブレーション)

1. `Remote PC`でroscoreを起動します。
``` bash
$ roscore
```

2. `SBC`でカメラをトリガーします。
``` bash
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_camera_pi.launch
```

    **警告**: 必ず`${Autorace_Misson}`を指定してください。 (つまり、**roslaunch turtlebot3_autorace_traffic_light_camera turtlebot3_autorace_camera_pi.launch**)
    {: .notice--warning}

3. `Remote PC`でコマンドを使用します。
``` bash
$ export AUTO_IN_CALIB=action
$ export GAZEBO_MODE=false
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

4. `Remote PC`で外部カメラキャリブレーション起動ファイルを実行します。
``` bash
$ export AUTO_EX_CALIB=calibration
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_extrinsic_camera_calibration.launch
```

5. `Remote PC`でrqtを実行します。
```
$ rqt
```
    
6. **plugins** > **visualization** > **Image view**を選択します。複数のウィンドウが表示されます。

7. 各モニターで`/camera/image_extrinsic_calib/compressed`および`/camera/image_projected_compensated`トピックを選択します。
  - 2つの画面のいずれかで、赤い長方形のボックスの付いた画像が表示されます。 もう1つは、地上投影図（鳥瞰図）を示しています。
    ![](/assets/images/platform/turtlebot3/autonomous_driving/camera_image_extrinsic_calib_compressed.png)
    > `/camera/image_extrinsic_calib/compressed` topic
    
    ![](/assets/images/platform/turtlebot3/autonomous_driving/camera_image_projected_compensated.png)
    > `/camera/image_projected_compensated` topic

6. `Remote PC`でrqt_reconfigureを実行します。
``` bash
$ rosrun rqt_reconfigure rqt_reconfigure
```

7. `/camera/image_projection`と`/camera/image_compensation_projection`のパラメーターを調整します。
  - `/camera/image_projection`のパラメーター値を変更します。`/camera/image_extrinsic_calib/compressed`トピックに影響します。
  - 固有のカメラキャリブレーションは、赤い長方形で囲まれた画像を変換し、車線の上から見た画像を表示します。
    
    ![](/assets/images/platform/turtlebot3/autonomous_driving/camera_image_projection_compensation_projection.png)
    > rqt_reconfigure
  
    ![](/assets/images/platform/turtlebot3/autonomous_driving/modify_image_projection_image_extrinsic_calib.png)
    > Result from parameter modification.   
  
### [認識の設定](#認識の設定)

すべてのカメラキャリブレーション（カメライメージングキャリブレーション、固有キャリブレーション、外因性キャリブレーション）を完了したら、キャリブレーションがカメラに正常に適用されていることを確認します。
次の手順では、認識の設定について説明します。

1. `Remote PC`でroscoreを起動します。
``` bash
$ roscore
```

2. `SBC`でカメラをトリガーします。
``` bash
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_camera_pi.launch
```

    **警告**: 必ず`${Autorace_Misson}`を指定してください。(つまり、**roslaunch turtlebot3_autorace_traffic_light_camera turtlebot3_autorace_camera_pi.launch**)
    {: .notice--warning}

3. `Remote PC`で組み込みのカメラキャリブレーション起動ファイルを実行します。
``` bash
$ export AUTO_IN_CALIB=action
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

4. ターミナルを開き、`Remote PC`でコマンドを使用します。
``` bash
$ export AUTO_EX_CALIB=action
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_extrinsic_camera_calibration.launch
```
    以下の説明では、主に`特徴検出器/カラーフィルター`を対象物認識用に調整します。 ここ以降のすべての調整は、互いのプロセスから独立しています。ただし、各パラメータを連続して調整する場合は、すべての調整を完全に完了してから、次に進みます。

## [車線検出](#車線検出)

車線検出パッケージにより、Turtlebot3は外部の影響を受けずに2つの車線間を走行できます。

次の手順では、車線検出機能の使用方法とrqtを介したカメラのキャリブレーション方法について説明します。

1. TurtleBot3を黄色と白の車線の間に配置します。
    
    **注記**: 黄色の車線がロボットの左側に配置され、白い車線がロボットの右側に配置されていることを確認してください。
    {: .notice}

2. `Remote PC`でroscoreを起動します。
``` bash
$ roscore
```

3. `SBC`でカメラをトリガーします。
``` bash
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_camera_pi.launch
```

    **警告**: 必ず`${Autorace_Misson}`を指定してください。(つまり、**roslaunch turtlebot3_autorace_traffic_light_camera turtlebot3_autorace_camera_pi.launch**)
    {: .notice--warning}

4. `Remote PC`で組み込みのカメラキャリブレーション起動ファイルを実行します。
``` bash
$ export AUTO_IN_CALIB=action
$ export GAZEBO_MODE=false
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

5. `Remote PC`で外部カメラキャリブレーション起動ファイルを実行します。
``` bash
$ export AUTO_EX_CALIB=action
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_camera turtlebot3_autorace_extrinsic_camera_calibration.launch
```

6. `Remote PC`で車線検出起動ファイルを実行します
``` bash
$ export AUTO_DT_CALIB=calibration
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_detect turtlebot3_autorace_detect_lane.launch
```

7. `Remote PC`でrqtを実行します。
```
$ rqt
```

8. **plugins** > **visualization** > **Image view**を選択します。複数のウィンドウが表示されます。
  
9. 各画像ビューで3つのトピックを選択します： `/detect/image_yellow_lane_marker/compressed`、`/detect/image_lane/compressed`、 `/detect/image_white_lane_marker/compressed`
    - 左（黄色の線）と右（白の線）の画面には、フィルター処理された画像が表示されます。
    ![](/assets/images/platform/turtlebot3/autonomous_driving/rqt_yellow_lane_detection.png)  
    > `/detect/image_yellow_lane_marker/compressed`トピックのイメージビュー
    
    ![](/assets/images/platform/turtlebot3/autonomous_driving/rqt_white_lane_detection.png)
    > Image view of `/detect/image_white_lane_marker/compressed` topic
    
    - 中央の画面は、TurtleBot3からのカメラのビューです。
    ![](/assets/images/platform/turtlebot3/autonomous_driving/rqt_image_lane.png)
    > `/detect/image_lane/compressed`トピックのイメージビュー

10. `Remote PC`でrqt_reconfigureを実行します。
``` bash
$ rosrun rqt_reconfigure rqt_reconfigure
```

11. **Detect Lane**をクリックし、パラメーター調整してラインカラーフィルタリングを実行します。

      ![](/assets/images/platform/turtlebot3/autonomous_driving/result_of_ybw_configuration_01.png)  
      > 車線検出パラメータのリスト
      
      ![](/assets/images/platform/turtlebot3/autonomous_driving/result_of_ybw_configuration_02.png)
      > フィルター処理された画像は、rqt_reconfigureでパラメーターを調整した結果です。
    
    **ヒント**：部屋の光の輝度などの物理的環境により、ラインカラーフィルタリングのキャリブレーションプロセスが難しい場合があります。すべてをすばやく行うには、次の**lane.yaml**ファイルの値を入力します。 再構成パラメーターの**turtlebot3_autorace_[Autorace_Misson]_detect/param/lane/**で、キャリブレーションを開始します。 色相を低く調整します - 最初は高い値です。（1）色相値は色を意味し、`黄色`、`白`などのすべての色には、独自の色相値の領域があります（hsvマップを参照）。次に、飽和度を低 - 高値に校正します。（2）すべての色には独自の彩度フィールドもあります。 最後に、明度を低 - 高値に調整します。（3）ただし、ソースコードには自動調整機能があるため、明度の低い値を校正しても意味がありません。明度の高い値を255に設定するだけです。明確にフィルタリングされたライン画像により、車線の明確な結果が得られます。
     {：.notice--success}

12. **turtlebot3_autorace_[Autorace_Misson]_detect/param/lane/**にある**lane.yaml**ファイルを開きます。変更した値をファイルに書き込む必要があります。これにより、次回の起動からここで設定したときに、カメラがパラメーターを設定します。

    ![](/assets/images/platform/turtlebot3/autonomous_driving/write_lane_yaml.png)
    
13. **rqt_rconfigure**と**turtlebot3_autorace_detect_lane**の両方を閉じます。

14. ターミナルを開き、`Remote PC`でコマンドを使用します。
``` bash
$ export AUTO_DT_CALIB=action
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_detect turtlebot3_autorace_detect_lane.launch
```

15. 結果が正しく表示されるかどうかを確認します。
    
    - ターミナルを開き、`Remote PC`でコマンドを使用します。
    ``` bash
    $ roslaunch turtlebot3_autorace_${Autorace_Misson}_control turtlebot3_autorace_control_lane.launch
    ```

    - ターミナルを開き、`Remote PC`でコマンドを使用します。
    ``` bash
    $ roslaunch turtlebot3_bringup turtlebot3_robot.launch
    ```
    
16. コマンドを使用した後、TurtleBot3が実行を開始します。

## [交通標識の検出](#交通標識の検出)

TurtleBot3は、`SIFTアルゴリズム`を備えたノードを使用して交通標識を検出し、構築されたトラックを運転しながらプログラムされたタスクを実行できます。 提供された指示にしたがって交通標識検出を使用します。

**注釈** ： 交通標識のエッジが多いほど、SIFTからの認識結果が増加します。
{: .notice} 

1. TurtleBot3のカメラと `rqt_image_view`ノードを使用して交通標識の写真を撮ります。
2. Linux OSで使用できるフォトエディターを使用して写真を編集します。
3. TurtleBot3を車線に配置します。 交通標識は、TurtleBot3が簡単に確認できる場所に配置する必要があります。

    **注釈** ： まだ車線でTurtleBot3を実行していません。
    {: .notice}

4. `Remote PC`でrqt_image_viewを実行します。
```
$ rqt_image_view
```

4. 選択ボックスで`/camera/image_compensated`トピックを選択します。 画面にTurtleBot3からのビューが表示されます。
5. `Remote PC`から写真をキャプチャし、フォトエディターで編集します。
6. 編集した画像を、配置したturtlebot3_autoraceパッケージ **/turtlebot3_autorace/turtlebot3_autorace_detect/file/detect_sign/**に配置し、必要に応じて名前を変更します。（ただし、デフォルトのファイル名を変更する場合は、ソース **detect_sign.py**ファイルに書き込まれているファイル名を変更する必要があります。）

7. ターミナルを開き、`Remote PC`でコマンドを使用します。
``` bash
$ roslaunch turtlebot3_autorace_${Autorace_Misson}_detect turtlebot3_autorace_detect_sign.launch
```

8. ターミナルを開き、`Remote PC`でコマンドを使用します。
```bash
$ rqt_image_view
```

9. 選択ボックスで`/detect/image_traffic_sign/compressed`トピックを選択します。 画面に交通標識検出の結果が表示されます。

## [TurtleBot3 AutoRace 2020](#turtlebot3-autorace-2020)

![](/assets/images/platform/turtlebot3/autonomous_driving/autorace_rbiz_challenge_2017_robots_1.png)

AutoRaceは、自動運転ロボットプラットフォームをめぐる競争です。
ロボットアプリケーション開発にさまざまな条件を提供するために、ゲームは可能な限り構造的な規制を提供しません。 提供されるオープンソースはROSに基づいており、このコンテストに適用できます。 内容は継続的に更新できます。 コンテストに参加して、あなたのスキルを示してください。

**警告** ： ミッションを開始するには、必ず[自動運転](#autonomous-driving)をお読みください。
{: .notice--warning}

### [ミッション1：信号機](#ミッション1-信号機)

信号機はAutoRaceの最初のミッションです。 TurtleBot3は信号を認識し、コースを開始します。

<iframe width="560" height="315" src="https://www.youtube.com/embed/JNj_STs3OSg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### [信号機のカメラキャリブレーション](#信号機のカメラキャリブレーション)

1. ターミナルを開き、`Remote PC`でコマンドを使用します。
```bash
$ export AUTO_DT_CALIB=calibration
$ roslaunch turtlebot3_autorace_traffic_light_detect turtlebot3_autorace_detect_traffic_light.launch
```

2. `Remote PC`でrqtを実行します。
```bash
rqt
```

3. `/detect/image_red_light`、`/detect/image_yellow_light`、`/detect/image_green_light`、`/detect/image_traffic_light`の4つのトピックを選択します。

4. rqt_reconfigureを実行します。
```bash
$ rosrun rqt_reconfigure rqt_reconfigure
```

5. 信号機のトピックに関するパラメーターを調整して、交通標識の検出を強化します。 パラメーターの調整方法は、[車線検出](＃車線検出)の手順5と同様です。

6. **turtlebot3_autorace_traffic_light_detect/param/traffic_light/**にある**traffic_light.yaml**ファイルを開きます。

7. 変更した値をファイルに書き込んで保存します。

8. 次のステップから、キャリブレーションが正常に適用されたかどうかをテストするために、実行中のrqtとrqt_reconfigureの両方を終了します。

9. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_DT_CALIB=action
$ roslaunch turtlebot3_autorace_traffic_light_detect turtlebot3_autorace_detect_traffic_light.launch
```

10. rqt_image_viewを実行します。
```bash
$ rqt_image_view
```

11. 信号機のキャリブレーションが正常に適用されていることを確認してください。

#### [信号機ミッションを実行する](#信号機ミッションを実行する)

**警告** : 信号機ノードを実行する前に、必ず[信号機のカメラキャリブレーション](＃信号機のカメラキャリブレーション)をお読みください。
{: .notice--warning}

1. `Remote PC`でコマンドを使用します。
``` bash
$ export AUTO_IN_CALIB=action
$ export GAZEBO_MODE=false
$ roslaunch turtlebot3_autorace_traffic_light_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

2. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_EX_CALIB=action
$ export AUTO_DT_CALIB=action
$ roslaunch turtlebot3_autorace_traffic_light_core turtlebot3_autorace_core.launch
```

3. `Remote PC`でコマンドを使用します。
```bash
$ rostopic pub -1 /core/decided_mode std_msgs/UInt8 "data: 3"
```

### [ミッション2：交差点](#ミッション2-交差点)

交差点は、AutoRaceの2番目のミッションです。TurtleBot3は、交差点のコースで特定の交通標識（カーブ標識など）を検出し、指定された方向に進みます。

<iframe width="560" height="315" src="https://www.youtube.com/embed/8K4GMbfXFXI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### [交差点ミッションの実行方法](#交差点ミッションの実行方法)

1. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_IN_CALIB=action
$ roslaunch turtlebot3_autorace_intersection_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

2. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_EX_CALIB=action
$ export AUTO_DT_CALIB=action
$ roslaunch turtlebot3_autorace_intersection_core turtlebot3_autorace_core.launch
```

3. `Remote PC`でコマンドを使用します。
```bash
$ rostopic pub -1 /core/decided_mode std_msgs/UInt8 "data: 2"
```

### [ミッション3：建築物](#ミッション3-建築物)

建築物はAutoRaceの3番目のミッションです。 TurtleBot3は、運転中のトラックでの建築物を回避します。

<iframe width="560" height="315" src="https://www.youtube.com/embed/88nXiX8UUtw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### [How to Run Construction Mission](#how-to-run-construction-mission)

1. `Remote PC`でコマンドを使用します。 
```bash
$ export AUTO_IN_CALIB=action
$ export GAZEBO_MODE=false
$ roslaunch turtlebot3_autorace_construction_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

2. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_EX_CALIB=action
$ export AUTO_DT_CALIB=action
$ roslaunch turtlebot3_autorace_construction_core turtlebot3_autorace_core.launch
```

3. `Remote PC`でコマンドを使用します。
```bash
$ rostopic pub -1 /core/decided_mode std_msgs/UInt8 "data: 2"
```

### [ミッション4：駐車](#ミッション4-駐車場)

駐車場はAutoRaceの4番目のミッションです。 TurtleBot3は駐車標識を検出し、駐車場に駐車します。

<iframe width="560" height="315" src="https://www.youtube.com/embed/F3ZG8PlagaA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### [パーキングミッションの実行方法](#パーキングミッションの実行方法)

1. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_IN_CALIB=action
$ export GAZEBO_MODE=false
$ roslaunch turtlebot3_autorace_parking_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

2. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_EX_CALIB=action
$ export AUTO_DT_CALIB=action
$ roslaunch turtlebot3_autorace_parking_core turtlebot3_autorace_core.launch
```

3. `Remote PC`でコマンドを使用します。
```bash
$ rostopic pub -1 /core/decided_mode std_msgs/UInt8 "data: 2"
```

### [ミッション5：踏切](#ミッション5-踏切)

踏切はAutoRaceの5番目のミッションです。 TurtleBot3が踏切に遭遇すると、運転を停止し踏切が開くまで待ちます。

<iframe width="560" height="315" src="https://www.youtube.com/embed/xyBaLeg4Ifk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### [踏切のカメラキャリブレーション](#踏切のカメラキャリブレーション)

1. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_DT_CALIB=calibration
$ roslaunch turtlebot3_autorace_level_crossing_detect turtlebot3_autorace_detect_level.launch
```

2. rqtを実行します。
```bash
$ rqt
```

3. 2つのトピックを選択します：`/detect/image_level_color_filtered`、`/detect/image_level`

4. rqt_reconfigureを実行します。
```bash
$ rosrun rqt_reconfigure rqt_reconfigure
```

4. `/detect_level`を選択し、踏切トピックに関するパラメーターを調整して、踏切オブジェクトの検出を強化します。 パラメーターの調整方法は、[車線検出](＃車線検出)の手順5と同様です。

5. **turtlebot3_autorace_stop_bar_detect/param/level/**にある**level.yaml**を開きます。

6. 変更した値をファイルに書き込んで保存します。

7. 検出レベルの起動ファイルを実行します。
```bash
$ export AUTO_DT_CALIB=action
$ roslaunch turtlebot3_autorace_detect turtlebot3_autorace_detect_level.launch
```

#### [踏切ミッションの実行方法](#踏切ミッションの実行方法)

1. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_IN_CALIB=action
$ export GAZEBO_MODE=false
$ roslaunch turtlebot3_autorace_level_crossing_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

2. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_EX_CALIB=action
$ export AUTO_DT_CALIB=action
$ roslaunch turtlebot3_autorace_level_crossing_core turtlebot3_autorace_core.launch
```

3. `Remote PC`でコマンドを使用します。
```bash
$ rostopic pub -1 /core/decided_mode std_msgs/UInt8 "data: 2"
```

### [ミッション6：トンネル](#ミッション6-トンネル)

トンネルはAutoRaceの6番目のミッションです。TurtleBot3はトンネルを正常に通過します。

<iframe width="560" height="315" src="https://www.youtube.com/embed/QtBx_MKLsPs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### [トンネルミッションの実行方法](#トンネルミッションの実行方法)

1. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_IN_CALIB=action
$ export GAZEBO_MODE=false
$ roslaunch turtlebot3_autorace_tunnel_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
```

3. `Remote PC`でコマンドを使用します。
```bash
$ export AUTO_EX_CALIB=action
$ export AUTO_DT_CALIB=action
$ export TURTLEBOT3_MODEL=burger
$ roslaunch turtlebot3_autorace_tunnel_core turtlebot3_autorace_core.launch
```

4. `Remote PC`でコマンドを使用します。
``` bash
$ rostopic pub -1 /core/decided_mode std_msgs/UInt8 "data: 2"
```

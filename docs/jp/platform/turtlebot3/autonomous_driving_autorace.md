---
layout: archive
lang: jp
ref: autonomous_driving_autorace
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/autonomous_driving_autorace/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 25
---

<div style="counter-reset: h1 13"></div>
<div style="counter-reset: h2 6"></div>

<!--[dummy Header 1]>
  <h1 id="dummy"><a href="#dummy">Dummy</a></h1>
<![end dummy Header 1]-->

## [TurtleBot3 AutoRace](#turtlebot3-autorace)

![](/assets/images/platform/turtlebot3/autonomous_driving/autorace_rbiz_challenge_2017_robots_1.png)

AutoRaceは、自動運転ロボットプラットフォームをめぐる競技です。 ロボットアプリケーション開発のさまざまな条件を提供するために、ゲームは可能な限り構造的な規制を少なくします。 コンテンツ全体が[ソフトウェア](https://github.com/ROBOTIS-GIT/autorace_referee)(レフリーシステムのソースコード)と[ハードウェア](https://github.com/ROBOTIS-GIT/autorace_track)（ゲームマップのstp/dwgファイル）に関して公開にされています。

ロボット全体、さらにはフィールドのレフリーシステムもROSによって運営されているので、さらに多くの種類のコンテンツを作成できます。 各コンテストに参加しオープンソースを入手してください。

**警告**: このページで作成された内容を読んでいることに注意してください。 AutoRace 2020に参加する場合は、[TurtleBot3 AutoRace 2020](/docs/jp/platform/turtlebot3/autonomous_driving/#turtlebot3-autorace-2020)を参照してください。
{: .notice--warning}

### [TurtleBot3 AutoRace チュートリアル](#turtlebot3-autorace-チュートリアル)

- AutoRaceチュートリアルのソースコード： [turtlebot3_autorace パッケージ][turtlebot3_autorace]

- チュートリアル1：信号機

  <iframe width="560" height="315" src="https://www.youtube.com/embed/yp7MVZCiaWs" frameborder="0" allowfullscreen></iframe>

- チュートリアル2：車線追跡

  <iframe width="560" height="315" src="https://www.youtube.com/embed/1RBOfPWdpsc" frameborder="0" allowfullscreen></iframe>

- チュートリアル3：駐車場

  <iframe width="560" height="315" src="https://www.youtube.com/embed/ValFotgBfC0" frameborder="0" allowfullscreen></iframe>

- チュートリアル4：ノードの最適化

  <iframe width="560" height="315" src="https://www.youtube.com/embed/YpjHOzx432M" frameborder="0" allowfullscreen></iframe>

- チュートリアル5：踏切

  <iframe width="560" height="315" src="https://www.youtube.com/embed/MBDMBq6IDd4" frameborder="0" allowfullscreen></iframe>

- チュートリアル6：トンネル

  <iframe width="560" height="315" src="https://www.youtube.com/embed/ezkwAs1kVkM" frameborder="0" allowfullscreen></iframe>


### [チュートリアル：1. 要件](#チュートリアル：1-要件)

- `TurtleBot3 Burger`
  - ROSおよび依存するROSパッケージをロボットにインストールする必要があります。
  - [TurtleBot3 e-Manual](http://emanual.robotis.com/docs/en/platform/turtlebot3/overview/)で説明されているTurtleBot3 Burgerのすべての機能は、TurtleBot3Autoのソースコードを実行する前にテストする必要があります。

- `Remote PC (ラップトップ、デスクトップ etc.)`
  - ROSおよび依存するROSパッケージをコンピューターにインストールする必要があります
  - [TurtleBot3 e-Manual](http://emanual.robotis.com/docs/en/platform/turtlebot3/overview/)で説明されているTurtleBot3 Burgerのすべての機能は、TurtleBot3Autoのソースコードを実行する前にテストする必要があります。

- `TurtleBot3 Burger アドオン`
  - ラズベリーパイカメラタイプG（魚眼レンズ）：入手可能[こちら](https://www.waveshare.com/rpi-camera-g.htm)
    - 導電性材料のフレームに取り付ける前に、ページの`4つのネジ穴の特徴`を注意深く参照してください。
  - ラズベリーパイカメラマウント

- 交通標識、信号機、その他のオブジェクトなどの`トラック構造とアクセサリ`。
  - [autorace_referee](https://github.com/ROBOTIS-GIT/autorace_referee)からAutoRaceレフリーシステムのソースを取得します
  - [autorace_track](https://github.com/ROBOTIS-GIT/autorace_track)からレーストラックの3DCADモデルデータを取得します

### [チュートリアル：2. AutoRaceパッケージのインストール](#チュートリアル：2-AutoRaceパッケージのインストール)

`Remote PCとTurtleBot SBC`でターミナルを開き、AutoRaceパッケージをインストールします。

``` bash
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_autorace.git
$ cd ~/catkin_ws && catkin_make
```

### [チュートリアル：3. 追加の依存パッケージのインストール](#チュートリアル-3-追加の依存パッケージのインストール)

`Remote PCとTurtleBot SBC`で新しいターミナルを開き、次のように入力します

``` bash
$ sudo apt-get install ros-kinetic-image-transport ros-kinetic-cv-bridge ros-kinetic-vision-opencv python-opencv libopencv-dev ros-kinetic-image-proc
```

### [チュートリアル：4. キャリブレーション](#チュートリアル-4-キャリブレーション)

#### [チュートリアル：4.1. カメライメージングキャリブレーション](#チュートリアル-41-カメライメージングキャリブレーション)

1. `Remote PC`で新しいターミナルを開き、次の用に入力します。

    ``` bash
    $ roscore
    ```

2. `TurtleBot SBC`で新しいターミナルを開き、次のように入力します

    ``` bash
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_camera_pi.launch
    ```

3. `Remote PC`で新しいターミナルを開き、次のように入力します。

    ``` bash
    $ rqt_image_view
    ```

    次に、選択ボックスで`/camera/image/compressed`または`/camera/image/`トピックをクリックします。 すべてが正常に機能する場合、画面にはロボットからのビューが表示されます。

4. `Remote PC`で新しいターミナルを開き、次のように入力します。

    ``` bash
    $ rosrun rqt_reconfigure rqt_reconfigure
    ```

    次に、`カメラ`をクリックし、カメラがきれいで十分に明るい画像を表示するようにパラメーター値を調整します。 その後、各値を`turtlebot3_autorace_camera/calibration/camera_calibration/camera.yaml`に上書きします。 これにより、次回の起動からここで設定したときに、カメラがパラメーターを設定します。

#### [チュートリアル：4.2. カメラ固有のキャリブレーション](#チュートリアル-42-カメラ固有のキャリブレーション)

1. `リモートPC`で新しいターミナルを開き、次のように入力します

    ``` bash
    $ roscore
    ```

2. `TurtleBot SBC`で新しいターミナルを開き、次のように入力します。

    ``` bash
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_camera_pi.launch
    ```

3. `Remote PC`のカメラキャリブレーション用のチェッカーボードをA4サイズの用紙に印刷します。 チェッカーボードは `turtlebot3_autorace_camera/data/checkerboard_for_calibration.pdf`にあります。 [キャリブレーションマニュアル](http://wiki.ros.org/camera_calibration)を参照し、`turtlebot3_autorace_camera/launch/turtlebot3_autorace_intrinsic_camera_calibration.launch`に記述されているパラメーター値を変更してください。

4. `Remote PC`で新しいターミナルを開き、次のように入力します。

    ``` bash
    $ export AUTO_IN_CALIB=calibration
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
    ```

5. `Remote PC`でキャリブレーションが終了すると、組み込みカメラのキャリブレーションファイルが`turtlebot3_autorace_camera/calibration/intrinsic_calibration/camerav2_320x240_30fps.yaml`に保存されます。

#### [チュートリアル：4.3. 外因性カメラのキャリブレーション](#チュートリアル-43-外因性カメラのキャリブレーション)

1. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ roscore
    ```

2. 新しいターミナルを`TurtleBot SBC`で開き、次のように入力します。

    ``` bash
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_camera_pi.launch
    ```

3. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_IN_CALIB=action
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
    ```

4. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_EX_CALIB=calibration
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_extrinsic_camera_calibration.launch
    ```

5. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ```
    $ rqt
    ```

    画面上部の`plugins` -> `visualization` -> `Image view`をクリックすると、カメラビューの監視機能が追加されます。 それに従って、rqtプレートに2つの追加モニターを作成します。 次に、追加の各モニターで`/camera/image_extrinsic_calib/compressed`トピックと`/camera/image_projected_compensated`トピックを選択します。 すべてが正常に機能する場合、画面の1つは赤い長方形の画像を表示し、もう1つは相互に基づいた地面投影ビュー（鳥瞰図）を表示します。

6. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ rosrun rqt_reconfigure rqt_reconfigure
    ```

    次に、画像の視覚的な変更を実行する`/camera/image_projection`と`/camera/image_compensation_projection`のパラメーター値を調整します。 パラメーター`image_projection`は、`/camera/image_extrinsic_calib/compressed`画像の赤い長方形の形状を変更します。 固有のカメラキャリブレーションは、赤い長方形で囲まれた画像を変換し、車線の上から見た画像を表示します。 その後、`turtlebot3_autorace_camera/calibration/externalsic_calibration/`の`yaml`ファイルの各値を上書きします。 これにより、次回の起動からここで設定したときに、カメラがパラメーターを設定します。

#### [チュートリアル：4.4. 認識の設定](#チュートリアル-44-認識の設定)

Until now, all the preprocess of image must have been tested.

1. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ roscore
    ```

2. 新しいターミナルを`TurtleBot SBC`で開き、次のように入力します。

    ``` bash
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_camera_pi.launch
    ```

3. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_IN_CALIB=action
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
    ```

4. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_EX_CALIB=action
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_extrinsic_camera_calibration.launch
    ```

    From now, the following descriptions will mainly adjust `feature detector / color filter` for object recognition. Every adjustment after here is independent to each other's process. However, to make sure, if you want to adjust each parameters in series, finish every adjustment perfectly, then continue to next.

### [チュートリアル：5. ミッション](#チュートリアル-5-ミッション)

#### [チュートリアル：5.1. レーン検出](#チュートリアル-51-レーン検出)

1. ロボットをレーンに置きます。ロボットを正しく配置した場合、`黄色い線`はロボットの左側に配置され、もちろん`白い線`はロボットの右側に配置されます。`turtlebot3_bringup`パッケージの` turtlebot3_robot`ノードがまだ起動されていないことを確認してください。走行中の場合、ロボットは突然軌道上を走行します。


2. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_DT_CALIB=calibration
    $ roslaunch turtlebot3_autorace_detect turtlebot3_autorace_detect_lane.launch
    ```

3. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ```
    $ rqt
    ```

    画面上部の`plugins` -> `visualization` -> `Image view`をクリックすると、カメラビューの監視機能が追加されます。 それにしたがって、rqtプレートに3つの追加モニターを作成します。次に、追加の各モニターで`/detect/image_yellow_lane_marker/compressed`、`/detect/image_lane/compressed`および`/detect/image_white_lane_marker /compressed`トピックを選択します。すべてが正常に機能する場合、左右の画面には黄色の線と白い線のフィルタリングされた画像が表示され、中央の画面にはロボットが移動する場所の車線が表示されます。 キャリブレーションモードでは、左右の画面が白く表示され、中央の画面に異常な結果が表示される場合があります。ここから、正しい線と方向を表示するようにフィルターパラメーターを調整する必要があります。

4. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ rosrun rqt_reconfigure rqt_reconfigure
    ```

    次に、画像の視覚的な変更を実行する `/camera/image_projection`と`/camera/image_compensation_projection`のパラメーター値を調整します。 パラメーター`image_projection`は、`/camera/image_extrinsic_calib/compressed`画像の赤い長方形の形状を変更します。カメラ固有のキャリブレーションは、赤い長方形で囲まれた画像を変換し、車線の上から見た画像を表示します。 その後、`turtlebot3_autorace_detect/param/lane/`の`lane.yaml`ファイルに値を上書きします。これにより、次回の起動からここで設定したときに、カメラがパラメーターを設定します。

    **ヒント** ： ラインカラーフィルタリングのキャリブレーションプロセスは、部屋の光の輝度などの物理的環境のために非常に難しい場合があります。したがって、この手順を実行するには忍耐力が必要です。すべてをすばやく行うには、再構成パラメーターに `turtlebot3_autorace_detect/param/lane/lane.yaml`の値を入力してから、キャリブレーションを開始します。 色相を低く調整します - 最初は高い値です。（1）色相値は色を意味し、`黄色`、`白`などのすべての色には、独自の色相値の領域があります（hsvマップを参照）。 次に、飽和度を低 - 高値に校正します。（2）すべての色には独自の彩度フィールドもあります。 最後に、明度を低 - 高値に調整します。（3）ただし、ソースコードには自動調整機能があるため、明度の低い値を校正しても意味がありません。 明度の高い値を255に設定するだけです。明確にフィルタリングされたライン画像により、レーンの明確な結果が得られます。
    {: .notice--success}

5. `Remote PC`のキャリブレーションファイルを上書きした後、`rqt_rconfigure`と`turtlebot3_autorace_detect_lane`を閉じて、次のように入力します

    ``` bash
    $ export AUTO_DT_CALIB=action
    $ roslaunch turtlebot3_autorace_detect turtlebot3_autorace_detect_lane.launch
    ```

6. 入力し、結果が正しいかどうかを確認します

    `Remote PC`

    ``` bash
    $ roslaunch turtlebot3_autorace_control turtlebot3_autorace_control_lane.launch
    ```

    `TurtleBot SBC`

    ``` bash
    $ roslaunch turtlebot3_bringup turtlebot3_robot.launch
    ```

    これらのコマンドを入力した後、ロボットはキックオフします。

#### [チュートリアル：5.2. 交通標識](#チュートリアル-52-交通標識)

1. 交通標識の検出には、交通標識の写真が必要です。`rqt_image_view`ノードを使用して写真を撮り、Linuxで利用可能な`フォトエディター`のいずれかを使用してサイズ、形状を編集します。ノードは `SIFTアルゴリズム`で交通標識を見つけるので、カスタマイズされた交通標識（`autorace_track`では導入されていません）を使用する場合は、`交通標識のエッジが多いほど、SIFTからの認識結果が向上します`。

2. ロボットをレーンに置きます。このとき、交通標識はロボットが見やすい場所に配置する必要があります。`turtlebot3_bringup`パッケージの`turtlebot3_robot`ノードがまだ起動されていないことを確認してください。走行中の場合、ロボットが突然軌道上を走行することがあります。

3. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ```
    $ rqt_image_view
    ```

    次に、選択ボックスで`/camera/image_compensated`トピックをクリックします。 すべてが正常に機能する場合、画面にはロボットからのビューが表示されます。

4.`リモートPC`で`alt` + `print screen`で写真を撮り、お好みのフォトエディターでキャプチャしたものを編集します。 その後、`/turtlebot3_autorace/turtlebot3_autorace_detect/file/detect_sign/`を配置したturtlebot3_autoraceパッケージに画像を配置し、必要に応じて名前を変更します。（ただし、デフォルトのファイル名を変更する場合は、ソース `detect_sign.py`に記述されているファイル名を変更する必要があります。）

5. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ roslaunch turtlebot3_autorace_detect turtlebot3_autorace_detect_sign.launch
    ```

6. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ```
    $ rqt_image_view
    ```

    次に、選択ボックスで`/detect/image_traffic_sign/compressed`トピックをクリックします。 すべてが正常に機能している場合、認識に成功すると、画面に交通標識検出の結果が表示されます。

#### [チュートリアル：5.3. 信号機](#チュートリアル-53-信号機)

1. ロボットをレーンに置きます。 ロボットを正しく配置した場合、`黄色い線`はロボットの左側に配置され、もちろん`白い線`はロボットの右側に配置されます。`turtlebot3_bringup`パッケージの` turtlebot3_robot`ノードがまだ起動されていないことを確認してください。 走行中の場合、ロボットは突然軌道上を走行します。

2. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_DT_CALIB=calibration
    $ roslaunch turtlebot3_autorace_detect turtlebot3_autorace_detect_traffic_light.launch
    ```

3. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ```
    $ rqt
    ```

    画面上部の`plugins` -> `visualization` -> `Image view`をクリックすると、カメラビューの監視機能が追加されます。 それにしたがって、rqtプレートに3つの追加モニターを作成します。次に、追加の各モニターで`/detect/image_red_light`、`/detect/image_yellow_light`、`/detect/image_green_light`、`/detect/image_traffic_light`トピックを選択します。すべてが正常に機能する場合、3つの画面には赤/黄/緑の光のフィルタリングされた画像が表示され、もう1つの画面には認識された色が短い文字列で表示されます。キャリブレーションモードでは、3つの画面に白が表示され、もう1つの画面にわかりやすい結果が表示される場合があります。ここから、正しい線と方向を表示するようにフィルターパラメーターを調整する必要があります。

4. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ rosrun rqt_reconfigure rqt_reconfigure
    ```

    次に、 `/ detect_traffic_light`のパラメーター値を調整します。 カラーフィルターの値を変更すると、各色の画面にフィルターされたビューの変更が表示されます。 その後、 `turtlebot3_autorace_detect/param/traffic_light/`の `traffic_light.yaml`ファイルに値を上書きします。 これにより、次回の起動からここで設定したとおりにパラメーターが設定されます。

    `ヒント`: [5.1. 車線検出][51-車線検出]と同様です。

5. `Remote PC`でキャリブレーションファイルを上書きした後、`rqt_rconfigure`と `turtlebot3_autorace_detect_traffic_light`を閉じて次を入力します。

    ``` bash
    $ export AUTO_DT_CALIB=action
    $ roslaunch turtlebot3_autorace_detect turtlebot3_autorace_detect_traffic_light.launch
    ```

6. `rqt_image_view`ノードを使用して、結果がうまくいくかどうかを確認します。

#### [チュートリアル：5.4. 駐車場](#チュートリアル-54-駐車場)

1. `駐車場`の準備は、交通標識認識の1つだけです。

2. ダミーロボットをどちらかの駐車場に置きます。

3. ロボットをレーンに適切に配置します。

#### [チュートリアル：5.5. 踏切](#チュートリアル-55-踏切)

1. 踏切は、レベル上で3つの赤い長方形を見つけ、レベルが開いているか閉じているか、およびロボットがどれだけ近づいているかを計算します。

2. ロボットをレーンに正しく配置します。 次に、ロボットを閉じたレベルの前に移動します。

3. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_DT_CALIB=calibration
    $ roslaunch turtlebot3_autorace_detect turtlebot3_autorace_detect_level.launch
    ```

4. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ```
    $ rqt
    ```

    画面上部の`plugins` -> `visualization` -> `Image view`をクリックすると、カメラビューの監視機能が追加されます。それにしたがって、rqtプレートに3つの追加モニターを作成します。 次に、追加の各モニターで`/detect/image_level_color_filtered`トピックと`/detect/image_level`トピックを選択します。すべてが正常に機能する場合、3つの画面に赤い長方形のフィルター処理された画像が表示され、別の画面に長方形を結ぶ線が描画されます。キャリブレーションモードでは、画面に白が表示され、他の画面にはわかりやすい結果が表示される場合があります。ここから、正しい線と方向を表示するようにフィルターパラメーターを調整する必要があります。

5. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ rosrun rqt_reconfigure rqt_reconfigure
    ```

    次に、 `/detect_level`のパラメーター値を調整します。 カラーフィルターの値を変更すると、各色の画面にフィルターされたビューの変更が表示されます。 その後、 `turtlebot3_autorace_detect/param/level/`の`level.yaml`ファイルに値を上書きします。 これにより、次回の起動からここで設定したとおりにパラメーターが設定されます。

    `ヒント`: [5.1. レーン検出][51-レーン検出]と同様です。

6. `REmote PC`でキャリブレーションファイルを上書きした後、`rqt_rconfigure`と `turtlebot3_autorace_detect_level`を閉じて、次のように入力します

    ``` bash
    $ export AUTO_DT_CALIB=action
    $ roslaunch turtlebot3_autorace_detect turtlebot3_autorace_detect_level.launch
    ```

7. `rqt_image_view`ノードを使用して、結果がうまくいくかどうかを確認します

#### [チュートリアル：5.6. トンネル](#チュートリアル-56-トンネル)

1. トンネルノードは、turtlebot3ナビゲーションパッケージを使用して、入口から出口まで移動します。 キャリブレーションする必要があるのは、トンネルをマッピングし（または、オートレーストラックをそのまま使用している場合は、自分で変更する必要はありません）、ロボットが出る直前にロボットのポーズの`ポーズ`を確認することです。 トンネルから（デフォルトのマップを使用している場合もこれは不要です）。

2. `Remote PC`、`SLAM`または`Navigation`パッケージの実行中に、RVizの`exit`の`pose`を確認します。 その後、`detect_tunnel.py`ファイル`144行目`に値を上書きします

### [チュートリアル：6. 自動運転を実行する](#チュートリアル-6-自動運転を実行する])

1. From now, all the related nodes will be run in `action mode`. Close all `ROS-related programs` and `terminals` on `Remote PC` and `TurtleBot SBC`, if some were not closed yet. Then, put the robot on the lane correctly.

1. これ以降、関連するすべてのノードは`アクションモード`で実行されます。一部がまだ閉じられていない場合は、`Remote PC``と`TurtleBotSBC`のすべての`ROS関連プログラム`と`ターミナル`を閉じます。 次に、ロボットをレーンに正しく配置します。

2. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ roscore
    ```


3. 新しいターミナルを`TurtleBot SBC`で開き、次ぎのように入力します。
    ``` bash
    $ roslaunch turtlebot3_bringup turtlebot3_robot.launch
    ```

4. 新しいターミナルを`TurtleBot SBC`で開き、次ぎのように入力します。

    ``` bash
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_camera_pi.launch
    ```

5. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_IN_CALIB=action
    $ roslaunch turtlebot3_autorace_camera turtlebot3_autorace_intrinsic_camera_calibration.launch
    ```

6. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ export AUTO_EX_CALIB=action
    $ export AUTO_DT_CALIB=action
    $ export TURTLEBOT3_MODEL=burger
    $ roslaunch turtlebot3_autorace_core turtlebot3_autorace_core.launch
    ```

7. 新しいターミナルを`Remote PC`で開き、次のように入力します。

    ``` bash
    $ rostopic pub -1 /core/decided_mode std_msgs/UInt8 "data: 2"
    ```

    turtlebot3_autorace_coreは、パッケージ内のすべてのシステムを制御します（パッケージ内の起動ノードを開いたり閉じたりします）。


### [TurtleBot3 AutoRace オンラインコンペティション](#TurtleBot3-AutoRace-オンラインコンペティション)

![](/assets/images/platform/turtlebot3/challenges/competition_autorace.png)

- [TurtleBot3 AutoRace](http://emanual.robotis.com/docs/en/platform/turtlebot3/challenges/#online-competition-on-rds)


[turtlebot3_autorace]: https://github.com/ROBOTIS-GIT/turtlebot3_autorace

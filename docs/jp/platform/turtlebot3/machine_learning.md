---
layout: archive
lang: jp
ref: machine_learning
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/machine_learning/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 27
---

<div style="counter-reset: h1 13"></div>

# [[ROS 1]機械学習](＃ros-1機械学習)

機械学習は、コンピューターに人や動物にとって自然なことを認識するように教えるデータ分析手法です。つまり、経験を通じて学習します。 機械学習には、教師あり学習、教師なし学習、強化学習の3種類があります。

このアプリケーションは、DQN（Deep Q-Learning）による強化学習です。 強化学習は、累積報酬の概念を最大化するために、ソフトウェアエージェントが環境内でどのように行動を起こすべきかに関するものです。

<iframe width="854" height="480" src="https://www.youtube.com/embed/WADmP0wzLxs" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

e-Manualの内容は、予告なしに更新される場合があります。 そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

これは、gazeboでTurtleBot3を使用した強化学習を示しています。
この強化学習は、LDSでDQN(Deep Q-Learning)アルゴリズムを適用します。
4段階の強化学習チュートリアルを準備しています。

## [インストール](＃インストール)
このチュートリアルを行うには、Ubuntu 16.04とROS kineticを使用してTensorflow、Keras、Anacondaをインストールする必要があります。

<iframe width="854" height="480" src="https://www.youtube.com/embed/s0qgunKt654" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### [Anaconda](#anaconda)
Python2.7バーション用のの[Anaconda 5.2](https://www.anaconda.com/download/#linux)をダウンロードできます。

Anacondaをダウンロード後、ダウンロードファイルを配置したディレクトリへ行き、次のコマンドを入力します。

``` bash
$ bash Anaconda2-x.x.x-Linux-x86_64.sh
```

Anacondaインストール後、
``` bash
$ source ~/.bashrc
$ python -V
```
Anacondaがインストールされている場合は、`Python 2.7.xx :: Anaconda, Inc.`が表示されます。

### [ROS依存関係パッケージ](＃ROS依存関係パッケージ)
ROSとAnacondaを一緒に使用するには、ROS依存関係パッケージを追加でインストールする必要があります。
``` bash
$ pip install -U rosinstall msgpack empy defusedxml netifaces
```

### [Tensorflow](#tensorflow)
[TensorFlow](https://www.tensorflow.org/install/)をインストールできます。
``` bash
$ conda create -n tensorflow pip python=2.7
```
このチュートリアルは、python2.7(CPUのみ)で使用されます。別のPythonバージョンとGPUを使用する場合は、[TensorFlow](https://www.tensorflow.org/install/)を参照してください。
``` bash
$ pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.8.0-cp27-none-linux_x86_64.whl
```

### [Keras](#keras)
[Keras](https://keras.io/)は、Pythonで記述された高レベルのニューラルネットワークAPIであり、TensorFlow上で実行できます。
``` bash
$ pip install keras==2.1.5
```

### [機械学習パッケージ](#機械学習パッケージ)
**警告**: このパッケージをインストールする前に[turtlebot3](https://github.com/ROBOTIS-GIT/turtlebot3),[turtlebot3_msgs](https://github.com/ROBOTIS-GIT/turtlebot3_msgs)および [turtlebot3_simulations](https://github.com/ROBOTIS-GIT/turtlebot3_simulations)パッケージをインストールしてください。
{: .notice--warning}

``` bash
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_machine_learning.git
$ cd ~/catkin_ws && catkin_make
```
## [パラメータの設定](#パラメータの設定)

DQN Agentの目標は、障害物を回避してTurtleBot3を目標に到達させることです。 TurtleBot3が目標に近づくと、正の報酬を受け取り、遠くに近づくと、負の報酬を受け取ります。
エピソードは、TurtleBot3が障害物に衝突したとき、または一定期間後に終了します。 エピソード中、TurtleBot3はゴールに到達すると大きなプラスの報酬を受け取り、TurtleBot3は障害物に衝突すると大きなマイナスの報酬を受け取ります。

<iframe width="1236" height="695" src="https://www.youtube.com/embed/807_cByUBSI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

e-Manualの内容は、予告なしに更新される場合があります。 そのため、一部の動画はe-Manualの内容と異なる場合があります。
{: .notice--warning} 

### [状態の設定](#状態の設定)
状態は環境の観察であり、現在の状況を説明します。ここで、`state_size`は26、24のLDS値、ゴールまでの距離、ゴールまでの角度があります。

Turtlebot3のLDSのデフォルトは360に設定されています。LDSのサンプルは `turtlebot3/turtlebot3_description/urdf/turtlebot3_burger.gazebo.xacro`で変更できます。

``` bash
<xacro:arg name="laser_visual" default="false"/>   # Visualization of LDS. If you want to see LDS, set to `true`
```
``` bash
<scan>
  <horizontal>
    <samples>360</samples>            # The number of sample. Modify it to 24
    <resolution>1</resolution>
    <min_angle>0.0</min_angle>
    <max_angle>6.28319</max_angle>
  </horizontal>
</scan>
```

|![](/assets/images/platform/turtlebot3/machine_learning/sample_360.png)|![](/assets/images/platform/turtlebot3/machine_learning/sample_24.png)|
|:--:|:--:|
|**sample = 360**|**sample = 24**|

### [アクションの設定](＃set-action)
アクションは、エージェントが各状態で実行できることです。 ここで、turtlebot3の線速度は常に0.15 m/sです。 角速度は作用によって決定されます。

|アクション|角速度(rad/s)|
|:--:|:--:|
|0   |-1.5|
|1   |-0.75|
|2   |0   |
|3   |0.75|
|4   |1.5 |

### [報酬の設定](#報酬の設定)
turtlebot3がある状態でアクションを実行すると、報酬を受け取ります。 報酬のデザインは学習にとって非常に重要です。 報酬はプラス/マイナスの両方になり得ます。 turtlebot3がゴールに到達すると、大きなプラスの報酬が得られます。 turtlebot3が障害物と衝突すると、大きなマイナスの報酬が得られます。 報酬デザインを適用する場合は、 `/turtlebot3_machine_learning/turtlebot3_dqn/src/turtlebot3_dqn/environment_stage_＃.py`にある`setReward`関数を変更します。

### [Set hyper parameters](#set-hyper-parameters)
This tutorial has been learned using DQN. DQN is a reinforcement learning method that selects a deep neural network by approximating the action-value function(Q-value). Agent has follow hyper parameters at `/turtlebot3_machine_learning/turtlebot3_dqn/nodes/turtlebot3_dqn_stage_#`.

### [ハイパーパラメータの設定](#ハイパーパラメータの設定)
このチュートリアルは、DQNを使用して学習しました。 DQNは、アクション値関数（Q値）を近似することによってディープニューラルネットワークを選択する強化学習法です。 エージェントは、`/turtlebot3_machine_learning/turtlebot3_dqn/nodes/turtlebot3_dqn_stage_＃`にあるハイパーパラメータに従います。

|ハイパーパラメータ|デフォルト|説明|
|：-：|：-：|：-：|
| Episode_step    |   6000    | 1つのエピソードのタイムステップ|
| target_update   |   2000    | ターゲットネットワークの更新レート|
| discount_factor |   0.99    | 距離に応じて、将来のイベントがどれだけ価値を失うかを表します|
| Learning_rate   |   0.00025 | 学習速度。値が大きすぎると学習がうまくいかず、小さすぎると学習時間が長くなります|
|epsilon          |   1.0     | ランダムなアクションを選択する確率|
| epsilon_decay   |   0.99    | イプシロンの減少率。1つのエピソードが終了すると、イプシロンは減少します|
| epsilon_min     |   0.05    | イプシロンの最小値|
| batch_size      |   64      | トレーニングサンプルのグループのサイズ|
| train_start     |   64      | リプレイメモリサイズが64より大きい場合は、トレーニングを開始します|
|メモリ            |   1000000 | リプレイメモリのサイズ|

## [機械学習の実行](#機械学習の実行])
<iframe width="1280" height="720" src="https://www.youtube.com/embed/5uIZU8PCHT8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### [ステージ1 (障害物なし)](#ステージ1-障害物なし)
ステージ1は、障害物のない4x4のマップです。

![](/assets/images/platform/turtlebot3/machine_learning/stage_1.jpg)
``` bash
$ roslaunch turtlebot3_gazebo turtlebot3_stage_1.launch
$ roslaunch turtlebot3_dqn turtlebot3_dqn_stage_1.launch
```

### [ステージ2 (静的障害物)](#ステージ2-静的障害物)
ステージ2は、4つのシリンダーの静的障害物がある4x4マップです。

![](/assets/images/platform/turtlebot3/machine_learning/stage_2.jpg)
``` bash
$ roslaunch turtlebot3_gazebo turtlebot3_stage_2.launch
$ roslaunch turtlebot3_dqn turtlebot3_dqn_stage_2.launch
```

### [ステージ3 (移動障害物)](#ステージ3-移動障害物)
ステージ2は、移動する4つのシリンダーの障害物を含む4x4マップです。

![](/assets/images/platform/turtlebot3/machine_learning/stage_3.jpg)
``` bash
$ roslaunch turtlebot3_gazebo turtlebot3_stage_3.launch
$ roslaunch turtlebot3_dqn turtlebot3_dqn_stage_3.launch
```
### [ステージ4(組み合わせの障害)](#ステージ4-組み合わせの障害物)
ステージ4は、壁と2つの移動するシリンダーの障害物を備えた5x5のマップです。

![](/assets/images/platform/turtlebot3/machine_learning/stage_4.jpg)
``` bash
$ roslaunch turtlebot3_gazebo turtlebot3_stage_4.launch
$ roslaunch turtlebot3_dqn turtlebot3_dqn_stage_4.launch
```

グラフをみたい場合は、グラフ起動ファイルを起動してください。

``` bash
$ roslaunch turtlebot3_dqn result_graph.launch
```

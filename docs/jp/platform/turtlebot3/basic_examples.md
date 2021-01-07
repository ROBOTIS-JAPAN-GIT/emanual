---
layout: archive
lang: jp
ref: basic_examples
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/basic_examples/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 18
---

<div style="counter-reset: h1 8"></div>
<div style="counter-reset: h2 2"></div>

<!--[dummy Header 1]>
  <h1 id="basic-operation"><a href="#basic-operation">Basic Operation</a></h1>
<![end dummy Header 1]-->

## [基本的な例](#basic-examples)

**警告**: この例を実行する前に必ず[Bringup][bringup]コマンドを実行し、テーブルの上でテストをする場合にロボットが落下する可能性があるため注意してください。
{: .notice--warning}

{% capture notice_01 %}
**注釈**:
- このコマンドは`Ubuntu 16.04` と `ROS Kinetic Kame` で動作確認をしました。
- このコマンドはリモートPC上で実行されることを前提としています。使用している **リモートPC** の指示にしたがってください。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

{% capture notice_02 %}
{% include en/platform/turtlebot3/ros_book_info.md %}
{% endcapture %}
<div class="notice--success">{{ notice_02 | markdownify }}</div>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Xg1pKFQY5p4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

e-Manualの内容は予告なしで更新されることがあります。そのため、一部の映像はe-Manualの内容と異なる場合があります。
{: .notice--warning}

### [インタラクティブマーカーを用いた移動](#move-using-interactive-markers)

TurtleBot3はRViz上の[インタラクティブマーカー][interactive_markers]によって動かすことができます。インタラクティブマーカーを使って、TurtleBot3を回転や直線に移動させることができます。

**[リモートPC]** 新しいターミナルを開き、リモートファイルを起動します。

**ヒント**: このコマンドを実行する前に、TurtleBot3のモデル名を指定する必要があります。`${TB3_MODEL}` は `burger`、`waffle`、`waffle_pi` のうちの使用しているモデルの名前です。このエクスポート設定を次回以降にも続けて使用したい場合には、[export_turtlebot3_model][export_turtlebot3_model]{: .popup}ページを参照してください。
{: .notice--success}

**ヒント**: ターミナルアプリは、画面左上のUbuntuの検索アイコンで見つけることができます。ターミナルのショートカットキーは`Ctrl`-`Alt`-`T`です。
{: .notice--info}

``` bash
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ roslaunch turtlebot3_bringup turtlebot3_remote.launch
```

**[リモートPC]** interactive markers fileを起動します。
``` bash
$ roslaunch turtlebot3_example interactive_markers.launch
```

**[リモートPC]** RVizを使用してモデルを3Dで可視化します。
``` bash
$ rosrun rviz rviz -d `rospack find turtlebot3_example`/rviz/turtlebot3_interactive.rviz
```

### [障害物検知](#obstacle-detection)

TurtleBot3は、LDSのデータによって移動・停止できます。TurtleBot3が移動している際に、前方の障害物を検知した場合は停止します。

**[リモートPC]** obstacle fileを起動します。
``` bash
$ roslaunch turtlebot3_example turtlebot3_obstacle.launch
```

### [座標指令](#point-operation)

TurtleBot3は、2次元上の`点(x, y)`と`z-angular`で移動することができる。たとえば、(0.5, 0.3, 60)を入れると、TurtleBot3は点（x = 0.5m, y = 0.3m）に移動し、60度回転します。

**[リモートPC]** pointop fileを起動します。
``` bash
$ roslaunch turtlebot3_example turtlebot3_pointop_key.launch
```

### [パトロール](#patrol)

TurtleBot3はカスタムルートを使って移動できます。ルートは、長方形、三角形、円の3種類があります。この例では、action topicを使用しています。Action clientは、パトロールデータ（モード、エリア、カウント）をaction serverに転送します。action clientはパトロールデータ（mode, area, count）をaction serverに転送し、アクションサーバは `cmd_vel` をTurtleBot3に転送します。詳しい使い方は上記の[tutorial video][tutorial_video]を参照してください。

**[リモートPC]** patrol server fileを起動します。
``` bash
$ rosrun turtlebot3_example turtlebot3_server
```
**[リモートPC]** patrol client fileを起動します。
``` bash
$ roslaunch turtlebot3_example turtlebot3_client.launch
```

[bringup]: /docs/en/platform/turtlebot3/bringup/#bringup
[interactive_markers]: http://wiki.ros.org/interactive_markers
[tutorial_video]: https://youtu.be/Xg1pKFQY5p4
[export_turtlebot3_model]: /docs/en/platform/turtlebot3/export_turtlebot3_model

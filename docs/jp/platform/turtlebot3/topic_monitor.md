---
layout: archive
lang: jp
ref: topic_monitor
read_time: true
share: true
author_profile: false
permalink: /docs/en/platform/turtlebot3/topic_monitor/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 16
---

<div style="counter-reset: h1 8"></div>
<div style="counter-reset: h2 0"></div>

<!--[dummy Header 1]>
  <h1 id="basic-operation"><a href="#basic-operation">Basic Operation</a></h1>
<![end dummy Header 1]-->

## [Topic Monitor](#topic-monitor)

**警告**: ロボットをテーブルの上で動かす際には、ロボットが落下する可能性があるため注意してください。
{: .notice--warning}

{% capture notice_01 %}
**注意**:
- このコマンドは、`Ubuntu 16.04` と `ROS Kinetic Kame` で動作確認をしました。
- このコマンドは、リモートPC上で実行されることを想定しています。以下の手順を **リモートPC** で実行してください。
- コマンドを使用する前に、必ず[Bringup](/docs/en/platform/turtlebot3/bringup/#bringup)のコマンドを実行してください。
{% endcapture %}
<div class="notice--info">{{ notice_01 | markdownify }}</div>

TurtleBot3のtopicを確認するために、ROSが提供している[rqt][rqt]を利用します。rqtは、QtベースのROSのGUI開発用フレームワークです。rqtは、topic listにすべてのtopicを表示することによりユーザーが簡単にtopicの状態を確認できるツールです。GUIでは、topic名、種類、帯域幅、Hz、値があります。

**[Remote PC]** rqtを実行してください。
``` bash
$ rqt
```
![](/assets/images/platform/turtlebot3/example/rqt_1.png)

**注釈**: rqtが表示されない場合は、`plugin` -> `Topics` -> `Topic Monitor`を選択してください。
{: .notice--info}を選択してください。

rqtを最初に実行すると、topicの値はモニターされません。topicをモニタリングするには、各トピックの横にあるチェックボックスをクリックします。

![](/assets/images/platform/turtlebot3/example/rqt_2.png)

より詳細なトピックメッセージを表示したい場合は、各チェックボックスの横にある `▶` ボタンをクリックしてください。

![](/assets/images/platform/turtlebot3/example/rqt_3.png)


- `/battery_state` は、現在のバッテリーの電圧や残量など、バッテリーの状態に関するメッセージを表示します。

![](/assets/images/platform/turtlebot3/example/rqt_4.png)

- `/diagnostics` は、MPU9250、DYNAMIXEL-X、HLS-LFCD-LDS、バッテリー、OpenCRなど、TurtleBot3に接続されているコンポーネントの状態をメッセージで表示します。

![](/assets/images/platform/turtlebot3/example/rqt_5.png)

- `/odom` は、TurtleBot3のオドメトリを示すメッセージです。このtopicはエンコーダ値による方位と位置を持っています。

![](/assets/images/platform/turtlebot3/example/rqt_6.png)

- `/sensor_state` は、エンコーダ値、バッテリー、トルクをメッセージで表示しています。

![](/assets/images/platform/turtlebot3/example/rqt_7.png)

- `/scan` は、angle_maxとmin、range_maxとmin等のすべてのLDSデータが範囲を示すメッセージです。
![](/assets/images/platform/turtlebot3/example/rqt_8.png)

さらに、topicを追加するたびに、rqtを介してトピックをモニタリングできます。

[bringup]: /docs/en/platform/turtlebot3/bringup/#bringup
[rqt]: http://wiki.ros.org/rqt

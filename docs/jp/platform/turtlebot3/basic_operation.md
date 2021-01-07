---
layout: archive
lang: jp
ref: basic_operation
read_time: true
share: true
author_profile: false
permalink: /docs/jp/platform/turtlebot3/basic_operation/
sidebar:
  title: TurtleBot3
  nav: "turtlebot3"
product_group: turtlebot3
page_number: 15
---

<div style="counter-reset: h1 7"></div>

# [[ROS 1] 基本操作](#ros-1-基本操作)

{% capture notice_02 %}
{% include en/platform/turtlebot3/ros_book_info.md %}
{% endcapture %}
<div class="notice--success">{{ notice_02 | markdownify }}</div>

## [トピックモニター](#トピックモニター)
- [トピックモニター][topic_monitor]：RQTのTopic Monitorプラグインを使用して、TurtleBot3の全トピックを表示します。

## [遠隔操作](#teleoperation)
- [遠隔操作][teleoperation]：PS3、Xbox360、ROBOTIS RC100などの無線機器を使ってTurtleBot3を遠隔操作します。

<iframe width="640" height="360" src="https://www.youtube.com/embed/Z4s18hlazb4" frameborder="0" allowfullscreen></iframe>

## [基本的な例](#基本的な例)
- [基本的な例][basic_examples]：基本的な例には、以下に示すような様々なコンテンツがあります。

  - RViz上でのインタラクティブマーカーを使った移動。
  - LDSを使った移動と停止。
  - ゴール位置までの移動。
  - カスタム経路を移動。

<iframe width="560" height="315" src="https://www.youtube.com/embed/Xg1pKFQY5p4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

e-Manualの内容は、予告なしで更新される場合があります。そのため、いくつかの動画はe-Manualの内容と異なる場合があります。

{: .notice--warning}

## [センサの追加](#センサの追加)
- [センサの追加][additional_sensors]：IRセンサ、超音波センサ、スイッチ等をTurtleBot3のOpenCRで使いましょう。

[topic_monitor]: /docs/en/platform/turtlebot3/topic_monitor/
[teleoperation]: /docs/en/platform/turtlebot3/teleoperation/
[basic_examples]: /docs/en/platform/turtlebot3/basic_examples/
[additional_sensors]: /docs/en/platform/turtlebot3/additional_sensors/

---
layout: archive
lang: en
ref: opencr10
read_time: true
share: true
author_profile: false
permalink: /docs/en/parts/controller/opencr10/
sidebar:
  title: OpenCR1.0
  nav: "opencr10"
---

![](/assets/images/parts/controller/opencr10/opencr_product.png)

> OpenCR 1.0

# [イントロダクション](#introduction)
OpenCR1.0は、完全にオープンソースのハードウェアとソフトウェアを提供するために、ROSの組み込みシステム用に開発されました。  
ボードの回路図、PCBカバー、BOM、TurtleBot3とOP3用のファームウェアのソースコードなど、ボードに関するすべてのものは、ユーザーとROSコミュニティのためにオープンソースライセンスのもとで無料で配布されています。  
OpenCR1.0ボード内のSTM32F7シリーズチップは、浮動小数点演算ユニットを搭載した非常に強力なARM Cortex-M7をベースにしています。  
OpenCR1.0の開発環境は、若い学生向けのArduino IDEやScratchから、エキスパート向けの従来のファームウェア開発まで幅広く対応しています。  

# [仕様表](#specifications)

| 項目                | 仕様                                                                                                                                                                                                                            |
|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| マイクロコントローラー      | STM32F746ZGT6 / 32-bit ARM Cortex®-M7 with  FPU (216MHz, 462DMIPS)<br />[Reference Manual], [Datasheet]                                                                                                                                   |
| センサー              | (**廃止**) 三軸ジャイロ, 三軸加速度, 三軸磁気センサ (MPU9250)<br /> (**新設**) 三軸ジャイロ, 3軸加速度, デジタルモーションプロセッサー™ (ICM-20648)                                            |
| プログラマー           | ARM Cortex 10ピン JTAG/SWD コネクター<br />USBデバイスファームウェア アップグレード (DFU)<br />シリアル                                                                                                                                                    |
| デジタルI/O          | 32ピン (L 14, R 18) *Arduino connectivity<br />5ピン OLLO x 4<br />GPIO x 18ピン<br />PWM x 6<br />I2C x 1<br />SPI x 1                                                                                                                  |
| アナログ入力         | ADCチャンネル (最大 12bit) x 6                                                                                                                                                                                                              |
| 通信ポート  | USB x 1 (マイクロUSBタイプB/USB 2.0/Host/Peripheral/OTG)<br />TTL x 3 ([B3B-EH-A] / DYNAMIXEL)<br />RS485 x 3 ([B4B-E名称H-A] / DYNAMIXEL)<br />UART x 2 ([20010WS-04])<br />CAN x 1 ([20010WS-04])                                        |
| LEDとボタン     | LD2 (red/green) : USB通信<br />ユーザーLED x 4 : LD3 (赤), LD4 (緑), LD5 (青)<br />ユーザーボタン  x 2<br />パワーLED : LD1 (赤, 3.3 V 電源オン)<br />リセットボタン x 1 (ボードの電源リセット用)<br />電源オン/オフ スイッチ x 1 |
| 入力電源  | 5 V (USB VBUS), 7-24 V (バッテリーもしくはSMPS)<br />標準バッテリー : LI-PO 11.1V 1,800mAh 19.98Wh<br />標準SMPS : 12V 4.5A<br />RTC（リアルタイムクロック）用外部バッテリー ([Molex 53047-0210])                                       |
| 出力電源 | <sup>`*`</sup>12V 最大 4.5A([SMW250-02])<br /><sup>`*`</sup>5V 最小 4A([5267-02A]), 3.3V@800mA([20010WS-02])                                                                                                                                |
| 寸法           | 105(幅) X 75(奥行き) mm                                                                                                                                                                                                                         |
| 重量               | 60g                                                                                                                                                                                                                                       |


**注釈**: 2020年以降、MPU9250センサーはICM-20648に置き換えられました。MPU9250は生産が中止されているためです。  
{: .notice}

<sup>`*`</sup> 5V電源は、安定化された12V出力から供給されます。  
{: .notice}

**注釈** : OpenCR1.0ボードからの"ショアパワー"(12V, 4.5A SMPS)と"モバイルパワー"(バッテリー)のホットスワップ電源切り替えにより、UPS(無停電電源)機能を有効にすることができます。  
{: .notice}

# [レイアウト/ピン配置](#layoutpin-map)

![](/assets/images/parts/controller/opencr10/opencr_pinout.png)

{% include en/dxl/pinout_warning.md %}

## [Arduinoコネクター](#arduino-connector)
OpenCRにはArduino Unoのピンに対応したコネクタが付属しています。  
0～21番のピンはArduino Unoと同じピンで、それ以降はOpenCRに追加されたピンにマッピングされています。  

![](/assets/images/parts/controller/opencr10/arduino_pinmap_01.png)

| ピン番号 |    関数   |     1     |     2     |   3    | etc  |
|:-------:|:--------:|:---------:|:---------:|:------:|:----:|
|    0    | UART RXD | UART6_RX  |           |        | `FT` |
|    1    | UART TXD | UART6 TX  |           |        | `FT` |
|    2    |          |           |           | EXTI_0 | `FT` |
|    3    |   PWM    | TIM3_CH1  |           | EXTI_1 | `FT` |
|    4    |          |           |           | EXTI_2 | `FT` |
|    5    |   PWM    | TIM1_CH1  |           |        | `FT` |
|    6    |   PWM    | TIM2_CH3  |           |        | `FT` |
|    7    |          |           |           | EXTI_3 | `FT` |
|    8    |          |           |           | EXTI_4 | `FT` |
|    9    |   PWM    | TIM9_CH2  |           |        | `FT` |
|   10    | PWM/NSS  | TIM11_CH1 | SPI2_NSS  |        | `FT` |
|   11    | PWM/MOSI | TIM12_CH2 | SPI2_MOSI |        | `FT` |
|   12    |   MISO   |           | SPI2_MISO |        | `FT` |
|   13    |   SCK    |           | SPI2_SCK  |        | `FT` |
|   14    |   SDA    |           | I2C1_SDA  |        | `FT` |
|   15    |   SCL    |           | I2C1_SCL  |        | `FT` |
|   16    |   ADC    |    A0     |           |        | `FT` |
|   17    |   ADC    |    A1     |           |        | `FT` |
|   18    |   ADC    |    A2     |           |        | `FT` |
|   19    |   ADC    |    A3     |           |        | `FT` |
|   20    |   ADC    |    A4     |           |        | `FT` |
|   21    |   ADC    |    A5     |           |        | `FT` |

`FT`ピンはアナログモード時を除き、5Vトレラントです。FTピンの最大注入電流は **-5mA** です。また、全I/O端子を合計した総出力電流は、**120mA / -120mA** となります。  
{: .notice}

## [ユーザーLED](#user-led)
OpenCRの追加LEDは4つのLEDで構成されており、Arduinoのピン22-25にマッピングされています。  

![](/assets/images/parts/controller/opencr10/arduino_pinmap_03.png)

|  名称    | Arduinoピン  | ピンの名称         |
|:--------|:------------|:-----------------|
| USER1   | 22          | BDPIN_LED_USER_1 |
| USER2   | 23          | BDPIN_LED_USER_2 |
| USER3   | 24          | BDPIN_LED_USER_3 |
| USER4   | 25          | BDPIN_LED_USER_4 |
| STS     | 36          | BDPIN_LED_STATUS |
| Arduino | 13          | LED_BUILTIN      |

## [ディップスイッチ](#dip-switch)

![](/assets/images/parts/controller/opencr10/arduino_pinmap_04.png)

| Arduinoピン | ピンの名称       |
|:------------|:---------------|
| 26          | BDPIN_DIP_SW_1 |
| 27          | BDPIN_DIP_SW_2 |

## [GPIO](#pgio)
18ピンの共通GPIO拡張コネクタを持ち、ArduinoのGPIOピンにマッピングされています。下のピン番号はArduinoのピン番号です。  

![](/assets/images/parts/controller/opencr10/arduino_pinmap_05.png)

| ピン番号 | Arduinoピン | ピンの名称      | ピン番号 | Arduinoピン | ピンの名称      | etc  |
|:-----------|:------------|:--------------|:-----------|:------------|:--------------|:-----|
| 1          | -           | 3.3V          | 2          | -           | GND           | -    |
| 3          | 50          | BDPIN_GPIO_1  | 4          | 51          | BDPIN_GPIO_2  | `FT` |
| 5          | 52          | BDPIN_GPIO_3  | 6          | 53          | BDPIN_GPIO_4  | `FT` |
| 7          | 54          | BDPIN_GPIO_5  | 8          | 55          | BDPIN_GPIO_6  | `FT` |
| 9          | 56          | BDPIN_GPIO_7  | 10         | 57          | BDPIN_GPIO_8  | `FT` |
| 11         | 58          | BDPIN_GPIO_9  | 12         | 59          | BDPIN_GPIO_10 | `FT` |
| 13         | 60          | BDPIN_GPIO_11 | 14         | 61          | BDPIN_GPIO_12 | `FT` |
| 15         | 62          | BDPIN_GPIO_13 | 16         | 63          | BDPIN_GPIO_14 | `FT` |
| 17         | 64          | BDPIN_GPIO_15 | 18         | 65          | BDPIN_GPIO_16 | `FT` |
| 19         | 66          | BDPIN_GPIO_17 | 20         | 67          | BDPIN_GPIO_18 | `FT` |

`FT`ピンはアナログモード時を除き、5Vトレラントです。FTピンの最大注入電流 **-5mA** です。また、全I/Oピンを合計した総出力電流は、 **120mA / -120mA** となります。  
{: .notice}

**注釈** : プルアップ/プルダウンの代表的な抵抗は40kΩです。  
{: .notice}

## [ROBOTIS 5-ピン コネクター](#robotis-5-pin-connector)

![](/assets/images/parts/controller/opencr10/arduino_pinmap_06.png)

## [プッシュスイッチ](#push-switch)

![](/assets/images/parts/controller/opencr10/arduino_pinmap_08.png)

| Arduinoピン | ピンの名称        |
|:------------|:----------------|
| 34          | BDPIN_PUSH_SW_1 |
| 35          | BDPIN_PUSH_SW_2 |

## [外部割り込み](#external-interrupt)
外部割込みは以下のピンに割り当てられており、 *attachInterrupt(EXTI_Pin, callbackFunction, Mode)* マクロで使用することができます。  

| EXTI ピン | Arduinoピン | ピンの名称    |
|:---------|:------------|:------------|
| 0        | 2           | -           |
| 1        | 3           | TIM3_CH1    |
| 2        | 4           | -           |
| 3        | 7           | -           |
| 4        | 8           | -           |
| 5        | 42          | OLLO_P1_ADC |
| 6        | 45          | OLLO_P2_ADC |
| 7        | 72          | OLLO_P3_ADC |
| 8        | 75          | OLLO_P4_ADC |

```c
/*
  EXTI_0 is assigned to Arduino PIN 2
*/
pinmode(2, INPUT_PULLDOWN); //set Arduino Pin 2 as input with pull-down
attachInterrupt(0, changeDirection_EXIT_0, RISING);

void changeDirection_EXIT_0(void){
  Serial.println("EXIT_Interrupt! 0");
}
```

## [UART(Serial)](#uartserial)

| Classインスタンス     | Arduinoピン   |ハードウェア|
|:--------------------|:-------------|:---------|
| Serial              | USB          | USB      |
| Serial1             | 0(RX), 1(TX) | USART6   |
| Serial2 (SerialBT1) | UART1        | USART2   |
| Serial3             | DXL Port     | USART3   |
| Serial4 (SerialBT2) | UART2        | UART8    |

**注意**: DYNAMIXELではSerial3を使用しているため、他のシリアルとは使い方が異なります。（詳細はDYNAMIXEL Workbenchを参照してください）。  
{: .notice--warning}


## [ピンの定義](#pin-definition)

```c++
extern const Pin2PortMapArray g_Pin2PortMapArray[]=
{
  {GPIOC, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 0  UART6_RX
  {GPIOC, GPIO_PIN_6,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 1  UART6_TX
  {GPIOG, GPIO_PIN_6,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , 0       },  // 2              EXTI_0
  {GPIOB, GPIO_PIN_4,   NULL,     NO_ADC        , &hTIM3 ,   TIM_CHANNEL_1, 1       },  // 3  TIM3_CH1    EXTI_1
  {GPIOG, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , 2       },  // 4              EXTI_2
  {GPIOA, GPIO_PIN_8,   NULL,     NO_ADC        , &hTIM1 ,   TIM_CHANNEL_1, NO_EXTI },  // 5  TIM1_CH1
  {GPIOA, GPIO_PIN_2,   NULL,     NO_ADC        , &hTIM2 ,   TIM_CHANNEL_3, NO_EXTI },  // 6  TIM2_CH3
  {GPIOC, GPIO_PIN_1,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , 3       },  // 7              EXTI_3
  {GPIOC, GPIO_PIN_2,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , 4       },  // 8              EXTI_4
  {GPIOA, GPIO_PIN_3,   NULL,     NO_ADC        , &hTIM9 ,   TIM_CHANNEL_2, NO_EXTI },  // 9  TIM9_CH2
  {GPIOB, GPIO_PIN_9,   NULL,     NO_ADC        , &hTIM11,   TIM_CHANNEL_1, NO_EXTI },  // 10 TIM11_CH1   SPI2_NSS
  {GPIOB, GPIO_PIN_15,  NULL,     NO_ADC        , &hTIM12,   TIM_CHANNEL_2, NO_EXTI },  // 11 TIM12_CH2   SPI2_MOSI
  {GPIOB, GPIO_PIN_14,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 12             SPI2_MISO
  {GPIOA, GPIO_PIN_9,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 13 LED         SPI2_SCK
  {GPIOB, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 14             I2C1_SDA
  {GPIOB, GPIO_PIN_8,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 15             I2C1_SCL

  {GPIOA, GPIO_PIN_0,   &hADC3,   ADC_CHANNEL_0 , NULL   ,   NO_PWM       , NO_EXTI },  // 16 A0
  {GPIOF, GPIO_PIN_10,  &hADC3,   ADC_CHANNEL_8 , NULL   ,   NO_PWM       , NO_EXTI },  // 17 A1
  {GPIOF, GPIO_PIN_9,   &hADC3,   ADC_CHANNEL_7 , NULL   ,   NO_PWM       , NO_EXTI },  // 18 A2
  {GPIOF, GPIO_PIN_8,   &hADC3,   ADC_CHANNEL_6 , NULL   ,   NO_PWM       , NO_EXTI },  // 19 A3
  {GPIOF, GPIO_PIN_7,   &hADC3,   ADC_CHANNEL_5 , NULL   ,   NO_PWM       , NO_EXTI },  // 20 A4
  {GPIOF, GPIO_PIN_6,   &hADC3,   ADC_CHANNEL_4 , NULL   ,   NO_PWM       , NO_EXTI },  // 21 A5

  {GPIOG, GPIO_PIN_12,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 22 BDPIN_LED_USER_1
  {GPIOE, GPIO_PIN_5,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 23 BDPIN_LED_USER_2
  {GPIOE, GPIO_PIN_4,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 24 BDPIN_LED_USER_3
  {GPIOG, GPIO_PIN_10,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 25 BDPIN_LED_USER_4
  {GPIOG, GPIO_PIN_11,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 26 BDPIN_DIP_SW_1
  {GPIOE, GPIO_PIN_6,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 27 BDPIN_DIP_SW_2
  {GPIOA, GPIO_PIN_4,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 28 BDPIN_SPI_CS_IMU
  {GPIOC, GPIO_PIN_0,   &hADC3,   ADC_CHANNEL_10, NULL   ,   NO_PWM       , NO_EXTI },  // 29 BDPIN_BAT_PWR_ADC
  {GPIOC, GPIO_PIN_3,   &hADC3,   ADC_CHANNEL_13, NULL   ,   NO_PWM       , NO_EXTI },  // 30
  {GPIOF, GPIO_PIN_14,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 31 BDPIN_BUZZER
  {GPIOF, GPIO_PIN_15,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 32 BDPIN_DXL_PWR_EN
  {GPIOG, GPIO_PIN_14,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 33
  {GPIOG, GPIO_PIN_3,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 34 BDPIN_PUSH_SW_1
  {GPIOC, GPIO_PIN_12,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 35 BDPIN_PUSH_SW_2
  {GPIOG, GPIO_PIN_9,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 36 BDPIN_LED_STATUS
  {GPIOA, GPIO_PIN_5,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 37 BDPIN_SPI_CLK_IMU
  {GPIOA, GPIO_PIN_6,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 38 BDPIN_SPI_SDO_IMU
  {GPIOB, GPIO_PIN_5,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 39 BDPIN_SPI_SDI_IMU

  {GPIOB, GPIO_PIN_0,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 40 OLLO_P1_SIG1
  {GPIOC, GPIO_PIN_8,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 41 OLLO_P1_SIG2
  {GPIOA, GPIO_PIN_7,   &hADC1,   ADC_CHANNEL_7 , NULL   ,   NO_PWM       , 5       },  // 42 OLLO_P1_ADC    EXTI_5
  {GPIOC, GPIO_PIN_5,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 43 OLLO_P2_SIG1
  {GPIOB, GPIO_PIN_1,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 44 OLLO_P2_SIG2
  {GPIOC, GPIO_PIN_4,   &hADC1,   ADC_CHANNEL_14, NULL   ,   NO_PWM       , 6       },  // 45 OLLO_P2_ADC    EXTI_6
  {GPIOD, GPIO_PIN_10,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 46 OLLO_SLEEP
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 47
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 48
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 49

  {GPIOB, GPIO_PIN_10,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 50 BDPIN_GPIO_1
  {GPIOB, GPIO_PIN_11,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 51 BDPIN_GPIO_2
  {GPIOC, GPIO_PIN_13,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 52 BDPIN_GPIO_3
  {GPIOD, GPIO_PIN_2,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 53 BDPIN_GPIO_4
  {GPIOE, GPIO_PIN_3,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 54 BDPIN_GPIO_5
  {GPIOG, GPIO_PIN_2,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 55 BDPIN_GPIO_6
  {GPIOE, GPIO_PIN_10,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 56 BDPIN_GPIO_7
  {GPIOE, GPIO_PIN_11,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 57 BDPIN_GPIO_8
  {GPIOE, GPIO_PIN_12,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 58 BDPIN_GPIO_9
  {GPIOE, GPIO_PIN_13,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 59 BDPIN_GPIO_10
  {GPIOE, GPIO_PIN_14,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 60 BDPIN_GPIO_11
  {GPIOE, GPIO_PIN_15,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 61 BDPIN_GPIO_12
  {GPIOF, GPIO_PIN_0,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 62 BDPIN_GPIO_13
  {GPIOF, GPIO_PIN_1,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 63 BDPIN_GPIO_14
  {GPIOF, GPIO_PIN_2,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 64 BDPIN_GPIO_15
  {GPIOD, GPIO_PIN_8,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 65 BDPIN_GPIO_16
  {GPIOF, GPIO_PIN_4,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 66 BDPIN_GPIO_17
  {GPIOD, GPIO_PIN_9,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 67 BDPIN_GPIO_18
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 68
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 69

  {GPIOF, GPIO_PIN_12,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 70 OLLO_P3_SIG1
  {GPIOF, GPIO_PIN_11,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 71 OLLO_P3_SIG2
  {GPIOF, GPIO_PIN_5,   &hADC3,   ADC_CHANNEL_15, NULL   ,   NO_PWM       , 7       },  // 72 OLLO_P3_ADC    EXTI_7
  {GPIOE, GPIO_PIN_9,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 73 OLLO_P4_SIG1
  {GPIOE, GPIO_PIN_8,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 74 OLLO_P4_SIG2
  {GPIOF, GPIO_PIN_3,   &hADC3,   ADC_CHANNEL_9 , NULL   ,   NO_PWM       , 8       },  // 75 OLLO_P4_ADC    EXTI_8
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 76
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 77
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 78
  {GPIOF, GPIO_PIN_7,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 79

  {GPIOD, GPIO_PIN_6,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 80 BDPIN_UART1_RX
  {GPIOD, GPIO_PIN_5,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 81 BDPIN_UART1_TX
  {GPIOE, GPIO_PIN_0,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 82 BDPIN_UART2_RX
  {GPIOE, GPIO_PIN_1,   NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI },  // 83 BDPIN_UART2_TX

  {NULL , 0          ,  NULL,     NO_ADC        , NULL   ,   NO_PWM       , NO_EXTI }
};
```


# [Arduino IDE](#arduino-ide)

## [Linuxにインストール](#install-on-linux)

### [USBポート](#usb-port-settings)
OpenCRのUSBポートにArduino IDEプログラムをroot権限無しでアップロードできるようにしました。  

```bash
$ wget https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/99-opencr-cdc.rules
$ sudo cp ./99-opencr-cdc.rules /etc/udev/rules.d/
$ sudo udevadm control --reload-rules
$ sudo udevadm trigger
```

![](/assets/images/platform/turtlebot3/preparation/7_1_1_usb_port_setting.png)

### [コンパイラの設定](#compiler-settings)
OpenCRライブラリは、32ビットプラットフォーム用に構築されているため、64ビットPCではArduinoIDE用の32ビットコンパイラが必要になります。  

```bash
$ sudo apt-get install libncurses5-dev:i386
```

![](/assets/images/platform/turtlebot3/preparation/7_1_2_compiler_settings.png)

### [Arduino IDE(Linux)のインストール](#install-arduino-idelinux)
Arduino公式ホームページから最新版のArduino IDEをダウンロードして、インストールします。現在、OpenCRはバージョン1.6.4.以降でサービス開始予定です。  

[https://www.arduino.cc/en/Main/Software](https://www.arduino.cc/en/Main/Software)

そして、ダウンロードしたファイルを任意のフォルダに展開し、ターミナルからインストールファイルを実行します。この場合、以下の例ではユーザーの一番上のフォルダ(~/)にtoolsというフォルダを作ります。このフォルダがArduino IDEフォルダとして機能します。  

```bash
$ cd ~/tools/arduino-1.6.4
$ ./install.sh
```

インストールしたArduino IDEのファイルパスを、bashrcファイルのPATHという絶対パスに設定します。ここでは、geditエディタの使用を推奨します。(必要に応じて他のエディタを使用してください。) 最後にソースを作成して変更を適用します。  

```bash
$ gedit ~/.bashrc
$ export PATH=$PATH:$HOME/tools/arduino-1.6.4
$ source ~/.bashrc
```

### [Arduino IDE(Linux)の実行](#run-arduino-idelinux)
Linuxプラットフォーム上でArduino IDEを実行するには、ターミナルに以下のように入力します。  

```bash
$ arduino
```

![](/assets/images/platform/turtlebot3/preparation/ide0.png)

### [Arduino IDE(Linux)へのポーティング](#porting-to-arduino-idelinux)

#### 環境設定
Arduino IDEを起動した後に、IDEのトップメニューにある「ファイル」→「環境設定」をクリックします。環境設定ウィンドウが表示されたら、以下のリンクをコピーして、Additional Boards Manager URLsのテキストボックスに貼り付けます。(この作業には約20分かかります)  

```
https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/arduino/opencr_release/package_opencr_index.json
```

![](/assets/images/platform/turtlebot3/preparation/ide1.png)

#### ボードマネージャーを使ってOpenCRパッケージをインストール
ツール → ボード → ボードマネージャーをクリックします。  

![](/assets/images/platform/turtlebot3/preparation/ide2.png)

テキストボックスにOpenCRと入力して、OpenCR by ROBOTISパッケージを検索します。見つかったら、インストールをクリックします。  

![](/assets/images/platform/turtlebot3/preparation/ide3.png)

インストールが終わると「INSTALLED」と表示されます。  

![](/assets/images/platform/turtlebot3/preparation/ide4.png)

ツール → ボードのリストにOpenCRボードが表示されるようになったかどうかを確認します。これをクリックして、OpenCRボードのソースをインポートします。  

![](/assets/images/platform/turtlebot3/preparation/ide5.png)

#### ポート設定
このステップでは、プログラムをアップロードするためのポート設定を行います。パソコンとOpenCRをUSBポートで接続してください。  

ツール → ポート → /dev/ttyACM0を選択してください。  

**警告** : 文字列 `/dev/ttyACM0` の下一桁の値 `0` は USB 接続環境によって異なる場合があります。  
{: .notice--warning}

![](/assets/images/platform/turtlebot3/preparation/ide6.png)

### [モデムマネージャの削除](#remove-modemmanager)

Arduino IDEでプログラミングを行い、OpenCRにプログラムをアップロードすると、OpenCRが再起動して再接続されます。同時に、Linuxのモデム関連のパッケージから、デバイスを管理するためのATコマンドが送信されます。このように、OpenCR上で接続エラーが発生していることを示していますので、このステップは事前に行っておく必要があります。　　

```bash
$ sudo apt-get purge modemmanager
```

## [Macにインストール](#install-on-mac)

### [Arduino IDE(Mac)のインストール](#install-arduino-idemac)

Arduinoの公式ホームページから最新版のArduino IDEをダウンロードしてインストールします。現在、OpenCRはバージョン1.6.4以降でサービスを開始する予定です。

[https://www.arduino.cc/en/Main/Software](https://www.arduino.cc/en/Main/Software)

### [Arduino IDE(Mac)の実行](#run-arduino-idemac)

MacプラットフォームでArduino IDEを実行するには、以下のようにArduino IDEのアイコンをクリックします。  

![](/assets/images/parts/controller/opencr10/arduino_mac_01.png)

![](/assets/images/parts/controller/opencr10/arduino_mac_02.png)

### [Arduino IDE(Mac)へのポーティング](#porting-to-arduino-idemac)

#### 環境設定
Arduino IDEを起動したら、IDEのトップメニューにある「ファイル」→「環境設定」をクリックします。環境設定ウィンドウが表示されたら、以下のリンクをコピーして、Additional Boards Manager URLsのテキストボックスに貼り付けます。(この作業には約20分かかります)  

```
https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/arduino/opencr_release/package_opencr_index.json
```

![](/assets/images/parts/controller/opencr10/arduino_mac_03.png)

#### ボードマネージャーを使ってOpenCRパッケージをインストール
ツール → ボード→ ボードマネージャーをクリックします。  
実行 
![](/assets/images/parts/controller/opencr10/arduino_mac_04.png)

テキストボックスにOpenCRと入力して、OpenCR by ROBOTISパッケージを探します。OpenCRパッケージをインストールします。  
インストールが終わると「INSTALLED」と表示されます。  

ツール → ボードの一覧にOpenCR Boardが表示されるようになったかどうかを確認してください。これをクリックして、OpenCR Boardのソースをインポートします。  

![](/assets/images/parts/controller/opencr10/arduino_mac_05.png)

#### ポート設定
このステップでは、プログラムアップロードをするためのポート設定を行います。パソコンとOpenCRをUSBポートで接続します。  
 ツール → ポート → /dev/cu.usbmodem1411を選択します。

**注意** : PSの接続されている環境によっては、 `/dev/cu.usbmodem1411` 値が異なる場合があります。  
{: .notice--warning}

![](/assets/images/parts/controller/opencr10/arduino_mac_06.png)

## [Install on Windows](#install-on-windows)

### [Install Driver](#install-driver)

**WARNING** : For Windows 10 PCs, please skip this driver installation.  
Proper driver will be automatically installed.
{: .notice--warning}

To use OpenCR's USB port as a serial port in Windows below 8.x, you need a USB CDC driver. You can install the USB driver provided by ST.

[http://www.st.com/en/development-tools/stsw-stm32102.html](http://www.st.com/en/development-tools/stsw-stm32102.html)

### [Install Arduino IDE(Windows)](#install-arduino-idewindows)

Download the latest version of Arduino IDE from the official arduino homepage, and install it. Currently, the OpenCR will be on service in the version 1.6.4 or later.

[https://www.arduino.cc/en/Main/Software](https://www.arduino.cc/en/Main/Software)

The Arduino IDE for Windows is available as an installation version and a compressed version, so you can install it using your preferred method.

### [Porting to Arduino IDE(Windows)](#porting-to-arduino-idewindows)

#### Preferences
After Arduino IDE is run, click File → Preferences in the top menu of the IDE. When the Preferences window appears, copy and paste following link to the Additional Boards Manager URLs textbox. (This step may take about 20 min.)

```
https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/arduino/opencr_release/package_opencr_index.json
```

#### Install the OpenCR package via Boards Manager
1. Click Tools → Board → Boards Manager.
2. Type OpenCR into the textbox to find the OpenCR by ROBOTIS package. After it finds out, click Install.
3. After the installation, “INSTALLED” will be appeared.
4. See if OpenCR Board is now on the list of Tools → Board. Click this to import the OpenCR Board source.

#### Port setting
This step shows the port setting for the program uploads. The OpenCR should be connected to the PC and the OpenCR via the USB ports.  
Select Tools → Port → COM1.

**CAUTION** : The value of `COM1` may be different depending on the environment connected to the PC.
{: .notice--warning}


# [Examples](#examples)

## [LED](#led)
It is a built-in LED test on the OpenCR board.  

### Code
There are 5 LEDs available in OpenCR, USER1 ~ 4, and the LED connected to base 13 of Arduino.  
USER1 ~ 4 arduino pin numbers are defined as follows. When the corresponding pin is output as High / Low, the LED turns on / off.

```c++
#define BDPIN_LED_USER_1        22
#define BDPIN_LED_USER_2        23
#define BDPIN_LED_USER_3        24
#define BDPIN_LED_USER_4        25
```

It is a code that sequentially turns on and off all the LEDs.

```c++
int led_pin = 13;
int led_pin_user[4] = { BDPIN_LED_USER_1, BDPIN_LED_USER_2, BDPIN_LED_USER_3, BDPIN_LED_USER_4 };

void setup() {
  // Set up the built-in LED pin as an output:
  pinMode(led_pin, OUTPUT);
  pinMode(led_pin_user[0], OUTPUT);
  pinMode(led_pin_user[1], OUTPUT);
  pinMode(led_pin_user[2], OUTPUT);
  pinMode(led_pin_user[3], OUTPUT);
}

void loop() {
  int i;

  digitalWrite(led_pin, HIGH);  // set to as HIGH LED is turn-off
  delay(100);                   // Wait for 0.1 second
  digitalWrite(led_pin, LOW);   // set to as LOW LED is turn-on
  delay(100);                   // Wait for 0.1 second

  for( i=0; i<4; i++ )
  {
    digitalWrite(led_pin_user[i], HIGH);
    delay(100);
  }
  for( i=0; i<4; i++ )
  {
    digitalWrite(led_pin_user[i], LOW);
    delay(100);
  }
}
```

### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/VTz_iBqisFk" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

## [Button](#button)
It is a built-in BUTTON test on the OpenCR board.  

### Code
There are Push switches SW1 ~ 2 and Dip switches 1 ~ 2 in OpenCR. The pin number is defined as below, so you can see the status of the current button when you input the data of that pin.

```c++
#define BDPIN_DIP_SW_1          26
#define BDPIN_DIP_SW_2          27
#define BDPIN_PUSH_SW_1         34
#define BDPIN_PUSH_SW_2         35
```

It is a code that outputs the button input status in serial.

```c++
void setup(){
  Serial.begin(115200);

  pinMode(BDPIN_DIP_SW_1, INPUT);
  pinMode(BDPIN_DIP_SW_2, INPUT);
  pinMode(BDPIN_PUSH_SW_1, INPUT);
  pinMode(BDPIN_PUSH_SW_2, INPUT);

}
void loop(){
  int dip_state;
  int push_state;

  dip_state  = digitalRead(BDPIN_DIP_SW_1)<<0;
  dip_state |= digitalRead(BDPIN_DIP_SW_2)<<1;

  push_state  = digitalRead(BDPIN_PUSH_SW_1)<<0;
  push_state |= digitalRead(BDPIN_PUSH_SW_2)<<1;

  Serial.print("dip_state = ");
  Serial.print(dip_state, BIN);

  Serial.print("\tpush_state = ");
  Serial.println(push_state, BIN);


  delay(100);
}
```

### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/8RfEmWHOjlQ" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

## [Buzzer](#buzzer)
It is a BUZZER related test built in the OpenCR board and uses the Tone function of Arduino.

### Code
OpenCR has a built-in BUZZER, and the sound varies depending on the frequency.
The built-in BUZZER is also mapped to the arduino pin number, and the arduino pin number is as follows. Arduino's Tone function is ported, so you can use BUZZER by using this function.

```c++
#define BDPIN_BUZZER            31
```

It outputs the melody according to the scale defined in the pitches.h header. The following code is a change from OpenCR's BUZZER to only the PIN number in the example provided in the Arduino IDE.

```c++
#include "pitches.h"

// notes in the melody:
int melody[] = {
  NOTE_C4, NOTE_G3, NOTE_G3, NOTE_A3, NOTE_G3, 0, NOTE_B3, NOTE_C4
};

// note durations: 4 = quarter note, 8 = eighth note, etc.:
int noteDurations[] = {
  4, 8, 8, 4, 4, 4, 4, 4
};

void setup() {
  // iterate over the notes of the melody:
  for (int thisNote = 0; thisNote < 8; thisNote++) {

    // to calculate the note duration, take one second
    // divided by the note type.
    //e.g. quarter note = 1000 / 4, eighth note = 1000/8, etc.
    int noteDuration = 1000 / noteDurations[thisNote];
    tone(BDPIN_BUZZER, melody[thisNote], noteDuration);

    // to distinguish the notes, set a minimum time between them.
    // the note's duration + 30% seems to work well:
    int pauseBetweenNotes = noteDuration * 1.30;
    delay(pauseBetweenNotes);
    // stop the tone playing:
    noTone(BDPIN_BUZZER);
  }
}
```

### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/gvICseDo0SQ" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

## [PWM](#pwm)
This is the PWM output test from the Arundin pin of the OpenCR board.

### Code

OpenCR has the same pin configuration as Arduino Uno. The PWM output is also mapped to the same port. Therefore, analogueWrite is used to output the PWM duty ratio to the corresponding ports. The resolution is 8 bits, from 0 to 255, and the frequency is 50 KHz.

![](/assets/images/parts/controller/opencr10/exam_pwm_01.png)

This is an example of PWM output on all six pins.

```c++
int pwm_pins[6] = { 3, 5, 6, 9, 10, 11 };

void setup() {
  // put your setup code here, to run once:
}

void loop() {
  // put your main code here, to run repeatedly:
  int i;
  static uint8_t pwm_out = 0;

  for( i=0; i<6; i++ )
  {
    analogWrite(pwm_pins[i], pwm_out++);
  }
  delay(100);
}
```

### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/de_K0mpgVcE" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

## [EEPROM](#eeprom)
It is the EEPROM library test of OpenCR board.

OpenCR does not have EEPROM memory, so it emulates a part of flash memory built in STM32F746 into EEPROM. The method of emulation was provided by ST as an example.  
The area used as EEPROM is 0x08010000 ~ 0x08020000 as shown below. Two sectors are used.

![](/assets/images/parts/controller/opencr10/ex_eeprom_01.png)

32 bits are used to store one data, the lower 16 bits are the data to be stored, and the upper 16 bits indicate the address of the corresponding data. When storing data, it is always stored in the new location. When you use one page while saving the data, only the latest values ​​from the saved page are copied to the new page and the existing page is deleted.
As a result, the number of flash memory erasures is reduced, thereby increasing the write-through life.

![](/assets/images/parts/controller/opencr10/ex_eeprom_02.png)

![](/assets/images/parts/controller/opencr10/ex_eeprom_03.png)

To use the EEPROM library, a header must be added, and the maximum size of the current EEPROM is 1 KBytes. Since the EEPROM library has ported what is supported in Arduino, the basic usage method is the same as that used in other existing Arduino boards. For more information on how to use it, please refer to the Adunion site.

[https://www.arduino.cc/en/Reference/EEPROM](https://www.arduino.cc/en/Reference/EEPROM)

### Code

```c++
#include <EEPROM.h>

void setup() {
  // put your setup code here, to run once:
  Serial.begin(115200);
}

void loop() {
  // put your main code here, to run repeatedly:
  uint32_t tTime;
  static int i = 0;


  if( (millis()-tTime) > 100 )
  {
    Serial.print(EEPROM.read(0));
    Serial.print("\t");
    Serial.print(EEPROM.read(1));
    Serial.print("\t");
    Serial.print(EEPROM.read(2));
    Serial.println("\t");

    tTime = millis();
  }

  if (Serial.available())
  {
    uint8_t inByte = Serial.read();

    if( inByte == '1' )
    {
      EEPROM.write(0, i+1);
      EEPROM.write(1, i+2);
      EEPROM.write(2, i+3);
      i++;
    }
  }
}
```

### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/wTTbqdFP8uc" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

## [OP3](#op3)
OpenCR is used for power and sensor control in OP3, a humanoid robot. If the OpenCR firmware for OP3 has been changed, follow the procedure below to update it.
 
**WARRNING** : Please turn off the power switch before connecting power source(battery or SMPS) to the product.
{: .notice--warning}

### Preparations
OpenCR develops and downloads firmware through the Arduino IDE. Therefore, you must install the Arduino IDE in advance and install the OpenCR board package. Install through the following link document.

- [Install Arduino IDE and OpenCR](#arduino-ide)

### Download OP3 firmware

1. To update OpenCR's firmware, open the front cover of OP3 and connect USB to PC as shown below.

    ![](/assets/images/parts/controller/opencr10/op3_01.png)

2. After connecting USB, select Tools-> Board-> OpenCR Board in Arduino IDE.
3. Change Tools-> Port to the port to which the board is connected.  
4. In the Arduino IDE Examples, select the firmware for OP3.

    ![](/assets/images/parts/controller/opencr10/op3_02.png)

5. Click on the icon in the Arduino IDE that displays the red circle in the following figure to build and download the firmware. When the download is completed, the firmware is automatically executed.

    ![](/assets/images/parts/controller/opencr10/op3_03.png)

### Editing OP3 Firmware
The firmware that is provided as a basic example of OpenCR is read-only. If you want to edit it, you have to save it to a new folder and work on it.  

1. Open the OP3 example.

    ![](/assets/images/parts/controller/opencr10/op3_02.png)

2. Select File-> Save.

    ![](/assets/images/parts/controller/opencr10/op3_05.png)

3. Since the example provided is Read-Only, select OK to save it as a new file.

    ![](/assets/images/parts/controller/opencr10/op3_06.png)

4. Save it to a new folder and edit it. Once editing is complete, repeat the process of building and downloading the firmware.

    ![](/assets/images/parts/controller/opencr10/op3_09.png)

## [Sensors](#sensors)

### [Ambient Light Sensor](#ambient-light-sensor)
It is ambient light sensor test on the OpenCR board.

- Pinouts
  - Green : Signal
  - Red : Vcc
  - Black : Gnd

- Specification
  - [ambient light sensor specification](https://www.dfrobot.com/wiki/index.php/DFRobot_Ambient_Light_Sensor_SKU:DFR0026#Application)
  - Supply Voltage : 3.3V to 5V
  - Illumination range : 1 Lux to 6000 Lux
  - Interface : Analog

#### Code
LED turns off/on sequentially depending on the light received by the sensor.  
LED turns off in bright place. If it is dark place, the LED turns on.  
This sensor is an analog sensor, connect it to port A0.

```c++
#define BDPIN_LED_USER_1     23
#define BDPIN_LED_USER_2     24
#define BDPIN_LED_USER_3     25
```

It is a code that turns on/off the LED depending on the brightness of light changes.

```c++

void setup()
{
  Serial.begin(9600);
  pinMode(BDPIN_LED_USER_1, OUTPUT);
  pinMode(BDPIN_LED_USER_2, OUTPUT);
  pinMode(BDPIN_LED_USER_3, OUTPUT);
}

void loop()
{
  if(analogRead(0)<200)
  {
    digitalWrite(BDPIN_LED_USER_1, LOW);
    digitalWrite(BDPIN_LED_USER_2, LOW);
    digitalWrite(BDPIN_LED_USER_3, LOW);
  }
  else if(analogRead(0)>200 && analogRead(0)<300)
  {
    digitalWrite(BDPIN_LED_USER_1, HIGH);
    digitalWrite(BDPIN_LED_USER_2, LOW);
    digitalWrite(BDPIN_LED_USER_3, LOW);
  }
  else if(analogRead(0)>300 && analogRead(0)<400)
  {
    digitalWrite(BDPIN_LED_USER_1, HIGH);
    digitalWrite(BDPIN_LED_USER_2, HIGH);
    digitalWrite(BDPIN_LED_USER_3, LOW);
  }
  else if(analogRead(0)>400 && analogRead(0)<500)
  {
    digitalWrite(BDPIN_LED_USER_1, HIGH);
    digitalWrite(BDPIN_LED_USER_2, HIGH);
    digitalWrite(BDPIN_LED_USER_3, HIGH);
  }
  Serial.println(analogRead(0), DEC);
  delay(100);
}
```

#### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/eqZsd12g0VI" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

### [Tilt Sensor](#tilt-sensor)
It is tilt sensor test on the OpenCR.

![](/assets/images/parts/controller/opencr10/tilt_sensor.png)

- Pinouts
  - Green : Signal
  - Red : Vcc
  - Black : Gnd

- Specification
  - [Tilt Sensor Specification](https://www.dfrobot.com/wiki/index.php/Digital_Tilt_Sensor_SKU:DFR0028)
  - Supply Voltage : 3.3V to 5V
  - Interface : Digital

#### Code
tilt sensor and led are connected to OpenCR. so that red/blue led is on/off when tilted and red/blue led is off/on when not tilted.  
Connect the Tilt Sensor, Led_blue, and Led_red signal pins to D0, D1, and D2.

```c++
#define tilt     0
#define led_blue 1
#define led_red  2
```

```c++

void setup()
{
  pinMode(tilt, INPUT);
  pinMode(led_blue, OUTPUT);
  pinMode(led_red, OUTPUT);
}

void loop()
{
  if(digitalRead(tilt) == HIGH)
  {
    digitalWrite(led_blue, HIGH);
    digitalWrite(led_red, LOW);
  }
  else
  {
    digitalWrite(led_blue, LOW);
    digitalWrite(led_red, HIGH);
  }
}
```

#### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/VejyCWv4FLc" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

### [Rotation Sensor](#rotation-sensor)

It is rotation sensor test on the OpenCR board.

![](/assets/images/parts/controller/opencr10/rotation_sensor.png)

- Specification
  - [Rotation Sensor Specification](https://www.dfrobot.com/wiki/index.php/Digital_Tilt_Sensor_SKU:DFR0028)
  - Rotation Angle : 3600 degrees
  - Supply Voltage : 3.3V to 5V
  - Interface : Analog

#### Code
Rotation sensor is an analog sensor, the output value depending on the degree of rotation.  
The LED turned on/off depending on the degree of rotation.  
The signal pin is connected to A0 of OpenCR.

```c++
#define BDPIN_LED_USER_1        22
#define BDPIN_LED_USER_2        23
#define BDPIN_LED_USER_3        24
#define BDPIN_LED_USER_4        25
```

```c++
const int analogInPin = A0;
int sensorValue = 0;

void setup()
{
  Serial.begin(9600);
  pinMode(BDPIN_LED_USER_4, OUTPUT);
  pinMode(BDPIN_LED_USER_3, OUTPUT);
  pinMode(BDPIN_LED_USER_2, OUTPUT);
  pinMode(BDPIN_LED_USER_1, OUTPUT);
}

void loop()
{
  sensorValue = analogRead(analogInPin);
  Serial.print(" sensorValue : ");
  Serial.println(sensorValue);

  if(sensorValue>0 && sensorValue<50)
  {
    digitalWrite(BDPIN_LED_USER_4, LOW);
    digitalWrite(BDPIN_LED_USER_3, HIGH);
    digitalWrite(BDPIN_LED_USER_2, HIGH);
    digitalWrite(BDPIN_LED_USER_1, HIGH);
  }
  if(sensorValue>50 && sensorValue<100)
  {
    digitalWrite(BDPIN_LED_USER_4, LOW);
    digitalWrite(BDPIN_LED_USER_3, LOW);
    digitalWrite(BDPIN_LED_USER_2, HIGH);
    digitalWrite(BDPIN_LED_USER_1, HIGH);
  }
  if(sensorValue>100 && sensorValue<150)
  {
    digitalWrite(BDPIN_LED_USER_4, LOW);
    digitalWrite(BDPIN_LED_USER_3, LOW);
    digitalWrite(BDPIN_LED_USER_2, LOW);
    digitalWrite(BDPIN_LED_USER_1, HIGH);
  }
  if(sensorValue>200 && sensorValue<250)
  {
    digitalWrite(BDPIN_LED_USER_4, LOW);
    digitalWrite(BDPIN_LED_USER_3, LOW);
    digitalWrite(BDPIN_LED_USER_2, LOW);
    digitalWrite(BDPIN_LED_USER_1, LOW);
  }
  delay(100);
}
```

#### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/z2AbTL7R6rg" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

### [Capacitive Touch Sensor](#capacitive-touch-sensor)
It is capacitive touch sensor test on the OpenCR board.

![](/assets/images/parts/controller/opencr10/cap_sensor.jpg)

- Pinouts
  - Green : Signal
  - Red : Vcc
  - Black : Gnd

- Specification
  - [Capacitive Touch Sensor Specification](https://www.dfrobot.com/wiki/index.php/DFRobot_Capacitive_Touch_Sensor_SKU:DFR0030)
  - Supply Voltage : 3.3V to 5V
  - Interface : Digital

#### Code
When you put your hand on the sensor, the led turn on/off sequentially and then the LED turns off when you take your hand.  
Tilt sensor is a digital sensor, signal of sensor is connected to D0 of OpenCR.

```c++
#define SensorINPUT             0
#define BDPIN_LED_USER_1        22
#define BDPIN_LED_USER_2        23
#define BDPIN_LED_USER_3        24
#define BDPIN_LED_USER_4        25
```

```c++

int LED[] = {BDPIN_LED_USER_1, BDPIN_LED_USER_2, BDPIN_LED_USER_3, BDPIN_LED_USER_4};
int i = 0;

void setup()
{
  pinMode(SensorINPUT, INPUT);
  pinMode(BDPIN_LED_USER_1, OUTPUT);
  pinMode(BDPIN_LED_USER_2, OUTPUT);
  pinMode(BDPIN_LED_USER_3, OUTPUT);
  pinMode(BDPIN_LED_USER_4, OUTPUT);
}

void loop()
{
  if (digitalRead(SensorINPUT) == HIGH )
  {
    for(i=0; i<4; i++)
    {
      digitalWrite(LED[i], LOW);
      delay(100);
      digitalWrite(LED[i], HIGH);
    }
  }
  if (digitalRead(SensorINPUT) == LOW )
  {
    for(i=0; i<4; i++)
    {
      digitalWrite(LED[i], HIGH);
    }
  }
}
```

#### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/CtYwSdOD1wI" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

### [Flame Sensor](#flame-sensor)
It is flame sensor test on the OpenCR board.

![](/assets/images/parts/controller/opencr10/flame_sensor.jpg)

- Pinouts
  - Blue : Signal
  - Red : Vcc
  - Black : Gnd

- Specification
  - [Flame Sensor Specification](https://www.dfrobot.com/wiki/index.php/Flame_sensor_SKU:_DFR0076)
  - Detection range : 20cm(4.8V) ~ 100cm(1V)
  - Supply Voltage : 3.3V to 5V
  - Interface : Analog

#### Code
If the flame is detected, turns on the led.  
Fire near the sensor, it outputs a high value close to 1024.  
If the output exceeds 800, led will turn on.  
Signal is connected to A0 of Arduino.

```c++
#define BDPIN_LED_USER_1 22
#define flame            0
```

```c++
void setup()
{
  Serial.begin(9600);
  pinMode(BDPIN_LED_USER_1, OUTPUT);
}

void loop()
{
  int val;
  val=analogRead(flame);

  if(val>800)
  {
    digitalWrite(BDPIN_LED_USER_1, LOW);
  }
  else
  {
    digitalWrite(BDPIN_LED_USER_1, HIGH);
  }

  Serial.println(val,DEC);
  delay(100);
}
```

#### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/DcDFl4UjUos" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

### [Joystic Sensor](#joystick-sensor)
It is joystic test on the OpenCR board.

![](/assets/images/parts/controller/opencr10/joystick_sensor.png)

- Specification
  - [Joystic Sensor Specification](https://www.dfrobot.com/wiki/index.php/Joystick_Module_For_Arduino_(SKU:DFR0061))
  - Interface : Analog

#### Code
Joystic is to get the output value according to the input.  
We will look at the X Y Z values ​​that change depending on how we move.  
Signal of x,y and z is connected to A0, A1, A2 of Arduino.

```c++
#define X A0
#define Y A1
#define Z A2
```

```c++
void setup()
{
  Serial.begin(9600);
}
void loop()
{
  int x,y,z;
  x=analogRead(X);
  y=analogRead(Y);
  z=analogRead(Z);
.
  Serial.print(" X = ");
  Serial.print(x,DEC);

  Serial.print(" Y = ");
  Serial.print(y,DEC);

  Serial.print(" Z = ");
  Serial.println(z,DEC);
  delay(100);
}
```

#### Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/7fOIeFTg7bY" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

## [DYNMAIXEL Workbench](#dynamixel-workbench)

- [DYNAMIXEL Workbench examples](/docs/en/software/dynamixel/dynamixel_workbench/#opencr-and-opencm-tutorials)

## [OpenMANIPULATOR](#openmanipulator)

- [OpenMANIPULATOR examples](/docs/en/platform/openmanipulator/#how-to-control-on-opencr)

# [Downloads](#downloads)

- `Download` [BOM](https://github.com/ROBOTIS-GIT/OpenCR-Hardware/tree/master/BOM)
- `Download` [Schematic](https://github.com/ROBOTIS-GIT/OpenCR-Hardware/tree/master/Schematic)
- `Download` [PCB](https://github.com/ROBOTIS-GIT/OpenCR-Hardware/tree/master/CAD)

# [References](#references)

## [Recovery Mode](#recovery-mode)

If currupted or incompleted firmware is downloaded and the board freezes or does not work, you must enter the boot loader to be able to download the normal firmware.  
To execute the boot loader, please follow the instruction below.

1. Hold down the `PUSH SW2` button.
2. Press the `Reset` button.
3. Release the `Reset` button.
4. Release the `PUSH SW2` button.

OpenCR will enter the boot loader after reset. When the boot loader is running, the STATUS LED blinks every 100ms.

![](/assets/images/parts/controller/opencr10/bootloader_19.png)

You can download the normal firmware while the boot loader is running.

## [Certifications](#certifications)
Please inquire us for information regarding unlisted certifications.

### [FCC](#fcc)
{% include en/dxl/fcc_class_a.md %}


# [Bootloader](#bootloader)
The bootloader is responsible for initializing the board and downloading and executing the firmware into flash memory.  

The STM32F7xx, which is used for the main MCU on the OpenCR board, supports DFU(Device Firmware Upgrade).  
This enables the built-in bootloader of the MCU by itself to boot the DFU protocol by using USB, primarily for the bootloader initialization, the recovery mode, and the bootloader update.  
The biggest advantage to let the users be able to use bootloader with USB but no other JTAG equipment.  
Write the firmware by using the DFU mode which is embedded in MCU without writing / debugging equipment, such as STLink.

|     Item     |     Description     |
|:------------:|:-------------------:|
| Supported OS | Windows, Linux, Mac |
|   Compiler   | gcc arm 5.4 2016q2  |

![](/assets/images/parts/controller/opencr10/bootloader_19.png)

- USB Port
  - Connected to PC and recognized as serial port
  - A communication cable for downloading firmware through the bootloader.

- PUSH SW2
  - Press and hold the button when the power is on or reset to execute the bootloader
  - If the button is not pressed when the power is turned on, the bootloader is executed. If the firmware is in the flash memory, the bootloader executes the firmware.

## [Memory Map](#memory-map)

The STM32F746 used in OpenCR has an internal flash memory of 1024KB, and each area is defined as follows. The bootloader is located at the lowest address in the flash memory and the bootloader is first executed when the power is turned on and reset.

![](/assets/images/parts/controller/opencr10/bootloader_01.png)

## [Boot Sequence](#boot-sequence)

![](/assets/images/parts/controller/opencr10/bootloader_03.png)

If the board is powered on or reset, if the SW2 button is pressed, it waits for commands from the PC in the boot loader state. If the SW2 button is not pressed, jump to the firmware if the firmware exists in the firmware area of ​​the flash memory and execute it.

## [Communication Protocol](#communication-protocol)

### [MavLink](#mavlink)

The communication protocol for downloading firmware from the boot loader uses MavLink. MavLink was created for communication in UAV, see the link below for details.

As a feature, when the command name and parameters are generated as an xml file, the communication code for each command is automatically generated by the code such as java / c / python. It creates a function that performs CRC checking or data parsing for communication automatically, so if you define only necessary commands as xml file, you can generate the source automatically.

It is also easy to port because it generates in various languages.

[http://qgroundcontrol.org/mavlink/start](http://qgroundcontrol.org/mavlink/start)

### [Composition of Commands](#composition-of-commands)

```xml
<?xml version="1.0"?>
<mavlink>
        <!--<include>common.xml</include>-->
        <!-- NOTE: If the included file already contains a version tag, remove the version tag here, else uncomment to enable. -->
<!--<version>3</version>-->
<enums>
</enums>
<messages>
 <message id="150" name="ACK">
  <description></description>
  <field type="uint8_t"    name="msg_id"></field>
  <field type="uint16_t"   name="err_code"></field>
  <field type="uint8_t"    name="length"></field>
  <field type="uint8_t[16]" name="data"></field>
 </message>
</messages>
      ... omit ...
</mavlink>
```

The final defined xml file is added to github.

```
https://github.com/ROBOTIS-GIT/OpenCR/blob/master/arduino/opencr_bootloader/common/msg/xml/opencr_msg
```

The defined xml file should be converted to the command code of the language you want to use through the MavLink utility. Download the MavLink utility source code from github below.

[https://github.com/mavlink/mavlink/](https://github.com/mavlink/mavlink/)

It is written in Python and requires Python 2.7 or later. If it is not installed, add it.

```bash
$ sudo apt-get install python-tk
$ sudo apt-get install python-future
$ python mavgenerate.py
```

When you run MAVLink Generator, a GUI screen will appear, select the XML file already created in XML, set the language to C, and set the protocol version to 1.0. If you select the folder name for output in Out, and select Generate, a function header file that can use the command added based on the xml file is created and can be used in the firmware

![](/assets/images/parts/controller/opencr10/bootloader_04.png)

The final generated communication code can be seen in the link below.

[https://github.com/ROBOTIS-GIT/OpenCR/tree/master/arduino/opencr_develop/opencr_bootloader/common/msg/mavlink](https://github.com/ROBOTIS-GIT/OpenCR/tree/master/arduino/opencr_develop/opencr_bootloader/common/msg/mavlink)

The commands for the boot loader to download and execute the firmware through the MavLink protocol are as follows.

```c++
void cmd_read_version( msg_t *p_msg );
void cmd_read_board_name( msg_t *p_msg );
void cmd_jump_to_fw( msg_t *p_msg );
void cmd_flash_fw_write_packet( msg_t *p_msg );
void cmd_flash_fw_write_begin( msg_t *p_msg );
void cmd_flash_fw_write_end( msg_t *p_msg );
void cmd_flash_fw_write_block( msg_t *p_msg );
void cmd_flash_fw_erase( msg_t *p_msg );
void cmd_flash_fw_verify( msg_t *p_msg );
void cmd_flash_fw_read_block( msg_t *p_msg );
```

### [Download Sequence](#download-sequence)

The main function of the boot loader is to receive the firmware file from the PC, store it in flash, and execute the stored firmware. The order of downloading is as follows, and you can check how to proceed and how to proceed by looking at it. You can do the actual implementation based on this.

![](/assets/images/parts/controller/opencr10/bootloader_05.png)

![](/assets/images/parts/controller/opencr10/bootloader_06.png)


### [Message Processing](#message-processing)

In the code actually implemented, the main function calls the msg_process_vcp() function for message processing. In this case, if there is data coming from USB, msg_recv function is called to parse the message, and if any command is received, it returns TRUE to call the corresponding command function.

```c++
int main(void)
{
  tTime = millis();
  while(1)
  {
    msg_process_vcp();
  }
}

void msg_process_vcp(void)
{
  BOOL ret;
  uint8_t ch;
  msg_t msg;


  while(vcp_is_available())
  {
    ch = vcp_getch();
    ret = msg_recv( 0, ch, &msg );

    if( ret == TRUE )
    {
      switch( msg.p_msg->msgid )
      {
        case MAVLINK_MSG_ID_READ_VERSION:
          cmd_read_version(&msg);
          break;

        case MAVLINK_MSG_ID_READ_BOARD_NAME:
          cmd_read_board_name(&msg);
          break;

        ... omit ...

       default:
         cmd_send_error(&msg, ERR_INVALID_CMD);
         break;
      }
    }
  }
}
```

For example, the Mavlink_msg_read_version_decode() function parses the message data into the data structure of the command. If the field Responsive to parsed mav_data is active, it sends the boot loader version and revision via the ACK command.

![](/assets/images/parts/controller/opencr10/bootloader_07.png)

```c++
void cmd_read_version( msg_t *p_msg )
{
  err_code_t err_code = OK;
  mavlink_ack_t     mav_ack;
  mavlink_read_version_t mav_data;


  mavlink_msg_read_version_decode(p_msg->p_msg, &mav_data);


  if( mav_data.resp == 1 )
  {
    mav_ack.msg_id   = p_msg->p_msg->msgid;
    mav_ack.err_code = err_code;
    mav_ack.data[0] = boot_version;
    mav_ack.data[1] = boot_version>>8;
    mav_ack.data[2] = boot_version>>16;
    mav_ack.data[3] = boot_version>>24;
    mav_ack.data[4] = boot_revision;
    mav_ack.data[5] = boot_revision>>8;
    mav_ack.data[6] = boot_revision>>16;
    mav_ack.data[7] = boot_revision>>24;
    mav_ack.length  = 8;
    resp_ack(p_msg->ch, &mav_ack);
  }
}
```

### [Folder Structure](#folder-structure)

| Item         | Contents                                                     |
|:-------------|:-------------------------------------------------------------|
| bin          | Save obj and bin files generated after build                 |
| common->bsp  | Includes board-specific functions (LED / BUTTON / USB, etc.) |
| common->hal  | Hardware independent function folders on the board           |
| common->lib  | Libraries used externally or publicly                        |
| msg->mavlink | Communication protocol source generated by Xml               |
| msg->xml     | The xml file folder where you defined the command            |
| src          | Function folder required for boot loader function            |

![](/assets/images/parts/controller/opencr10/bootloader_08.png)

## [Update Bootloader](#update-bootloader)

You can update the bootloader using the MCU's DFU mode on the OpenCR board.  
To update using DFU mode, you need to install dfu-util.

```bash
$ sudo apt-get install dfu-util
```

### [Enter DFU Mode](#enter-dfu-mode)
To run OpenCR in DFU mode, please follow the instruction below.

1. Hold down the `Boot` button.
2. Press the `Reset` button.
3. Release the `Reset` button.
4. Release the `Boot` button.

OpenCR will enter the DFU mode after reset by the built-in boot loader.

![](/assets/images/parts/controller/opencr10/bootloader_19.png)

### [Check Boot Mode](#check-boot-mode)

#### For Linux
If you run lsusb, you can check if it is in DFU mode. If the MCU is in DFU mode, the DFU device will be displayed after running lsusb.

```bash
$ lsusb
```

![](/assets/images/parts/controller/opencr10/bootloader_10.png)

#### For Windows
Open the `Device Manager` > `Universal Serial Bus Devices` and see if **STM32 BOOTLOADER** is detected.  
If not, please refer to [Driver Install(Optional)](#driver-installoptional) section.

![](/assets/images/parts/controller/opencr10/dfu_device_manager.png)

### [Burn Bootloader](#burn-bootloader)

#### [Burn Bootloader(Linux)](#burn-bootloaderlinux)

**CAUTION** : Update the bootloader if version has been updated.
{: .notice--warning}

##### Programmer Setting
Select Tools → DFU-UTIL

![](/assets/images/parts/controller/opencr10/bootloader_19.png)

##### Run DFU mode.
Press the `Reset Button` while the `Boot Button` is being pushed. This activates the DFU mode.  
If you successfully entered to DFU mode, you will be able to find `STMicroelectronics STM Device in DFU Mode` text string when *lsusb* is entered in the terminal.

![](/assets/images/platform/turtlebot3/preparation/ide10.png)

##### Download Bootloader.

Click Tools → Burn Bootloader to download the bootloader.

![](/assets/images/platform/turtlebot3/preparation/ide9.png)

#### [Burn Bootloader(Mac)](#burn-bootloadermac)

**CAUTION** : Update the bootloader if version has been updated.
{: .notice--warning}

##### Programmer Setting

Select Tools → DFU-UTIL

![](/assets/images/parts/controller/opencr10/arduino_mac_07.png)

##### Run DFU Mode
Press the `Reset Button` while the `Boot Button` is being pushed. This activates the DFU mode.

![](/assets/images/parts/controller/opencr10/bootloader_19.png)

##### Download Bootloader

Click Tools → Burn Bootloader to download the bootloader.

![](/assets/images/parts/controller/opencr10/arduino_mac_08.png)

#### [Burn Bootloader(Windows)](#burn-bootloaderwindows)

**CAUTION** : Update the bootloader if version has been updated.
{: .notice--warning}

##### Run DFU Mode
Press the `Reset Button` while the `Boot Button` is being pushed. This activates the DFU mode.

![](/assets/images/parts/controller/opencr10/bootloader_19.png)

##### Download Bootloader
After setting up the Arduino IDE, run Arduino IDE and go to `Tools` > `Burn Bootloader` to download the bootloader.

![](/assets/images/parts/controller/opencr10/bootloader_12.png)

#### Custom Bootloader
If you have built a custom bootloader, move to the folder where the bin file is located and update it with dfu-util.  
Below command is an example where *opencr_boot.bin* is placed at the root directory.

```bash
$ sudo dfu-util -d 0483:df11 -a 0 -s 0x08000000 -D ./opencr_boot.bin
```

![](/assets/images/parts/controller/opencr10/bootloader_11.png)

### [Driver Install(Optional)](driver-installoptional)
In order to perform Bootloader update, ST DFU driver has to be installed on your PC.
In Windows 10, ST DFU driver is usually **installed automatically**.  

However, if ST DFU driver is not properly installed, OpenCR will not be able to burn new bootloader in Arduino IDE.  
When failing to burn the bootloader in **Arduino IDE** with below error message, please reinstall the DFU driver as described below.

```
Cannot open DFU device 0483:df11
No DFU capable USB device available
Error while burning bootloader.
```

> Error Message from Arduino IDE while burning Bootloader.

1. Download Zadig from [http://zadig.akeo.ie/](http://zadig.akeo.ie/)

2. Install and run Zadig.

3. Go to `Options` > `List All Devices`.

    ![](/assets/images/parts/controller/opencr10/zadig_01.png)

4. Select **STM32 BOOTLOADER** and install **WinUSB** driver.

    ![](/assets/images/parts/controller/opencr10/zadig_02.png)

# [Downloader](#downloader)

The PC Downloader application communicates with the boot loader and downloads the firmware from the PC to the firmware area of ​​the OpenCR board flash.  
The Downloader appends necessary information to the provided binary file.

| Item         | Description                       |
|:-------------|:----------------------------------|
| Supported OS | Windows, Linux, Mac               |
| Compiler     | Linux : gcc<br />Windows : Qt 5.7 |

## [Usage](#usage)

```bash
$ opencr_ld <Communication port> <Baudrate> <Firmware binary> <Firmware execution status [0|1]>
```

- Communication port: The serial port name is usually `/dev/ttyACM0` for Linux, and it should be the same as the serial port connected to OpenCR.
- Baudrate : The speed to communicate and input at 115,200bps.
- Firmware binary : The firmware binary image has an extension of bin.
- Firmware execution status : In case of 1, the firmware will be executed after downloading the firmware. If it is not input or if it is 0, only downloading the firmware is performed.

  ![](/assets/images/parts/controller/opencr10/arduino_bin_export.png)

  > Exporting compiled binary file fron Arduino IDE

### [Linux/Mac Example](#linuxmac-example)

If OpenCR is connected to ttyACM0 port and the binary file *opencrfw.bin* is copied into the opencr_ld directory.

```bash
$ sudo opencr_ld /dev/ttyACM0 115200 ./opencrfw.bin 1
```

### [Windows Example](#windows-example)

If OpenCR is connected to COM1 port and the binary file *opencrfw.bin* is copied into the opencr_ld directory.

```
opencr_ld.exe COM1 115200 ./opencrfw.bin 1
```

## [Execution Result](#execution-result)

![](/assets/images/parts/controller/opencr10/downloader_01.png)


## [Download Executable File](#download-executable-file)

- [https://github.com/ROBOTIS-GIT/OpenCR/tree/master/arduino/opencr_arduino/tools/opencr_tools_1.0.0](https://github.com/ROBOTIS-GIT/OpenCR/tree/master/arduino/opencr_arduino/tools/opencr_tools_1.0.0)

[Reference Manual]: http://www.st.com/resource/en/reference_manual/dm00124865.pdf
[Datasheet]: http://www.st.com/resource/en/datasheet/stm32f745ie.pdf
[B3B-EH-A]: http://www.jst-mfg.com/product/pdf/eng/eEH.pdf
[B4B-EH-A]: http://www.jst-mfg.com/product/pdf/eng/eEH.pdf
[20010WS-04]: http://www.alldatasheet.com/datasheet-pdf/pdf/147797/YEONHO/20010WS-04000.html
[20010WS-04]: http://www.alldatasheet.com/datasheet-pdf/pdf/147797/YEONHO/20010WS-04000.html
[SMW250-02]: http://www.alldatasheet.com/datasheet-pdf/pdf/148144/YEONHO/SMW250-02P.html
[5267-02A]: http://www.molex.com/molex/products/datasheet.jsp?part=active/0022035025_PCB_HEADERS.xml&channel=Products&Lang=en-US
[20010WS-02]: http://www.alldatasheet.com/datasheet-pdf/pdf/147795/YEONHO/20010WS-02000.html
[Molex 53047-0210]: http://www.molex.com/molex/products/datasheet.jsp?part=active/0530470210_PCB_HEADERS.xml

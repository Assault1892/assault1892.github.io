---
title: "CPUとGPU(NVEnc)でどれほどの差が出るのか？"
date: 2022-03-18
categories: ["雑多", "Aviutl"]
tag: ["雑多"]
lead: "「GPUエンコの方が早いけど画質低い」とかよく言われてるけど結局どれほど変わるのかわからないから検証した"
description: "Aviutlでエンコードする際、CPUとGPUどっちの方が早く、画質が良いのか気になったので検証してみました。参考になるかどうかは知りません"
---

Lead の通りです。  
私実は海外 meme が好きで、それを使って動画を作る事があるんですが、ちょっと凝った事をするだけで **(わずか 30 秒程の動画で)** エンコード時間が 10 分だの行ってちょっとこれはな～と思いながら Googling していた所、「x264guiEx のプラグインで NVEnc 使えるようになってエンコード速度上がるけどその分画質はちょっと落ちるよ」と書いてあるページを見つけました。  
ただ、そんなん言ったって実際どんぐらい早くなってどんぐらい画質落ちるのか正直予想出来ません。  
てことで検証しました。  
ただ x264 と NVEnc で設定を揃えられてなかった為、割と不正確な記録ではあります。気になる方は自分で色々やってみてください。

## 環境

CPU は「AMD Ryzen5 3500」、GPU は「NVIDIA RTX 2060 **Mobile**」です。  
CPU はともかく GPU はモバイル版 (ノート PC 向け) なのに注意です。デスクトップ版 GPU だと GTX1070 ぐらいが近いんじゃないでしょうか？

## Aviutl

Aviutl は `1.00` を使用します。`1.10` でもよかったんですが、結構バグが多く尚且ちょっと編集作業するだけでメモリ不足だの何だので落ちまくったのでやめました。  
`x264guiEx` のバージョンは記事執筆時点(2022/03/19)での最新版 `3.03` を使用します。  
`NVEnc.auo` も同様、執筆時点での最新版 `5.46` を使用します。  
プリセットは `YouTube` プリセット、 `H.264 品質指定` をそのまま使用します。

{{<figure src="./image/1.png" alt="x264guiExプリセット設定" width="100%">}}
{{<figure src="./image/2.png" alt="NVEncプリセット設定" width="100%">}}

エンコードのために使用した aup は[こちら](https://youtu.be/P8RSlWAPiFw)の動画の aup を使用します。  
やっている事的にはほぼ基礎の基礎レベルの事しかしてませんが、凝った事するよりはこういったシンプルな動画の方が検証が楽なのです...。

エンコード時間の確認はエンコード終了後のログに記述されている `総エンコード時間` を使用します。

# 比較

|                    |   CPU    |   GPU    |
| :----------------- | :------: | :------: |
| 1 回目 差: 21.9 秒 | 56.9 秒  | 35.0 秒  |
| 2 回目 差: 21.4 秒 | 56.6 秒  | 35.1 秒  |
| 3 回目 差: 21.0 秒 | 56.9 秒  | 35.0 秒  |
| 4 回目 差: 21.8 秒 | 56.9 秒  | 35.1 秒  |
| 5 回目 差: 22.1 秒 | 57.2 秒  | 35.1 秒  |
| 平均: 21.64 秒     | 56.88 秒 | 35.06 秒 |

| ビットレート   |    CPU     |    GPU     |
| :------------- | :--------: | :--------: |
| データ速度     | 25585 kbps | 16867 kbps |
| 総ビットレート | 25887 kbps | 17055 kbps |

全くブレん。CPU エンコードだけ 5 回目が 0.3 秒増えてますがまぁ誤差程度でしょう (2 回目も同様 0.3 秒減ってる)  
ビットレートは CPU と GPU で結構差が開きましたね (設定を変えて同じビットレートにすればよかったかも...)  
この辺りの設定に関しては殆ど知識が無いので気が向いた時にでもまたビットレート等揃えて再度計測し直します。  
ファイルサイズは CPU が `86.4 MB ( 90,633,770 bytes )` GPU が `56.9 MB ( 59,732,633 bytes )` です。「ディスク上のサイズ」ではなく通常の「サイズ」の値です。  
ビットレート等下がっているのでまぁこれもそうですが、ファイルサイズ削減には結構役立ちますね。  
非 Discord Nitro ユーザーなので結構お世話になる事が多そうです~~が、そもそも Aviutl を使って作った動画を Discord に上げる事はそこまでない~~

## 画質の変化

**あくまで体感上の話ですが** そこまで違いを感じないです。  
よく聞く「ブロックノイズが目立つ」とかそういう事もなかったです。  
動画はこちら: [x264](https://uploader.cc/s/xu93olb6vbfhjwnqf134njqx72rmf74qmd0jvobzw7l6as46jvs1qxfv6vi312z4.mp4) [NVEnc](https://uploader.cc/s/owsqrveshzxtb3uap7myfhlcq8jxhano0l2s7y5na5upbdior8jj9zdsip7zlpql.mp4) (どちらも 1 ヶ月で消えます 消えてたら教えてください、必要なら上げ直したりします)  
正直画質の低下を感じずそれでいてファイルサイズはおおよそ半分ぐらいまで削れてるので結構お得感はありますが、動きの激しい映像とかだったらまた変わってくるかもしれません。

---

# 雑なまとめ

画質低下は殆ど感じないが、ズームしたりするともしかしたらブロックノイズ発生してたりするのかも。とは言え普通に見てる分には全然気にならないのでほぼ変わらん。  
ビットレートは大体半分ぐらいまで落ちているが、まぁ前述した通り殆ど画質低下は感じない。  
ファイルサイズも半分か 2/3 ぐらいなので結構お得？  
現状殆どの GPU でハードウェア支援機能付いてるし、わざわざ CPU エンコードをする必要はそこまでない？

いい感じの GPU 刺さってる PC で Aviutl 使って動画編集している方にはオススメかも。  
Intel GPU でも行けると思うが、まぁ最近の CPU じゃないと厳しそう (第 7 世代ぐらいは必要感ある...？)

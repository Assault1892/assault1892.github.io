---
title: "謎の技術でフルトラ方法"
---

# 謎技術6点トラッキング

一般的なAndroid スマートフォンを腰に，Nintendo SwitchのJoy-Conを両足につける事でかなり無理矢理フルトラをするやり方です。

## はじめに

この方法でのフルボディトラッキングは以下のデメリットがあります。
1. **めちゃくちゃズレる**
  - **スマホやJoy-ConのIMUセンサーのみしか使えず，機器同士の連携も出来ないため，頻繁に・めちゃくちゃズレます**。  
    方法だけ見ればHaritoraX等と近いのですが，あちらはかなり**洗練されたソフトウェアとハードウェアだからこその技です**。寄せ集めの物じゃ無理でしょう。

2. **固定が難しい**
  - 腰のスマホはともかく，**足のJoy-Conは固定が難しいです**。  
    私はBeat Saberをプレイするために買った手首用サポーターをそのまま転用し，脛に固定しました。
    
3. 純粋に**これでもやや高い**
  - **Joy-Con2本とスマホ1台を用意する金で多分HaritoraXが買えます**。  
    これをやるためだけにわざわざ購入するのはマジで金の無駄なので**HaritoraXを買いましょう**。

以上 4つのカスポイントを見てもなおやりたいと思う変人は続きをどうぞ。

### 必要なもの

1. SteamVRやFBT対応ゲームが動くPC
2. Joy-Con 2本
3. IMUセンサーのついてるAndroidスマホ (**タブレットやiOS端末では不可**)
4. Joy-Conを足に固定するための物
5. Bluetooth ドングル
6. SlimeVR Server  
  [ここ](https://github.com/SlimeVR/SlimeVR-Installer/releases/latest/download/slimevr_web_installer.exe) からインストールしてください。
7. SlimeVR Wrangler  
  [ここ](https://github.com/carl-anders/slimevr-wrangler/releases) から最新版の実行ファイルをダウンロードしてください。
8. owoTrack  
  Google Play Storeからダウンロードしてください。

各アプリケーションのセットアップ方法は省略します。  
owoTrackに関してはググれば無限に出てくるのでググってください。

### やりかた

SlimeVR Wranglerを起動後， **Joy-Conがゲームコントローラーになってる事を確認した上で** PCに接続してください。  
`Windows + I → デバイス → Bluetoothまたはその他のデバイスを追加する → Bluetooth` で接続できます。

正しく接続されてる事を確認してから次のJoy-Conを接続してください。  
もし**「接続済み」と出ているのにSlimeVR Wranglerに出ていない場合，デバイスを削除してもう一度やり直す必要があります**。
![トラッカー](/assets/img/fbt1.png)

---

正しく接続出来たら，足に装着してください。  
装着する際，ボタンが何らかの理由で押されてしまう場合，SlimeVR Wranglerの設定にある「Send yaw reset command to SlimeVR Server after B or UP button press.」のチェックを外して再起動してください。  
その代わり，**Yaw方向のズレを修正できなくなります**。

owoTrackを接続し，腰に装着してください。

---

SlimeVR Serverを起動するとセットアップウィザードが出てきますが，「Skip Setup (設定をスキップする)」でスキップしてください。  
右下に言語選択プルダウンがあるので英語アレルギーの方はここで日本語にしておいてください。(以降日本語前提で解説します)
![トップ](/assets/img/fbt5.png)

左下「設定」をクリックし，**「SteamVR」の項にある「腰」と「足」を有効化してください**。
![腰と足を有効化](/assets/img/fbt2.png)

---

次に，その下にある**「トラッカーメカニズム」の「フィルタータイプ」を「No filtering」に設定し，更にその下にある「Drift compensation」をオフにしてください**。  
![設定変更](/assets/img/fbt3.png)
これによりSlimeVRで掛かる仮想トラッカーのフィルタリングが全て無効化されます。

---

最後に，**「FK設定」の中にある「スケーティング補正」をオフにしてください**。
![スケーティング補正](/assets/img/fbt4.png)
足の滑りを補正する機能ですが，有効だと逆にズレが悪化するように感じたため無効化しています。  
~~Strengthを下げてみてもいいと思いますが，それなら無効化する方が楽だよなぁ...。~~

---

**「トラッカー割り当て」からJoy-Conを対応する足に割り当ててください**。  
足の甲に付ける場合は「足先」を，脛に付ける場合は「足」を，太ももぐらいに付ける場合は「膝」を選択してください。

---

HMDを被ってSteamVRを起動したら**「マウントキャリブレーション」に従って仮想トラッカーをリセットしてください**。  
後は通常通りVRChatにログインして「Calibrate FBT」したら終わりです。お疲れ様でした。

### 使用感

**正直あんまり使い勝手は良くないです**。  
Lighthouseやインサイドアウト等の頭のいいトラッキング方式ではなく，**IMUセンサーのみを使用しているため必然的に3DoFとなり，当然ながら動きに制限が生まれます**。  
また，[カスポイント](#はじめに)に書いた通りものすごくズレます。性質上「少し動いたらもうズレる」ため，すぐリセット出来る体制を作る必要があります。  
めちゃくちゃ面倒。労働してHaritoraX買おうと思った。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">謎の技術による6点トラッキング！ <a href="https://t.co/S0iNAzR7Sm">pic.twitter.com/S0iNAzR7Sm</a></p>&mdash; あさると (@SzlyNe_) <a href="https://twitter.com/SzlyNe_/status/1625580959767867392?ref_src=twsrc%5Etfw">February 14, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

とはいえ，ある程度自分の思い通りに動かせる様になるため，**頻繁なズレや劣悪な精度に目を瞑れば**割と悪くないフルトラ方法ではないでしょうか。
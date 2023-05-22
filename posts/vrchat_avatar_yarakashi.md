---
title: "VRChat やらかしリスト"
---

# VRChat やらかしリスト

- [VRChat やらかしリスト](#vrchat-やらかしリスト)
  - [これはなに](#これはなに)
  - [言葉の定義](#言葉の定義)
  - [環境](#環境)
- [全般](#全般)
  - [ジャンプすると腰がぐにゃったりする](#ジャンプすると腰がぐにゃったりする)
  - [着せ替えた服が追従しない](#着せ替えた服が追従しない)
    - [おっぱいが服からこぼれる](#おっぱいが服からこぼれる)
- [アバター](#アバター)
  - [アッシュちゃん ver 1.2.1](#アッシュちゃん-ver-121)
  - [カリンちゃん ver 1.11](#カリンちゃん-ver-111)
  - [桔梗ちゃん](#桔梗ちゃん)
  - [まめひなたちゃん ver1.30](#まめひなたちゃん-ver130)
  - [舞夜ちゃん ver1.02.2](#舞夜ちゃん-ver1022)
  - [メリノちゃん ver 1.02](#メリノちゃん-ver-102)
  - [ミーシェちゃん ver 1.04](#ミーシェちゃん-ver-104)
  - [ナユちゃん ver 1.2.2](#ナユちゃん-ver-122)
  - [NORANEKOSEVEN ちゃん ver 1.11](#noranekosevenちゃん-ver-111)
  - [ラスクちゃん ver 1.2](#ラスクちゃん-ver-12)
  - [サフィーちゃん ver1.0.7](#サフィーちゃん-ver107)
  - [セフィラちゃん v6.1](#セフィラちゃん-v61)
  - [シグちゃん ver 1.21](#シグちゃん-ver-121)
- [ワールド](#ワールド)
  - [ベイクしたライトマップが汚い](#ベイクしたライトマップが汚い)
  - [MeshBaker で結合したオブジェクトのライトマップが壊れる](#meshbakerで結合したオブジェクトのライトマップが壊れる)
- [ツール](#ツール)
  - [Gesture Manager](#gesture-manager)

## これはなに

VRChat のアバターを改変する際の **「うっかりミス」** や **「初期仕様がゴミ」** などによる **「やらかし」及び「やらかし因子」** をまとめたページです。  
本ページを執筆した日 (2023/2/14) の早朝からフレンドの抱える [めちゃくちゃ謎な問題](https://twitter.com/Junnichi_sato/status/1625071604119269377) に [悩まされた](https://twitter.com/SzlyNe_/status/1625163912231657472) 挙げ句，最終的な原因は [初期仕様がゴミな事による Controller の問題](https://twitter.com/Junnichi_sato/status/1625214467079548929) で [ハゲ上がりそうになった](https://twitter.com/SzlyNe_/status/1625213516578975744) ので備忘録も兼ねて。

私が所有しているアバターやワールド，アセットのみの情報を載せています。  
「xx という yy で zz の問題が発生する」等の情報があれば [GitHub の Issue](https://github.com/Assault1892/assault1892.github.io/issues) にでも投げてください。  
テンプレートとかはありません。各々の書きやすいように書いてください。でも私が分からなかったら死です。

## 言葉の定義

アニメーション
: FX の Animator Controller 内から再生される .anim を指す  
: 例: `表情アニメーション`, `着せ替えアニメーション`

ハンドジェスチャー
: Shift + F1-8 や VR コントローラーのボタン操作等によって変化する「表情」を指す  
: 名称や出し方は [VRChat 公式ドキュメント](https://docs.vrchat.com/docs/touch) に準拠しています

ジェスチャー
: Gesture の Animator Controller 内から再生される .anim を指す
: 例: `手のジェスチャー`

モーション
: 上 2 つに当てはまらない Controller 等から再生される .anim を指す
: 例: `AFKモーション`

アセット or ツール
: Unity Editor 拡張等の便利系ツール等を指す
: 例: `Modular Avatar`, `Kisetene`, `AvatarTools`, `Mesh Baker`, `VRWorld Toolkit`

## 環境

| Name                       | Description     |
| :------------------------- | :-------------- |
| OS                         | Windows 10 22H2 |
| Unity Version              | 2019.4.31f1     |
| is VCC?                    | true            |
| is migrated?               | true            |
| VRChat SDK Base            | 3.1.10          |
| VRChat SDK Avatars, Worlds | 3.1.10          |
| ClientSim                  | 1.2.2           |
| Gesture Manager            | 3.8.2           |
| Udon Sharp                 | 1.1.6           |
| VRWorld Toolkit            | 2.1.2           |
| lilToon                    | 1.3.7           |
| UTS2                       | 2.0.9           |

# 全般

## ジャンプすると腰がぐにゃったりする

Base (Locomotion) の Animator Controller 内にある `JumpAndFall` ステートを削除する事で解決する。  
或いは Conditions の中にある `VelocityY: Greater -2.001` を `VelocityY: Greater -4` にする事である程度改善する。削除が怖い場合はこちら。

## 着せ替えた服が追従しない

多分ボーンをちゃんと動かせてない。入れ子構造のようにする必要がある。  
面倒なら Modular Avatar を使うと楽に済ませられる...はず。過信は禁物。

### おっぱいが服からこぼれる

以下のようにボーンを動かす事で対処可能。確かもっとスマートな解決方法があるはずなので思い出したら修正します。

```text
ボーン名は桔梗ちゃん準拠

- 素体 Breast_root
  - 素体 Breast_1
    - 素体 Breast_2
      - 服 Breast_2
    - 服 Breast_1
  - 服 Breast_root
```

# アバター

ファイル名順です

## [アッシュちゃん ver 1.2.1](https://mk22.booth.pm/items/3234473)

- Fist のアニメーション (huhun.anim) が Unity 上 (Gesture Manager) では正常に再生されるものの，クライアント上では再生されない。

  - まばたきシェイプキーを使用しているのが原因と思われる。  
    アニメーションを編集して「ウインク 2」系を使用する事で回避可能。

- FX の Animator Controller 内 Idle ステートに `proxy_hands_idle.anim` が指定されている。

  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Idle ステートを `None (Motion)` に変更し，Avatar Mask を全て外す事で解決する。

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [カリンちゃん ver 1.11](https://komado.booth.pm/items/3470989)

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [桔梗ちゃん](https://ponderogen.booth.pm/items/3681787)

- ver1.03 (2022/4/26 リリース) にて，FX の Animator Controller 内 Idle ステートに `proxy_hands_idle.anim` が指定されている。

  - 「ハンドジェスチャーを変更しても表情は変わるが手が動かない」という症状が発生する。  
    Idle ステートを `None (Motion)` に変更し，Avatar Mask を全て外す事で解決する。
    - これに合計で 6 時間ぐらい苦しめられた。マジでやめてほしい。

- ver1.03 (2023/2/14 リリース) にて，Gesture の Animator Controller 内 Idle ステートに何も指定されていない。

  - 「片手のみハンドジェスチャーを変更するともう片手が変な手の形になる」という症状が発生する。  
    Idle ステートを `proxy_hands_idle.anim` に変更する事で解決する。
    - 「ハンドジェスチャーが破壊される」といった事はない。あくまで Idle のジェスチャーだけが壊れる。

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [まめひなたちゃん ver1.30](https://mukumi.booth.pm/items/4340548)

見てねえ。そのうち見る。

## [舞夜ちゃん ver1.02.2](https://kyubihome.booth.pm/items/3390957)

- FX の Animator Controller 内 All Parts レイヤーに Avatar Mask がセットされている。

  - 不具合の元になりかねない。Avatar Mask を全て外す事で解決する。

- UTS2 を使用している。

  - UTS2 2.0.9 を使用している場合は問題無いと思うが，気になるなら lilToon への移行を。

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [メリノちゃん ver 1.02](https://ponderogen.booth.pm/items/2351859)

- **Dynamic Bone を使用している** 。

  - パフォーマンスに大きな影響を与えるため，Physics Bone への変換を強く推奨。

- UTS2 を使用している。

  - UTS2 2.0.9 を使用している場合は問題無いと思うが，気になるなら lilToon への移行を。

- FX の Animator Controller 内 Idle ステートに `proxy_hands_idle.anim` が指定されている上，表情アニメーション内に手のボーンを動かす Property が存在している。

  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Idle ステートを `None (Motion)` に変更し，Avatar Mask を全て外した上でそれぞれの表情アニメーションから手の Property を削除する事で解決する。
  - ちなみに Gesture 内の各ハンドサインステートにもしっかりそれぞれのジェスチャーアニメーションがセットされている。
    - つまり二重...

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [ミーシェちゃん ver 1.04](https://ponderogen.booth.pm/items/1256087)

- **Dynamic Bone を使用している** 。

  - パフォーマンスに大きな影響を与えるため，Physics Bone への変換を強く推奨。

- FX の Animator Controller 内 Idle ステートに `proxy_hands_idle.anim` が指定されている上，表情アニメーション内に手のボーンを動かす Property が存在している。

  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Idle ステートを `None (Motion)` に変更し，Avatar Mask を全て外した上でそれぞれの表情アニメーションから手の Property を削除する事で解決する。
  - メリノちゃんと同様二重になっている。なんで

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [ナユちゃん ver 1.2.2](https://mk22.booth.pm/items/4023598)

パッと見た感じ問題なし。  
強いて言えば服などの切り替えが純粋なトグルではない事か。気になるなら自分で FX レイヤーを修正。

## [NORANEKOSEVEN ちゃん ver 1.11](https://captainworkshop.booth.pm/items/2555967)

- **Dynamic Bone を使用している** 。

  - パフォーマンスに大きな影響を与えるため，Physics Bone への変換を強く推奨。

- UTS2 を使用している。
  - UTS2 2.0.9 を使用している場合は問題無いと思うが，気になるなら lilToon への移行を。

## [ラスクちゃん ver 1.2](https://komado.booth.pm/items/2559783)

- FX の Animator Controller 内表情レイヤーに Avatar Mask がセットされている。

  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Avatar Mask を全て外す事で解決する。

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [サフィーちゃん ver1.0.7](https://yueou.booth.pm/items/3939858)

- FX の Animator Controller 内表情レイヤーに Avatar Mask がセットされている。
  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Avatar Mask を全て外す事で解決する。

## [セフィラちゃん v6.1](https://mk22.booth.pm/items/2953001)

- FX の Animator Controller 内 Idle ステートに `proxy_hands_idle.anim` が指定されている。

  - 「ハンドジェスチャーを変更しても表情は変わるが手が動かない」という症状が発生する。  
    Idle ステートを `None (Motion)` に変更し，Avatar Mask を全て外す事で解決する。

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [シグちゃん ver 1.21](https://hamini.booth.pm/items/3421652)

- FX の Animator Controller 内表情レイヤーに Avatar Mask がセットされている。

  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Avatar Mask を全て外す事で解決する。

- Write Defaults を使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

# ワールド

## ベイクしたライトマップが汚い

Lightmap の設定を見直す。Denoiser を使用する事で大体解決するが，「焼けたらおかしい場所が焼けている」などは根本的な何かが間違っている。  
FBX の Import Settings 内にある「Generate Lightmap UVs」にチェックが入ってるか確認する。

## MeshBaker で結合したオブジェクトのライトマップが壊れる

テクスチャをベイクする?時，「Use Shared Settings」にアタッチされている `TextureBaker (0)...` みたいな奴を Backspace でデタッチし，「Lightmapping UVs」を `Generate_new_UV2_layout` に変更してベイクし，ライトベイクで解決する...はず。

# ツール

## [Gesture Manager](https://github.com/BlackStartx/VRC-Gesture-Manager)

- 一部アバターのシェイプキーが 3 倍 (300) になる。
  - [Write Defaults を使用しているため](https://twitter.com/Sayabeans_0011/status/1625500330665611266) 。他のアバターでも起きるはず
    - [VRChat 上では 100 にクランプされる為実質セーフだが，0→20 でアニメーションさせている場合 3 倍の 60 になる為実害が発生する。](https://twitter.com/mimyquality/status/1625501731995144193)  
      アニメーションでシェイプキーを操作する際は目的の値/3 した値をアニメーションにセットした方が良いかも？[てかそもそも Write Defaults を使わん方が良い](https://docs.vrchat.com/docs/avatars-30#write-defaults-on-states&#:~:text=We%20recommend%20keeping%20Write%20Defaults%20off%20and%20explicitly%20animating%20any%20parameter%20that%20needs%20to%20be%20set%20by%20the%20animation.) 。

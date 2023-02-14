---
title: "VRChat やらかしリスト"
---

# VRChat やらかしリスト

- TOC
{:toc}

## これはなに

VRChatのアバターを改変する際の **「うっかりミス」** や **「初期仕様がゴミ」** などによる **「やらかし」及び「やらかし因子」** をまとめたページです。  
本ページを執筆した日 (2023/2/14) の早朝からフレンドの抱える [めちゃくちゃ謎な問題](https://twitter.com/Junnichi_sato/status/1625071604119269377) に [悩まされた](https://twitter.com/SzlyNe_/status/1625163912231657472) 挙げ句，最終的な原因は [初期仕様がゴミな事によるControllerの問題](https://twitter.com/Junnichi_sato/status/1625214467079548929) で [ハゲ上がりそうになった](https://twitter.com/SzlyNe_/status/1625213516578975744) ので備忘録も兼ねて。

私が所有しているアバターやワールド，アセットのみの情報を載せています。  
「xxというyyでzzの問題が発生する」等の情報があれば [GitHubのIssue](https://github.com/Assault1892/assault1892.github.io/issues) にでも投げてください。  
テンプレートとかはありません。各々の書きやすいように書いてください。でも私が分からなかったら死です。

## 言葉の定義

アニメーション
: FXのAnimator Controller内から再生される .anim を指す  
: 例: `表情アニメーション`, `着せ替えアニメーション`

ハンドジェスチャー
: Shift + F1-8 やVRコントローラーのボタン操作等によって変化する「表情」を指す  
: 名称や出し方は [VRChat 公式ドキュメント](https://docs.vrchat.com/docs/touch) に準拠しています

ジェスチャー
: GestureのAnimator Controller内から再生される .anim を指す
: 例: `手のジェスチャー`

モーション
: 上2つに当てはまらないController等から再生される .anim を指す
: 例: `AFKモーション`

アセット or ツール
: Unity Editor拡張等の便利系ツール等を指す
: 例: `Modular Avatar`, `Kisetene`, `AvatarTools`, `Mesh Baker`, `VRWorld Toolkit`

## 環境

| Name | Description |
| :-- | :-- |
| OS | Windows 10 22H2 |
| Unity Version | 2019.4.31f1 |
| is VCC? | true |
| is All migrated? | true |
| VRChat SDK Base | 3.1.10 |
| VRChat SDK Avatars, Worlds | 3.1.10 |
| ClientSim | 1.2.2|
| Gesture Manager | 3.8.2 |
| Udon Sharp | 1.1.6 |
| VRWorld Toolkit | 2.1.2 |
| lilToon | 1.3.7 |
| UTS2 | 2.0.9 |

# 全般

## ジャンプすると腰がぐにゃったりする

Base (Locomotion) のAnimator Controller内にある `JumpAndFall` ステートを削除する事で解決する。  
或いはConditionsの中にある `VelocityY: Greater -2.001` を `VelocityY: Greater -4` にする事である程度改善する。削除が怖い場合はこちら。

## 着せ替えた服が追従しない

多分ボーンをちゃんと動かせてない。入れ子構造のようにする必要がある。  
面倒ならModular Avatarを使うと楽に済ませられる...はず。過信は禁物。

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

- Fistのアニメーション (huhun.anim) がUnity上 (Gesture Manager) では正常に再生されるものの，クライアント上では再生されない。
  - まばたきシェイプキーを使用しているのが原因と思われる。  
    アニメーションを編集して「ウインク2」系を使用する事で回避可能。

- FXのAnimator Controller内Idleステートに `proxy_hands_idle.anim` が指定されている。
    - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
      Idleステートを `None (Motion)` に変更し，Avatar Maskを全て外す事で解決する。

- Write Defaultsを使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [カリンちゃん ver 1.11](https://komado.booth.pm/items/3470989)

- Write Defaultsを使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [桔梗ちゃん](https://ponderogen.booth.pm/items/3681787)

- ver1.03 (2022/4/26リリース) にて，FXのAnimator Controller内Idleステートに `proxy_hands_idle.anim` が指定されている。
  - 「ハンドジェスチャーを変更しても表情は変わるが手が動かない」という症状が発生する。  
    Idleステートを `None (Motion)` に変更し，Avatar Maskを全て外す事で解決する。
    - これに合計で6時間ぐらい苦しめられた。マジでやめてほしい。

- ver1.03 (2023/2/14リリース) にて，GestureのAnimator Controller内Idleステートに何も指定されていない。
  - 「片手のみハンドジェスチャーを変更するともう片手が変な手の形になる」という症状が発生する。  
    Idleステートを `proxy_hands_idle.anim` に変更する事で解決する。
    - 「ハンドジェスチャーが破壊される」といった事はない。あくまでIdleのジェスチャーだけが壊れる。

- Write Defaultsを使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [メリノちゃん ver 1.02](https://ponderogen.booth.pm/items/2351859)

- **Dynamic Boneを使用している** 。
  - パフォーマンスに大きな影響を与えるため，Physics Boneへの変換を強く推奨。

- UTS2を使用している。
  - UTS2 2.0.9を使用している場合は問題無いと思うが，気になるならlilToonへの移行を。

- FXのAnimator Controller内Idleステートに `proxy_hands_idle.anim` が指定されている上，表情アニメーション内に手のボーンを動かすPropertyが存在している。
  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Idleステートを `None (Motion)` に変更し，Avatar Maskを全て外した上でそれぞれの表情アニメーションから手のPropertyを削除する事で解決する。
  - ちなみにGesture内の各ハンドサインステートにもしっかりそれぞれのジェスチャーアニメーションがセットされている。
    - つまり二重...

- Write Defaultsを使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [ミーシェちゃん ver 1.04](https://ponderogen.booth.pm/items/1256087)

- **Dynamic Boneを使用している** 。
  - パフォーマンスに大きな影響を与えるため，Physics Boneへの変換を強く推奨。

- FXのAnimator Controller内Idleステートに `proxy_hands_idle.anim` が指定されている上，表情アニメーション内に手のボーンを動かすPropertyが存在している。
  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Idleステートを `None (Motion)` に変更し，Avatar Maskを全て外した上でそれぞれの表情アニメーションから手のPropertyを削除する事で解決する。
  - メリノちゃんと同様二重になっている。なんで

- Write Defaultsを使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [ナユちゃん ver 1.2.2](https://mk22.booth.pm/items/4023598)

パッと見た感じ問題なし。  
強いて言えば服などの切り替えが純粋なトグルではない事か。気になるなら自分でFXレイヤーを修正。

## [NORANEKOSEVENちゃん ver 1.11](https://captainworkshop.booth.pm/items/2555967)

- **Dynamic Boneを使用している** 。
  - パフォーマンスに大きな影響を与えるため，Physics Boneへの変換を強く推奨。

- UTS2を使用している。
  - UTS2 2.0.9を使用している場合は問題無いと思うが，気になるならlilToonへの移行を。

## [ラスクちゃん ver 1.2](https://komado.booth.pm/items/2559783)

- FXのAnimator Controller内表情レイヤーにAvatar Maskがセットされている。
  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Avatar Maskを全て外す事で解決する。

- Write Defaultsを使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [サフィーちゃん ver1.0.7](https://yueou.booth.pm/items/3939858)

- FXのAnimator Controller内表情レイヤーにAvatar Maskがセットされている。
  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Avatar Maskを全て外す事で解決する。

## [セフィラちゃん v6.1](https://mk22.booth.pm/items/2953001)

- FXのAnimator Controller内Idleステートに `proxy_hands_idle.anim` が指定されている。
  - 「ハンドジェスチャーを変更しても表情は変わるが手が動かない」という症状が発生する。  
    Idleステートを `None (Motion)` に変更し，Avatar Maskを全て外す事で解決する。

- Write Defaultsを使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

## [シグちゃん ver 1.21](https://hamini.booth.pm/items/3421652)

- FXのAnimator Controller内表情レイヤーにAvatar Maskがセットされている。
  - 見た目上はしっかりハンドジェスチャーが変わるが，不具合の元になりかねない。  
    Avatar Maskを全て外す事で解決する。

- Write Defaultsを使用している。将来的に問題が発生する可能性があるが，急を要するわけではない。

# ワールド

## ベイクしたライトマップが汚い

Lightmapの設定を見直す。Denoiserを使用する事で大体解決するが，「焼けたらおかしい場所が焼けている」などは根本的な何かが間違っている。  
FBXのImport Settings内にある「Generate Lightmap UVs」にチェックが入ってるか確認する。

## MeshBakerで結合したオブジェクトのライトマップが壊れる

テクスチャをベイクする?時，「Use Shared Settings」にアタッチされている `TextureBaker (0)...` みたいな奴をBackspaceでデタッチし，「Lightmapping UVs」を `Generate_new_UV2_layout` に変更してベイクし，ライトベイクで解決する...はず。

# ツール

## [Gesture Manager](https://github.com/BlackStartx/VRC-Gesture-Manager)

- 一部アバターのシェイプキーが3倍 (300) になる。
  - [Write Defaultsを使用しているため](https://twitter.com/Sayabeans_0011/status/1625500330665611266) 。他のアバターでも起きるはず
    - [VRChat上では100にクランプされる為実質セーフだが，0→20でアニメーションさせている場合3倍の60になる為実害が発生する。](https://twitter.com/mimyquality/status/1625501731995144193)  
    アニメーションでシェイプキーを操作する際は目的の値/3した値をアニメーションにセットした方が良いかも？[てかそもそもWrite Defaultsを使わん方が良い](https://docs.vrchat.com/docs/avatars-30#write-defaults-on-states&#:~:text=We%20recommend%20keeping%20Write%20Defaults%20off%20and%20explicitly%20animating%20any%20parameter%20that%20needs%20to%20be%20set%20by%20the%20animation.) 。
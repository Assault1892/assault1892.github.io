---
title: 「Failed to load SELinux Policy」を直す
date: 2024-05-27 14:28:40
tags: "Linux"
---

弊校実習環境の Virtual Box 上にある CentOS 7 をミスで異常終了させてしまい、再起動したところ

```
[!!!!!!] Failed to load SELinux policy, freezing.
```

で起動できなくなってしまった。

![](VirtualBoxVM_fLmnAuS6Ik.png)

結局再インストールして無理矢理解決。真相は闇に葬られてしまった。

<!-- toc -->

# これ何？

SELinux という RHEL 系 Linux のセキュリティつよつよモジュールらしい。  
その SELinux の設定ファイルを**正しくない**状態にしてしまうと、正常に SELinux を初期化できなくなり、結果的に起動中に「Failed to load SELinux policy」で止まってしまう、らしい...。

軽く検索してみると「うっかり書き換える場所を間違えた」などで止めてしまう例が多く、異常終了によって破壊されたものを修復するケースは 1 件しか見つけられなかった。レアケース？  
見つけた記事はこれ: [OS 異常終了後、起動時に「Failed to load SELinux policy」が出てフリーズする場合の対策方法。](https://zapping.beccou.com/2021/10/12/measures-to-be-taken-when-failed-to-load-selinux-policy-appears-and-freezes-at-startup-after-an-abnormal-os-termination/)

起動時に「Failed to load SELinux policy」と出たのち、そのまま一生ぐるぐるして進まないところまで症状が一致。
![](VirtualBoxVM_ijGfQim4zu.png)
多分同じケースっぽいんだけど、発生のきっかけが違う[^1]ことから同じ対処でいいのかわからず。

## とりあえず SELinux を無効化

ブートマネージャで E キーを押して設定画面に入り、記事に従って `linux16...` の末尾に `selinux=0` を追加して `Ctrl-x` で抜けて起動。

![](VirtualBoxVM_jaJTp4FOpk.png)

SELinux を無効化すれば起動は可能。  
しかし無効化したまま使うのはなんだか気に入らないし、できれば直しておきたい。

カーネルパラメーターに `rw init=/bin/sh` を渡して起動直後にシェルに突っ込み、 `/etc/selinux/config` を確認。  
しかし中身は健康そのもの。

![](VirtualBoxVM_kAcA6l8IrG.png)

よーわからんなぁと思いつつも `enforcing=0` を渡して再起動したところ、ログインサービスやらなんやらの起動に失敗。  
これもしかして SELinux 破壊？

![](VirtualBoxVM_BcMXSxmGTr.png)

## めんどくさいので再インストール

流石にどうしようもない雰囲気を感じ取ったので SELinux を無効化し、再インストールすることに。  
`selinux=0` を渡して再起動してログインし、 `su` で root に突っ込んで以下のコマンドを実行。

```bash
setenforce 0 # SELinuxを無効化
yum remove selinux-policy\* # SELinuxのすべてを削除
rm -rf /etc/selinux/targeted /etc/selinux/config
yum install selinux-policy-targeted selinux-policy-devel policycoreutils
```

ここまで通ったら最後に `touch /.autorelabel` して再起動[^2]。

再ラベリング作業が走ったあと自動再起動され、そこには元気なログイン画面が！  
途中なんか kdump のエラーが出ててちょっともやもやしますがまぁいいでしょう。どうしても気になったら環境ぶっ飛ばして再インストールすればいいので。

![](VirtualBoxVM_wcBvIFgXRH.png)

## なんで起こったんこれ

単純な原因は「カーネルパニックさせてしまったこと」だろうなあ...。  
ただそれだけでぶっ壊れるとも思えないのでなんか別の要因もあるかもしれない。

ともかく、今後は大事に扱おうと思います...

[^1]: 私は Linux 自体をカーネルパニックさせてしまったが、当該記事は VM のメモリエラーによる異常終了。
[^2]: カーネルパラメータに `autorelabel=1` を突っ込むのと同じ

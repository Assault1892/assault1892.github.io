---
title: systemdとか消したらどうなるの
excerpt: Linuxはsystemdとか消したらどうなるのか！
date: 2025-04-14 15:03:44
category: 雑記
tags: 
    - PC
    - Linux
hide: true
---

# 結論

やめとけ

# 概要

Linuxのブートプロセス関係の調べ物をしていたらふと「systemdとかの起動時に立ち上がってくる奴らを消したらどうなるんだろう」と気になったのでやってみた！

# 環境

| なまえ | せつめい |
| :--- | :--- |
| ホストOS | Windows11 Home 24H2 26100.3775 |
| 仮想化環境 | Oracle VM VirtualBox 7.020 r163905 |
| ゲストOS | Voyager 25.04 6.14.0-15-generic |
| ブートストラップローダー | GRUB |

今回は検証する物が物なだけに頻繁に再インストールなど復旧作業を要すると考えたため、時間短縮のために**事前にインストール後アプデ直後の時点の仮想アプライアンスを作成**。  
破壊後はそちらをインポートし直すことにより即座に復旧できるという算段。天才！

# ブートストラップのしょうもな話

ディストリビューションによってブートストラップローダーが違えばその次に動くプログラムも違う。  
[Archではブートストラップローダーに `systemd-boot` を採用している](https://wiki.archlinux.jp/index.php/Systemd-boot#:~:text=systemd%20に含まれており、Arch%20システムにデフォルトでインストールされます。)が、[UbuntuではGRUBを採用している](https://gihyo.jp/admin/serial/01/ubuntu-recipe/0743#:~:text=Ubuntuは、標準のブートローダーとしてGRUBを採用しています。)など。  
また、現在ではもうほとんどみないが、LILO (**LI**nux **LO**ader)なるブートストラップローダーもあるらしい。流石にもう今は見たことがない。  


また、initを使わずsystemdに移行している環境も多くあるようで [^1] 。  
事実今回検証で使うVoyagerもsystemdを使っている。
![](VirtualBoxVM_gx6hgXrFgA.png)

initとsystemdで何が違うんだ、と思って調べてみたら思ったより結構違った。  
initと比べて早く起動し、いい感じに動いてくれて、設定しやすいらしい。実感がない (ずっとsystemdをさわってきたので)  
また、System V系とBSD系でも違いがあるらしい。ここらへんは後々しらべてみたい。

# 検証

## systemdを (実質的に) 無効化する

systemdの実体を消すには割と手間がいるため、systemdを実質的に無効化する策を取る。

systemdのやることは端的に言えばLinuxが起動したあと、ブートに必要なサービスを立ち上げてシステムを稼働させられるレベルに持っていくところにある。  
そしてsystemd自体が全てを立ち上げるわけではなく、基幹となるものが立ち上がってから、それが立ち上げていく。

これは `systemctl list-dependencies` をするとわかる。  
一番最初に `default.target` が起動してきてから、それが更に全てを立ち上げていく。

![](VirtualBoxVM_wyT9jIpU8q.png)

つまり、 `default.target` を無効化したりすれば実質的に無効化できる！

---

[^1]: というか現代でinitを使っている環境をあまり見ないように思う。直近だとCentOSとか？
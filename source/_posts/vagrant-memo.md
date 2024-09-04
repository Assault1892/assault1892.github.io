---
layout: posts
title: Vagrant おぼえがき
date: 2024-09-04 12:25:27
tags:
- PC
- 雑記
---

完全にメモ用。

<!-- toc -->

# Vagrantってなに

楽に扱えるDockerみたいな感じ？vagrant initでコンテナ引っ張ってきてvagrant upしてsshすればもう使えそうな感じがする  
VirtualBoxとかHyper-V使ってまで仮想マシンを立ち上げるのはめんどくさいがWSLぐらいのノリで雑にVMつけたりしたいときに役立ちそう。私はもっぱらその用途だが

## 導入

[ここ](https://developer.hashicorp.com/vagrant/install)から自分の環境に合わせてインストーラー落としてきたりコマンド叩くだけ 楽でいい  
複数のハイパーバイザーを使っている場合はVirtualBox等と競合してしまうケースがあるらしい [ここ](https://developer.hashicorp.com/vagrant/docs/installation)に対処法あり

インストールしたら一回PCを再起動する必要がある PATH通したり仮想化周りの設定の都合か？

## コンテナを生やす

適当なディレクトリでvagrant init (引っ張りたいコンテナの名前) してからvagrant up  

```bash
$ vagrant init hashicorp/bionic64
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'hashicorp/bionic64' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'hashicorp/bionic64'

(中略)

==> default: Machine booted and ready!
```

コンテナが起動し終わってターミナルに戻ってきたらvagrant sshでもう繋げられる

## 破壊

いらなくなったら vagrant destroy で破壊できる  

```bash
$ vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...
```

コンテナが消えるわけではなく仮想マシンのディスクイメージが消えるだけなので再度vagrant upすればクリーンな環境で起動できる
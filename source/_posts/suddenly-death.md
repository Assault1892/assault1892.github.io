---
layout: posts
title: PCは急死することもある
date: 2024-06-09 00:24:51
tags: 
- PC
- 雑記
---

読んで字の如し。**PCも人も、前触れなく突然死ぬものである。**
祝完結。

<!-- toc -->

# 何があったん

乱暴に言うと、**PCが突然死した。** 具体的にはSSD~~かOS~~が死んだかな。
停止コード `UNEXPECTED_STORE_EXCEPTION` でBSoDしたあと、OSが起動できなくなった。

PCはマウスコンピューター製「G-Tune E5-D-MTS <small>(現在販売終了)</small>」を使用。  
2021年3月に購入しているのでまだギリギリ5年制限には至っておらず、修理可能っぽい。

> この記事は主観的なことしか書いてません。

# PC構成

疑わしいところは太字。

|Name|Desc|
|:--|:--|
|**OS**|**Windows 11 Home 23H2**|
|CPU|AMD Ryzen5 3500|
|RAM|DDR4 16GB|
|**GPU**|**NVIDIA GeForce RTX2060 6GB 555.85**|
|**SSD 1**|**ADATA XPG SX6000 Pro 512GB**|
|SSD 2|Crucial MX500 CT1000MX500SSD1 1000GB|

正直前からWindowsの挙動が色々おかしかったりしていていつか壊れんだろうな～とは思っていた。
まさかこんな形で死ぬとは思っていなかったが。

CPUスパイク時全ての入力機器からの入力 (マウスとかキーボードとか) が遅れたり、カクつく現象が起きていた。[^8]
「windows11 mouse stuttering」とかで検索すると結構そういうRedditとかが出てくるあたり、普通のことなのかもしれないが...

# 症状リスト

- **BSoD吐いた時の挙動がかなり異常。** 具体的には...
  - 音が壊れてから青画面に移行するまでに数分要した。普通は一瞬で行くよね。
  - 青画面に移行してから自動再起動するまでもこれまた数分かかった。
    - また、エラー情報の収集? のプログレスである n% の表記がずっと0%から変わらなかった。
      もしかしたらこの時点で既にSSDまたはOSが破壊されているんじゃないかな、と思う。
    - 自動再起動自体は5分ぐらい経ったらかかった。この挙動って正しいのかな。
- **メーカーロゴが出るのが異様に遅い。** 大体1～2分以上。  
  メモリを大量に積んでいると起きやすいらしいが、16GBってそんな多くないでしょ。  
  そもそも今まではこんな遅くなく、長くても5秒ぐらいでメーカーロゴ出てきたし。
  - **メーカーロゴまでは出るが、そこから進まない。** Windows起動時のぐるぐるが出てこない。
    多分UEFIがブートローダを見つけられていないか、ブートローダがWindowsを見つけられていないような。
    - でもブートローダがWindowsを見つけられないならちゃんとエラーは出るよな。やはり前者か？
- **SSDアクセスランプが周期的に点滅している。**
  - ワンチャンUEFIのエラー表示なんじゃね？とは思うが...実際のところどうなんだろ。  
    マウスコンピューターの中の人しか分からんか。[^6]
- 接続している全ての周辺機器を取り外しても**これら諸症状は改善せず。**

## 「やっておけばよかったなぁ」と思うこと

PC突然死が怖い人はできそうなやつやってみてもいいんじゃない。

- **定期的にバックアップを取る**
  - **マジでやっておけばよかった。** 特に地雷ドライバに上げる時にPC破壊を覚悟して色々やっておくべきだったのに。
  - そもそも「定期的にバックアップを取る」というのは**現代の計算機ユーザーには基礎中の基礎である。**
    将来的にはエンジニアを志す者が基礎を怠った結果全データ喪失の危機とかアホにも程があるだろ。
    - 一応ゲームとかは外部SSDに入れていたのでDL時間とかは食われずに済んだ。
      でもなんで大事なファイルも置いておかなかったんですかねえ。
- **別パーティションに別のOSを入れておく**
  - マジでやっておけばよかった2。軽くて小さいArch Linuxとか。
    別OSすら起動できなければSSDの問題だし、逆に起動できるならWindowsの問題だと切り分けられるため。
  - 起動さえできればWindowsのパーティションにアクセスしてデータをクラウドに退避させることなどもできたであろう。  
    後悔先に立たずとはこのことである。
- **大きな変更をする時は「覚悟」をする**
  - 特に「地雷ドライバ」にアプデする時とか。アレはマジでPCが壊れる可能性がある。
    過去GTX2xx系が主流だった頃に起きていたGPU破壊ドライバが最近復活気味らしい。何？
  - 常に大事なデータをバックアップしておくのは当たり前として、  
    地雷ドライバに上げたりする時はホームディレクトリ以下全部バックアップするとかそういうレベルでもいいと思う。
- **Windowsは定期的に再インストールをしたほうがよい**
  - 定期的に再インストールしないと安定性が保障されないOSって何だよって思うけどね。
    最近のWindows11は大丈夫らしいけど、それでも普通に怖い。  
    でもあまりに入れ直しすぎるとSSDのTBW超えやすいから程々に。
  - Windows10から11に上げる時とかも、「アップグレード」ではなく「クリーンインストール」をしたほうがいいまである。
    これは個人の感想ですが。**バックアップとかダルいしあんまり頻繁にやるもんではない。**(1行で矛盾)

# 修理に出すまでの時系列

一部あやふや。自分の認知を再確認するために。情報の正確性に欠けます。

## [疑惑] 5/26 未明 (GPUドライバアプデ)

正直疑惑に過ぎんけど。  
GPUドライバを546.01から当時最新バージョンの555.85にアップデートしている。

このバージョンは **俗に言う「地雷ドライバ」** で、インストールすると不具合がめちゃくちゃ出るタイプのドライバっぽい。  
正直もしかしたらこいつのせいでOSが破壊されて起動できなくなったんじゃね？という思いは今でもある。  

Twitterで「NVIDIA 555.85」で検索をかけてみると結構不具合情報が出てくる。  
~~なんでこんなバグまみれのドライバを正式版としてリリースしたんだ？[^1]~~ 

## 6/8 3時ぐらい (BSoD発生)

2024年6月8日午前3時頃。DiscordのVCを聞きつつChromeで動画を流しながらぼんやりしていたら急に音がバグり散らかす。具体的にはBSoDの時の「ブーーー・・・」という音。[^2]  
普通のBSoDであれば音がバグるのと同時に画面が真っ青 (か真緑) になり、停止コードとQRコードが出てくるはず。しかしそのときは待っても待ってもなかなか出てこなかった。

サウンドドライバがバグっただけか？と思いマウスやキーボードを適当に触るも一切の反応がない。  
動画の再生も完全に止まっていて、動いているのはタスクバー右端の時計の秒だけ。
5分ぐらい経ってやっと停止コード `UNEXPECTED_STORE_EXPCETION` で青画面になったが、そこでもまた5分ほど待たされた。  
「0% 完了」のままずっと止まっていてかなり気味が悪かった覚えがある。

> **今思えば、この時点でSSDかOSが死亡していたのかな...と思う。**
> `UNEXPECTED_STORE_EXCEPTION` はOSディスクの破損もしくはドライバの破損で発生しがちらしい。
> NVIDIA GPUドライバを5/26に危険Verである555.85に更新してしまっているため、それが原因かと考えたが、あとの症状と原因切り分けにて<small>ほぼ</small>否定される。

自動再起動後、本来なら1分立たずでメーカーロゴ表示～起動時ぐるぐる表示～ログイン画面表示 (Windows起動完了) までできるのになんか5分ぐらい経っても画面が暗転したまま。  
当初GPUドライバが壊れたかと思いつつしばらく放置していたらメーカーロゴが表示、**ここで「なんかやばいことが起きてる」ことを悟る。**

その後5分ぐらい待ってもWindows起動時のぐるぐるが出てこないため、本格的にブートローダ or Windows or UEFIがイカれたかな...と推測。

## 6/8 3時半～6時ぐらい (原因切り分け等)

明らかに普通じゃない数々の挙動を見て時間が解決してくれそうにないと思ったため、トラブルシューティングを開始。  
この時はまだ「GPUが壊れてUEFI/OSのチェックがバカ長くなってるだけじゃね？」と思っていたため、割と容赦ない行動をちらほら。  
無知は罪なり。[^3]

## 試したこと

この時、並行でDiscordで友人と相談 & マウスコンピューターのLINEサポートに連絡して原因切り分けを行っている。  
個人的にこのDiscordとLINEサポートが心の支えになった感じがする。私1人でこの状況に居るのは精神的にかなり辛いものがあった。  
相手は文字でしか話せないけど、文字だけでも「向こうには人がいる」ということを感じることができたのは本当に助かった。ありがとう。

### 再起動 (だめ)

困ったら再起動ということで。電源ボタン長押しで再起動。  
以前近いようなことが起きた時も何回か起動中に再起動かけて回復環境に入るなどの手法でパワー解決した覚えがある。  
<s>もしかしてこの時からSSDに異常が生じ始めていたのでは...？</s>

### 周辺機器全て取り外して再起動 (だめ)

なんかあったときは周辺機器全て外して再起動ということで。  
外付けSSDを繋いでいたのでワンチャンそいつが悪いか？という予想も立てていたが、まあ結果は変わらず。

> マウスのPCはディスク周りに地雷を抱えていることがあるらしい。(フレンド談)  
> 本当かどうかは知らんけど、今後マウス製PCでディスク周りが怪しいときは外したり色々試してみよう。

### 回復環境に入れるか挑戦 (だめ)

Windowsが起動しないときはとりあえず回復環境に入れるか試してみるとよいらしい。  
回復環境に入れればセーフモードに入ったりUEFIに入れたりするので。

しかし**そもそもWindowsのブートローダが呼び出されていない雰囲気がある**[^4]ため回復環境に入るとかどうとかのレベルの問題ではない。  
Windows起動時のぐるぐるが出ていないと回復環境に入るための操作すら叶わない。
つまり回復環境に入ることは不可能である。ヤンナルネ。

### UEFI画面に入れるか挑戦 (だめっぽい？)

とりあえずUEFIに入れるかテスト。入れるならUSBからブートできるのでUSB Linuxで入ってSSDとかクラウドに逃がそうという寸法。  
このPCは起動直後にF2連打でUEFI画面に入れるらしいので、電源ボタンを押して速攻F2連打。

しかし何も起こらなかった。F2以外にもいろんなキーを連打してみたりFnキー押しながらやってみたけどだめ。  
なんかこの時点でUEFIがおかしいような気持ちになってくる。

### CMOSクリアやBIOS復旧機能を探す (むり)

BIOS復旧機能はそもそもない。少なくともマウスのコンシューマ向けPCにはなさそう。  
HPのPCとかにならあるらしい。すごい。

CMOSクリアはワンチャンいけるか？と思ったが、背面カバーを開ける必要がある。つまり修理が受けられなくなる可能性が高い。
流石に修理を受けられるチャンスを棒に振るのは怖いのでやめた。いくじなしめ。
というかノートの場合マザボに直接つけられてることがあってどうしようもないケースが多いらしい。

### ひたすら放置する (だめ)

2時間ほどメーカーロゴ画面で放置。結果は変わらず。
この時点でもう諦めた。多分SSDかOSが終わってる。

デスクトップPCならSSD引っこ抜いたりできるけど、ノートなのでほぼ全バラが必要。
変なことして破壊しても嫌なので大人しく修理に出すことに。

## 6/8 22時ぐらい 修理依頼を出す

やり方はマウスコンピューターのサイト見たら分かるのでかなり省略気味。

### LINEサポートの指示に従う

LINEサポートに泣きついたとき、サポートの人もだめだと判断したのか、「[トラブルシューティング　Windowsが起動しない　ノートパソコン](https://www2.mouse-jp.co.jp/ssl/user_support2/sc_faq_documents.asp?FaqID=26506)」のリンクと共に **「35kまで負担してくれんなら超過分はこっちで負担する修理方法あるよ」** ってのを教えてくれた。うれしい。

当初修理依頼シートを印刷して書いたりしなきゃいけないのかと思ったが、それだとだめで、ネット上で修理申し込みをした。
元々付いてるSSDは消えたらマジで困る[^7]ので、症状詳細欄に「SSD交換することになったら元のは同梱して返してくれ」という旨を記述。
支払いは銀行口座振込とか色々あるけど **代金引換じゃないと上の35k負担の奴は使えない** っぽいので注意。
修理依頼の作成が終わったら出てくる仮修理番号とかサービスセンターの住所をメモ。写真撮っとくと楽。

## 6/8 23時ぐらい 梱包

発送のためにPCを箱に詰めてやる必要がある。当たり前。
**めんどくさがりのためにヤマト運輸を使って客先まで取りに来てくれるサービスもあるらしい。** 使ったことないから分からん。使えばよかったかな。
今回は既に無償修理保証は切れているため、PC本体とACアダプタ、そして仮修理番号とシリアルナンバー、「修理依頼シートにも書いたけどSSD交換するなら元の奴返してね」という旨を書いたメモを同梱。[^5]

梱包のための箱は元々デスクトップPCかなんかが入っていた箱をそのまま転用。結構デカかった。
PC本体とACアダプタを別々にプチプチで包み、タオルでつつみ、さらにその上にプチプチを被せる三段構え。
箱を動かしても少なくともPC本当に衝撃が入らないよう、四方 + 上下に新聞紙を入れ、箱を閉じてガムテープで閉鎖。
今思えば下方向がちょっと緩衝甘かった気がする。まあ精密機器マーク書かれてる & 取り扱い注意のシール貼ってもらってるし乱暴な扱いはされないでしょ。

PCを箱に詰めたあと、仮修理番号とシリアル番号、そして念には念をで「修理依頼シートにも書いたけどSSD交換することになったら元の返して」という旨を記述したメモを同梱。
9日お昼頃に発送。

## 6/9 12時ぐらい 発送

発送方法は至って単純で、コンビニとかに持っていって **元払いで** 発送する。
その際備考欄にシリアル番号を書いておく必要がある。うっかり書き忘れると死。

なんだかんだもう5回ぐらいコンビニから物送ってるけど、未だに送り状を書くのが苦手。
都道府県とか書く所と営業用コードを書く所を間違えたりした。かなしい。
**品名のところにシリアルナンバーを書かないと意味ないので注意。** うっかり書き忘れた場合はどうすればいいんですかね。

箱がデカいこともあって120サイズ料金に。2000円ぐらいした。
送る時間帯が悪かったのか翌日到着ではなく11日到着に。かなしい。
工場は土日休みなので過去事例から判断すると多分返ってくるのは15日～17日ぐらいかな？

## 6/10 未明 着

最寄りのサービスセンターに到着。多分昼前ぐらいじゃないかな。
なんか納入されたり出荷したりしたときにメールをくれるらしいけど、それらしきメールは来なかった。なんでだろ。

おそらく修理が始まったのは11日からだと思う。報告書を見た感じ11日に始まって12日に終わったように書いてある。

## 6/12 14時ぐらい 一次到着

スピード感がすごい。納入されて2日で出荷された。
発送は日本通運を使ってるみたい。前はヤマト運輸だったような気がする。

ただ、平日のお昼に代引の荷物を配達してくるのはどうかと思う。普通平日のお昼っていないのよ。
学校で電話が来たけど、授業中に電話取って「はいもしもし」なんてできるわけない。

## 同日 16時ぐらい 到着

再配達届をポストから回収して電話で再配達依頼。電話してから10分ぐらいで来てくれた。
3万半かかるのかなぁと思ってちょっとビビってたけど1万ちょっとで済んだ。
「**最高で**3万半かかる」ってだけで、普通はそんなかからんのかもね。もうちょっと人の話は聞いたほうがいい。

PCを受け取ったら開封してセットアップ。いつもの儀式。
修理依頼シートに「SSDかえして」って書いたおかげで古い死んだSSDも一緒に帰ってきた。これはあとで復旧チャレンジに使う。

修理詳細は「M.2 SSDの不具合でWindowsが起動できないのを確認したから交換したよ」という感じ。
M.2交換してOS再インストールしてくれた。最後に入れてたWindows11かと思ったらまさかの購入時のOS。

Windows10 21H1とかいうかなり懐かしいバージョン。2年前にサービス終了してる。
流石に使い物にならない...というかセキュリティ面がかなり怪しいためネットに繋いだあと即アプデ。
様々なドライバもついでに入ってくる。GPUドライバはもうこのバージョンでいいや。WUで降ってくるぐらいには安定してるってことだろうし。

# とりあえず復旧

2024年6月12日19時13分現在、とりあえず最低限使えるレベルまで復旧した。
データを失ったという喪失感はもちろんあるが、精神安定剤であるタルコフやVRChatがまた遊べるようになったという安心感がギリギリ打ち消してくれている。

3年間のVRChatの写真を失ってしまったのがかなり精神に来る。
自宅に腐らせてるラズパイとSSDを活かしてNASを構築する日が来たのかもしれない。がんばるぞ～

[^1]: 最近のNVIDIAは不具合まみれのドライバを出してる感じしかない...。
[^2]: これどういう原理でこういう音になるんでしょうかね。バッファがないから？
[^3]: 誤用。
[^4]: ブートローダが呼ばれているならぐるぐるが出るはず。
[^5]: もしかしたらシリアルナンバーは書かなくてもよかったかも。
[^6]: もしそうならトラブルシューティングページとかに書いといてほしい。
[^7]: 流石に3年分のVRChatの写真やバックアップコードが消えるのはしんどい。
[^8]: これ仕様かもしれん。
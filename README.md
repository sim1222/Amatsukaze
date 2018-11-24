# Amatsukaze

Automated MPEG2-TS Transcoder

## [ダウンロードはこちら](https://github.com/nekopanda/Amatsukaze/releases)

## これは何？

TSファイルをエンコードしてmp4やmkvにするソフトです。

### 対応入力フォーマット

- コンテナ: MPEG2-TS 188バイトパケット
- 映像: MPEG2, H264
- 音声: MPEG2-AAC

これだけです。

### 対応エンコーダ

x264, x265, QSVEnc, NVEnc

### 実装されている機能

- 各出力mp4(mkv)が単一フォーマットになるように必要に応じて分割
- タイムスタンプを元にAAC音声を無劣化で再構築
- デュアルモノAACを２つのモノラルAACに無劣化分離
- 字幕をSRTやASSに変換
- ニコニコ実況や2ch実況のコメントをASS字幕として追加（NicoConvAssが必要）
- 独自Avisynthソースクリップによる安定したチャプター・CM解析
- CMと本編の分離出力
- CMに本編とは別のビットレートを適用（一部エンコーダで制限あり）
- 複数話構成の番組を各話で分割
- ロゴあり区間を自動認識してロゴファイル生成
- TSファイルからチャンネルや日時を取得してロゴを自動選択
- チャンネルやジャンルからプロファイルを自動選択
- SCRenameによるリネームやジャンルごとにフォルダ分け
- 複数映像チャンネルが入ってるTSファイルは全チャンネル認識してエンコード
- AviSynthスクリプトによるフィルタ処理
- マルチパステレシネ判定によるVFR化
- x265の疑似VFRレートコントロール
- 高ビットフィルタ処理およびエンコード
- MPEG2ソースの量子化パラメータを使ったノイズリダクション
- 複数エンコーダの並列実行およびスケジューリング
- EDCBからの録画後自動エンコード

### 対応環境

Windows 7以降 (64bit)

### インストール方法

配布7zを解凍

### アンインストール方法

フォルダごと削除

## 使い方

### 1. 初回起動からエンコードまで

まずは、起動してエンコードしてみましょう。

#### 1-1. "Amatsukaze.vbs"を起動

<img src="https://i.imgur.com/SFTasoX.png" width="206">

#### 1-2. 「キュー」タブにTSファイルをドラッグ&ドロップ

<img src="https://i.imgur.com/bauBUKS.png" width="705">

プロファイル＆出力先選択パネルが出るので、「出力先」を設定して、「テスト」または「通常」を選択。

これでエンコードが始まります。

ただ、すべてデフォルトの状態だと、Amatsukazeの機能の半分も使えていません。
以下の説明を参考に、使いたい機能を設定してください。

### 2. エンコード設定

Amatsukazeはエンコード設定を「プロファイル」として保存します。
最初は「デフォルト」プロファイルがあるので、それをいじってみましょう。

#### 2-1. エンコーダのオプションを設定

デフォルトだと、x264をオプションなしで起動してエンコードします。
オプションを設定してみましょう。

「プロファイル」タブで「エンコーダ追加オプション」を入力します。

<img src="https://i.imgur.com/qgHUnoR.png" width="520">

CRF22にしてみました。プロファイルの設定を変更したら、「適用」ボタンを押します。
「適用」ボタンを押さないと、プロファイルに反映されないし、保存もされないので注意してください。

#### 2-2. 「キュー」タブにTSファイルをドラッグ&ドロップ

設定を変更したら、エンコードするファイルを投入してください。

#### Note: プロファイルについて

プロファイルにはエンコード設定が保存されています。別のプロファイルには別のエンコード設定が保存できます。TSファイルに適用するエンコード設定は、「キュー」タブにTSファイルをドラッグ&ドロップしたときに出る、プロファイル＆出力先選択パネルで選択したプロファイルの設定となります。

<img src="https://i.imgur.com/3kSCJU8.png" width="381.3">

最初は、「デフォルト」といくつかのサンプルプロファイルがあります。上で「デフォルト」プロファイルをいじったので、「デフォルト」プロファイルを選択したいときは「デフォルト」を選択しましょう。

プロファイル＆出力先選択パネルで選択できる項目には、自動選択と通常のプロファイルの2種類があります。自動選択は、 `自動選択_` で始まる項目で、これは、ファイルによって使うプロファイルを分けたい場合に使います。初期状態だと、「自動選択_デフォルト」がありますが、これはプロファイルを何も選択しない設定になっているので、これを選んでも、プロファイルが選択されないのでご注意ください。

### 3. QSVを使ってみる

#### 3-1. 「基本設定」タブでQSVEncCへのパスを設定

QSVは同梱されていないので、rigaya氏のサイトから入手して、以下のように「基本設定」タブでパスを設定してください。

<img src="https://i.imgur.com/dPjEfp6.png" width="570">

基本設定も設定を変更したら「適用」ボタンを押します。
こちらも、「適用」ボタンを押さないと、設定は反映されないし、保存もされないので注意してください。

#### 3-2. エンコード設定を変更

「プロファイル」タブに戻って、エンコーダを「QSVEnc」、追加オプションを入力します。「適用」ボタンも忘れずに。

<img src="https://i.imgur.com/0bzLXG4.png" width="515">

"--la-icq"モードは古いCPUだとサポートされていないので、注意してください。
エンコーダのオプションが間違っていると、Amatsukazeによるエンコードは開始しますが、エンコーダ（QSVEncC）を立ち上げたところで、エラーを吐いて終了します。

#### Note: 同じような手順でNVEncも設定できます。

<img src="https://i.imgur.com/aEDxg7r.png" width="463">←NVEncのパス設定

### 4. チャプター・CM解析

#### 4-1. チャプター・CM解析を有効にする。

チャプター・CM解析は、エンコード設定の「チャプター・CM解析を無効にする」のチェックを外すと有効になります。

<img src="https://i.imgur.com/EJgkDlB.png" width="520">

#### 4-2. 「キュー」タブにTSファイルをドラッグ&ドロップ

この状態でTSファイルを投入すると、最初はロゴファイルがないので、エンコードが始まりません。

<img src="https://i.imgur.com/vsipmJb.png" width="520.6">

以下の手順で、ロゴを生成します。

#### 4-3. 「ロゴ生成」を起動

キューのペンディング状態となってしまったファイルを右クリック→ロゴ生成

<img src="https://i.imgur.com/iR79QvA.png" width="429">

※ワンセグは対応していないので、常に失敗します。

#### 4-4. ロゴ位置を選択

ロゴを囲むようにマウスドラッグで選択（必要に応じて下部のタイムバーを操作）

<img src="https://i.imgur.com/1Pnbhnp.png" width="149">

#### 4-5. ロゴスキャン

「ロゴスキャン開始」をポチって、待つ

<img src="https://i.imgur.com/6IrmuvF.png" width="151">

#### 4-6. 結果を確認して「採用」or「キャンセル」

<img src="https://i.imgur.com/GFfZSba.png" width="285">

「採用」するとロゴが追加されてエンコードが始まります。

### 5. 字幕処理

#### 5-1. 字幕を有効にする。

字幕は、エンコード設定の「字幕を無効にする」のチェックを外すと有効になります。

<img src="https://i.imgur.com/1Ey9KyG.png" width="520.6">

#### 5-2. 「キュー」タブにTSファイルをドラッグ&ドロップ

これで、字幕が有効になり、字幕のあるTSなら、字幕がSRTorASSに変換されて出力されます。

#### 5-3. DRCS外字マッピングを追加

字幕を処理すると、DRCS外字マッピングがないというエラーが出ることがあります。
その場合はDRCS外字パネルにマッピングのない文字の画像が出てくるので、マッピングする文字を入力して「マッピング追加」してください。

<img src="https://i.imgur.com/vt6wVfU.png" width="238">←マッピング設定例

「マッピングのない文字」を見つけたら、この操作を行ってください。

#### 5-4. リトライ

失敗した項目は右クリック→リトライでリトライできます。

<img src="https://i.imgur.com/JO1UxAM.png" width="490">

### 6. 一時フォルダについて

一時フォルダは中間データが吐かれるフォルダです。SSDなどのなるべく速いディスクのフォルダを設定しましょう。

#### 6-1. 「基本設定」タブで一時フォルダを設定

<img src="https://i.imgur.com/IukOtCC.png" width="507.3">

設定したら「適用」ボタンを忘れずに。

## AmatsukazeCLI

実際のエンコードはAmatsukazeCLI.exeがバックエンドで動いています。
コマンドラインやバッチファイルから AmatsukazeCLI.exe を直接使うこともできます。
オプションや使い方は、引数なしでコマンドを打つと表示されるヘルプを見てください。

## 設定や動作について

下に行くほど、あまり重要じゃない項目になっています。

### プロファイル

ver0.4.0.0よりエンコード設定はプロファイルに保存されます。

エンコード設定は、エンコードするTSファイルを追加するときに、適用するプロファイルを選択します。つまり、エンコードするTSファイルのエンコード設定は、キューに追加されたときに適用したプロファイルの設定となります。

一旦プロファイルがエンコードするファイルに適用されると、そのファイルのエンコード設定は「プロファイル再適用」されない限り変更されません。プロファイルの設定を変更して、すでにキューに入っているアイテムに、変更を反映したい場合は、「プロファイル再適用」してください。（「リトライ」や「プロファイル変更」でもプロファイルは再適用されます。）

プロファイルは`profile`フォルダに保存されます。
他のAmatsukazeで保存したプロファイルを`profile`フォルダに入れてやればプロファイルが追加されて選べるようになります。

### プロファイル自動選択

ver0.5.0.0より、ファイルによって適用するプロファイルを自動で選択する、プロファイル自動選択が利用できます。「自動選択」パネルで、条件と選択するプロファイルを設定して、ファイル追加時のプロファイル選択で、この自動選択プロファイルを選択すると、プロファイルが設定した条件で自動選択されます。自動選択プロファイルは、プロファイル選択時は、 `自動選択_` で始まる名前になっています。例えば、自動選択「デフォルト」は、プロファイル選択時は、「自動選択_デフォルト」となっています。

### チャプター・CM解析

チャプター・CM解析が実装されています。（中身はjoin_logo_scpです）
独自形式のロゴファイルが必要なので、上で説明した手順でロゴを採取してください。

採取したロゴは"logo"フォルダに入れられます。
以後、同じチャンネルのTSファイルはこのロゴファイルが使われます。

TSファイルによっては、うまくロゴが採取できないことがあります。
（ロゴ周辺の背景が単一色になっているフレームが必要です。）
失敗する場合はアニメとかで試してください。

途中で映像フォーマットが変わるようなTSファイルは
「ロゴ生成」だとうまくいかないことがあります。
その場合は、「ロゴ生成（TS解析あり）」を選択してください。
起動にちょっと時間がかかりますが、コチラのほうが確実です。

あと、ロゴ生成ウィンドウはいくつでも起動できるので、
待つのが嫌なら、どうぞいくつでも同時に走らせてください。
（ただし、ちょっとHDDがやばいかもｗ）

### チャンネルごとの設定

デフォルトだと、該当ロゴファイルがないTSファイルはエンコードが始まりません。
ロゴがないチャンネル（AT-Xなど）、もしくはロゴを使う必要がないチャンネルは
「チャンネルごとの設定」タブで、当該サービス（＝チャンネル）を選択して、
右のロゴリストを右クリック→「ロゴなし」を追加で
サービス（＝チャンネル）にロゴなし設定を追加してください。

<img src="https://i.imgur.com/4SPX6KV.png" width="490">

チャンネルごとのjoin_logo_scpコマンドフィイルもこのタブで設定できます。

また、ロゴに期間を設定することもできます。「ロゴなし」も期間を設定できます。
期間はTSファイルに時刻情報がある場合のみ有効です。

ロゴはいくつでも追加できます。
同じチャンネルのロゴが複数ある場合は、マッチするロゴを自動で探します。
ロゴが変わってたら採取し直してロゴを追加してください。
一度「採用」したロゴを消したい場合は、"logo"フォルダから当該ファイルを消してください。

チャンネル名、時刻などの情報は、TSファイルから取得するようになっています。
TSSplitterなどの処理により、TSファイルからこれらの情報を取り除いてしまった場合、
チャンネル名が取得できなかったり、期間設定が反映されなかったりするので、
少し使いにくくなります。（チャンネル名が「不明」や「情報なし」になります）

これらの機能は、放送ソースを前提にしたものです。TSファイルが放送ソースでない場合は
「チャプター・CM解析を無効」にすれば一応使えると思います。

### 字幕

ARIB字幕では、文字セットに定義されていない文字を、ビットマップ画像で送ってくることがあります。
これをDRCS外字と言います。画像をSRTやASSに埋め込むのは難しいため、Amatsukzeは通常の文字に変換します。
画像を認識できるようにはなっていないので、この変換はユーザが設定する必要があります。
「DRCS外字」パネルで設定してください。

ASS字幕はMPC-BEでレイアウトが正しく表示されることを確認しています。
他のプレーヤーだとレイアウトが正しく表示されないことが多いです。
これに関してはレイアウトの互換性が低すぎてどうしようもないです・・・。
あと、サロゲートペアが認識されません。

SRT字幕はレイアウト情報はなく、文字だけです。ルビなどの小さい文字も省略されています。
こちらはサロゲートペアも表示されるようです。

MKVで出力すればSRTとASS両方がMKVファイルに組み込まれます。
MP4はASSに対応していないためSRTのみが組み込まれ、ASSは別ファイルとして出力されます。

### VFR（可変フレームレート）

付属のフィルタにKFMによるVFR出力があります。
Amatsukazeでは、VFRで24fps判定されたフレームのタイミングを60fpsと120fpsから選択できます。
60fpsタイミングは、TSソースのタイミングをなるべく忠実に再現した、23プルダウンと同等のタイミングで出力します。120fpsタイミングの場合、24fpsフレームは、120fpsのタイミングに沿って等間隔で出力されます。

### EDCBから自動エンコード

「その他」→「EDCB用録画後実行バッチ」で、EDCBで録画後、Amatsukzeで自動エンコードするためのバッチファイルが生成できます。

必要な設定をして、「バッチファイル作成」から、バッチファイルを生成し、EDCBの録画後実行バッチのところに生成したバッチファイルへのパスを設定してください。

エンコード自体はAmatsukazeサーバで実行されます。生成したバッチファイルは、Amatsukazeサーバにタスクを投げるのが仕事です。Amatsukazeサーバが起動していない場合は、バッチファイルから起動を試みます。あらかじめ起動しておけば、起動してるAmatsukazeサーバにタスクを投げます。エンコード状況の確認やリトライなどの操作は、Amatsukazeクライアント(AmatsukazeClient.vbsで起動)から接続して行ってください。Amatsukaze.vbsから起動するのはスタンドアロンバージョンなので、エンコード状況の確認はできません。

EDCBがサービスで動いてて、EpgTimerもAmatsukazeサーバも立ち上げていない場合、AmatsukazeServerCLIがサービスで動いてるEDCBから起動されます。結果、Amatsukazeサーバがシステムアカウントで動作するので、環境によっては、AviSynthプラグインがエラーを吐くことがあるようです。システムアカウントで動いてるAmatsukazeサーバでエラーが出る場合は、手動でAmatsukazeServer.vbsからAmatsukazeサーバを起動しておくようにしてください。なお、システムアカウントで起動しているAmatsukazeサーバは、Amatsukazeクライアントから「その他」→「Amatsukazeサーバ操作」で終了できます。Amatsukazeサーバは多重起動できないようになっているので、起動しないと思ったら、Amatsukazeクライアントで接続してみてください。

録画PCとエンコードPCを別にしたい場合は、[録画後リモートエンコード](https://github.com/nekopanda/Amatsukaze/wiki/%E9%8C%B2%E7%94%BB%E5%BE%8C%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E3%82%A8%E3%83%B3%E3%82%B3%E3%83%BC%E3%83%89)を参照してください。

### ニコニコ実況

[NicoConvAss](http://vb45wb5b.seesaa.net/)を使って[ニコニコ実況](http://jk.nicovideo.jp/)コメントの過去ログをASS字幕として追加できます。

#### 設定方法

1. [NicoConvAss](http://vb45wb5b.seesaa.net/)を入手

2. NicoConvAss.exeへのパスを設定

<img src="https://i.imgur.com/uL4TJm9.png" width="405">

3. ニコニコ実況コメントを有効化

<img src="https://i.imgur.com/ZFyOn5l.png" width="445">

「NicoJK18サーバからコメントを取得する」場合は、これだけでOKです。NicoJK18サーバを使わない場合は、[JKCommentGetter](https://github.com/ACUVE/JKCommentGetter)のセットアップが必要になります。

「NicoJKログから優先的にコメントを取得する」は、動作をNicoConvAssに依存しているので、これを有効にしたい場合は、NicoConvAssの設定ファイル`NicoConvAss.ini`の`NicoJK_path`を設定しておいてください。

NicoConvAssのパラメータを設定したい場合は、NicoConvAss.exeを起動して設定してください。

### [バッチファイル実行機能](https://github.com/nekopanda/Amatsukaze/wiki/%E3%83%90%E3%83%83%E3%83%81%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%AE%9F%E8%A1%8C%E6%A9%9F%E8%83%BD)

## CM位置入力

Trimファイルを `(TSソースファイル名).trim.avs` というファイル名でTSソースファイルと同じフォルダに置いておくと、そのファイルのCM位置情報を使います。

`(TSソースファイル名)` は拡張子まで含むファイル名です。例えば `hoge.ts`の場合、Trimファイルは `hoge.ts.trim.avs` という名前で置いてください。

Trimファイルとは、join_logo_scpの出力するCMカット情報のAVSファイルで、中身は例えば、以下のような1行で書かれたファイルです。

```
Trim(333,7285) ++ Trim(9084,26525) ++ Trim(28325,46336) ++ Trim(48135,48883)
```

スペース位置などを厳密に同じにする必要はありませんが、1行で同じように書いてください。

同じTSソースファイルでも、開くソフトによってフレーム番号は変わります。フレーム番号を正確に設定したい場合は、Amatsukazeが一時ファイルとして出力する、 `amts0.avs` のフレーム番号で設定してください。TSソースファイルが映像フォーマットの切り替わりを含む場合、CM解析は映像フォーマットごとに分離されたあとで実行されるので、 `amts1.avs` や `amts2.avs` が出力される場合があります。その場合は、最も再生時間の長いavsファイルを使ってください。ここで入力されるCM位置情報は、最も再生時間の長い映像フォーマットのファイルに適用されるからです。

## フィルタ設定

インターレース解除やノイズリダクション、リサイズなどのフィルタ処理ができます。

CUDAで処理する場合、Compute Capability 3.5以上のNVIDIA GPUが必要です。GeForceでも古いGPUやローエンドのGPUだと使えない可能性があるので注意してください。

ちなみに、ここで設定するフィルタの前に、内部ロゴ消しフィルタがあります。

### インターレース解除

KFM,D3DVP,QTGMC,Yadifの4つの方法から選択できます。

#### KFM

KFMは、映像のフレームレートを判別して、24p,30p,60p部分にそれぞれ別処理を施すフィルタです。
24pは逆テレシネ、30pはソースをそのまま、60pはQTGMC（CUDA版はKTGMC）によるインタレ解除を施します。
出力は24p,60p,VFRの3つから選べます。特に問題がなければインタレ解除は**KFMによるVFR推奨**です。

SVPによる60p化は、KFMで24p/30p判定された部分をSVPによるフレーム補間で60p化して、全体を60pで出力します。

SVPを使う場合は、SVPの[Avisynth and Vapoursynth plugins](https://www.svp-team.com/wiki/Download#libs)を入手して、"lib-windows\avisynth\x64"の中身を"exe_files\plugins64"に入れてください。

SMDegrainによるノイズリダクションは、時間軸方向の動きを検出して、似ている部分を平均化するフィルタです。モスキートノイズなどの細かなノイズを低減する効果があります。ただし、副作用として、グレインノイズや雨などのエフェクトが消えることがあります。

DecombeUCFは[DecombUCF](http://tyottoenc.blog.fc2.com/blog-entry-9.html)のアルゴリズムを使って、フレーム・フィールド置換します。

Amatsukazeに実装されているDecombUCFは、コア部分以外は[オリジナル](http://tyottoenc.blog.fc2.com/blog-entry-9.html)と
異なった処理となっています。オリジナルは24pにしか適用できませんが、60pにも適用できるように改良されています。
24pで片フィールドが汚い判定された場合、オリジナルは24pからbobフレームを生成しますが、
KFMDeintでは、30fpsソース上で、きれいなフィールドが2フィールド以上ある場合は、DoubleWeaveから取得するようになっています。
また、オリジナルではbobフレームはTDeintで生成していますが、KFMDeintでは除外フィールド指定KTGMCで生成するようになっています。
なお、除外フィールド指定はKTGMCのみの機能で、QTGMCには実装されていません。なので、CPU版はYadifで生成します。

#### D3DVP

D3DVPは、GPUのインタレ解除機能を使ったインタレ解除です。
PCで再生するときと同等の品質でインタレ解除します。
Windows 8以降のみ対応しています。**Windows 7では使えません。**

GPU指定自動の場合は、プライマリディスプレイに接続したGPUでインタレ解除します。
インタレ解除に使用するGPUを指定したい場合は、"Intel","NVIDIA","Radeon"を用意したので、
それを試してみてください。デバイス名の名前一致で探してくるので、
PCが認識したGPUのデバイス名が微妙に違ったりすると見つからなくてエラーになるかもしれません。
あと、当然ですが、PCにないGPUを指定したらエラーになります。

#### QTGMC

QTGMCは、ソフトウェア処理によるbobベースのインタレ解除フィルタです。
計算量が多く重いフィルタですが、その分、綺麗にインタレ解除できます。
ただし、24pや30pの映像に適用すると、本来は存在しない中間フレームが生成されるなどのデメリットがあります。

処理時間に関しては、CUDAで処理すればかなり速くなります。CUDA版はKTGMCが呼ばれます。(GTX1060でフルHDが110fpsくらい）。

AmatsukazeのQTGMCフィルタは、QTGMC単独のフィルタではありません。NNEDI3（Bob化）ベースのQTGMCなので、そのままだと細かい文字等が潰れます。KFMモジュールに実装されている、KStaticAnalyzeとKStaticMergeを使って、止まっている細かい文字等をソースから補間しています。

#### Yadif

Yadifはソフトウェアインターレース解除としてもっと広く使われているフィルタです。高速に処理できますが、画質はKFMやQTGMCと比べると、どうしても見劣りします。

### デブロッキング

MPEG2ソースの量子化パラメータを使ったノイズリダクションです。圧縮時と同じ周波数空間で、圧縮時と同じ量子化パラメータを使って、ノイズ成分を除去します。

ノイズは元データには無い特徴が現れることなので、圧縮によって失われてしまったはずの成分を取り除くことで、ノイズを見えなくします。ただし、このときノイズと一緒に、ノイズに紛れた本来の特徴も一緒に取り除いてしまうことがあるので、強さを設定できるようになっています。

このフィルタはMPEG2ソースの量子化パラメータが必要なので、MPEG2デコーダを「デフォルト」に設定しておく必要があります。MPEG2デコーダが「デフォルト」以外では、このフィルタは無効になります。

### 高ビット処理フィルタ

時間軸安定化、バンディング低減、エッジ強調（アニメ用）、の３つは14bitで処理されます。３つのフィルタを処理した後、10bitに変換されて、エンコーダに入力されます。（３つのフィルタが全て無効の場合は、8bitのままエンコーダに入力されます。）

#### 時間軸安定化

KTemporalNRで、時間軸方向のディザっぽい成分を安定化させます。バンディング低減と一緒に使うとグラデーションがきれいにエンコードされます。

#### バンディング低減

AviUtlのバンディング低減MTフィルタを移植したものです。

#### エッジ強調（アニメ用）

AviUtlのエッジレベル調整フィルタを移植して、改造したものです。
オリジナルの処理に比べて以下の部分を改善しています。
- 文字とかのすでに十分シャープなエッジに適用すると、アンチエイリアスが失われてギザギザになってしまうのを回避するため、上限の閾値を設けて、十分シャープなエッジは除外
- 演出でボケている部分に適用しても、シャープになってしまうことがあるので、下限の閾値を設けて、ボケすぎてる部分は除外
- 適用部分が「荒くなる」のを防ぐため、RgToolsのRepair相当の処理で適用

### フィルタの速度について

以下のフィルタは、CPUでも高速に処理できます。

- Yadif
- D3DVP
- デブロッキング

これ以外のフィルタは計算量が多い、または、CPUに最適化していない、等の理由により、CPUで処理するとかなり遅いです。CUDAでの処理推奨です。


### 独自AviSynthフィルタを使う

GUIから設定できるフィルタとは違うフィルタ処理をしたい場合は
スクリプトを"avs"フォルダに入れれば、カスタムフィルタとして選択できるようになります。
ただし、メインフィルタは"メイン_"、ポストフィルタは"ポスト_"というプリフィックスで
パターンマッチしてるので、そういう名前にしてください。

GUIのフィルタ設定の「フィルタをテキストでコピー」すると、フィルタの内容がクリップボードにコピーされます。このテキストは、そのままメインフィルタとして使用できます。GUIのフィルタ設定をベースに改変したい場合は、ここでコピーしたフィルタをベースに改変すると良いです。

GUIからコピーしたスクリプトを見れば分かりますが、入力は AMT_SOURCE という変数で渡されます。
メインフィルタの入力は、ソースがインターレースの場合は常にTFFです。
AviSynthには、クリップがインターレースかプログレッシブかを識別できる
ような機能はないので、便宜的にTFFをインターレース、BFFをプログレッシブと解釈しています。
インタレ解除するフィルタはAssumeBFF()でBFFで出力してください。

解像度を変更すると、出力のサンプルアスペクト比(SAR)は1:1と解釈されます。
解像度を変更しなければ、入力のサンプルアスペクト比はそのまま保持されます。
AviSynthのクリップにはサンプルアスペクト比を保持する機能がないので、
入力クリップのアスペクト比を取得する機能はありません。
もし必要なら解像度で条件分けとかしてください。

フィルタで処理されるのは映像だけです。音声は使われません。

音ズレを防ぐため、フィルタ出力の映像の時間が入力と一致しない場合はエラーとします。
カット処理や可変フレームレートには対応しません。

また、デフォルトだとシステムにインストールされているAviSynthとの干渉を防ぐため、
デフォルトのプラグインオートローディングは無効化されていて、
"exe_files/plugins64"のプラグインしか使えないようになっています。
「システムにインストールされているAviSynthプラグインを有効にする」を
チェックすると、デフォルトのプラグインオートローディングが有効になります。


### ハッシュチェック

入力TSファイルがネットワーク上にある場合、デフォルトでハッシュチェックが有効になっています。
これは、エンコードする時のネットワーク転送でデータ化けが発生していないかをチェックするAmatsukazeGUIの機能です。
（AmatsukazeCUIにこの機能はありません。）

この機能を使うには、事前にBatchHashChecker.exeでハッシュ値ファイルを生成しておく必要があります。
必要ない場合は、設定でハッシュチェックを無効にしてください。

#### BatchHashChecker

BatchHashChecker.exeは、ファイルがデータ化けしてないか、チェックするためのユーティリティです。
主に、ネットワーク転送でのビット化けや、HDD読み取りエラーでのデータ化けなどを検出するのが目的です。

例えば、D:\MyFolderにあるファイルをチェック対象にしたい場合は、以下のコマンドで、ハッシュ値ファイルを作成しておきます。
```
BatchHashChecker.exe m D:\MyFolder
```
このコマンドはフォルダ中の全てのファイルのハッシュ値を計算し、D:\MyFolder.hash に保存します。

チェックしたい時は、以下のコマンドを実行します。
```
BatchHashChecker.exe c D:\MyFolder
```
これで、ハッシュ値ファイルを作成した時点から、データが変化していないかをチェックできます。

ハッシュ値ファイルには、ファイルの相対パスが保存されているので、MyFolderを別の所に移動しても、
MyFolder.hashも同じところに移動すれば、チェックできます。

ハッシュ値ファイル自体も、ハッシュ値によるデータ化け検出ができるようになっています。
ハッシュ値ファイルの最後にハッシュ値ファイル自体のハッシュ値が記録されています。

ハッシュ値ファイルはテキストファイルなので、編集可能です。
編集するとハッシュ値が合わなくなるので、以下のコマンドでハッシュ値ファイル自体のハッシュ値を更新できます。
（D:\MyFolder.hashのハッシュ値を更新する場合）
```
BatchHashChecker.exe hu D:\MyFolder
```

#### Amatsukazeのハッシュチェック機能

ハッシュ値ファイルは、指定された入力TSファイルのあるフォルダの「フォルダ名.hash」ファイルを見に行きます。
このファイルがないとエラーになります。

ハッシュチェックが有効な場合、AmatsukazeGUIが出力mp4(or mkv)ファイルのハッシュ値も計算し、
出力フォルダの_mp4.hashファイルに書き込みます。

本来なら、ハッシュ値ファイルにファイルを追加する度に、
ハッシュ値ファイル自体のハッシュ値を再計算しなければならないのですが、
\_mp4.hashファイルはこれを省略しています。
そのため、このファイルにはハッシュ値がありません。
そのまま、出力mp4(or mkv)ファイルのハッシュチェックに使おうとするとエラーになります。
\_mp4.hashファイルが完成したら、
上で説明した`hu`コマンドで、\_mp4.hashファイルのハッシュ値を更新すれば、
BatchHashChecker.exeで出力mp4(or mkv)ファイルのハッシュチェックができるようになります。

### 制限、未実装項目

- VFRな入力（ワンセグなど）には対応していません
（VFR出力には対応しています。）
- 「通常」出力でQSVEnc,NVEncを使用した場合、CMビットレート倍率は適用されません
（x264,x265はzoneオプションで適用します）
- HEVCはインタレ保持に対応していないので、
インタレ解除しないでHEVCを使おうとするとエラーになります。
- QSVEncやNVEncの古いバージョンは高ビット入力に不具合があるので、注意してください。

## ライセンス

GPLのライブラリを組み込んでいるので、全体にGPLが適用されています。私の書いたコードはMITライセンスで提供します。
各プロジェクトと利用可能なライセンスは以下の通り。
- Amatsukaze: 一部GPLやその他のライセンスのコードを使っています。各ソースコードファイルにライセンスが書かれているのでそれを見てください。
- AmatsukazeCLI: MITライセンス
- AmatsukazeGUI: MITライセンス
- AmatsukazeUnitTest: MITライセンス
- BatchHashChecker: MITライセンス
- FileCutter: MITライセンス
- libfaad2: GPL

## 同梱&依存ライブラリ
- [FFmpeg](https://github.com/nekopanda/FFmpeg/tree/amatsukaze)(デブロッキングフィルタの精度を上げるため、少し改造しています)
- [FAAD2](http://www.audiocoding.com/faad2.html)
- [L-SMASH](https://github.com/l-smash/l-smash)
- [x264](https://www.videolan.org/developers/x264.html)
- [x265](https://bitbucket.org/Nekopanda/x265)（本家版はzonesを多く指定すると落ちるバグがあるので少し改造しています）
- [Ut Video Codec Suite](http://umezawa.dyndns.info/wordpress/)
- [AviSynthNeo](https://github.com/nekopanda/AviSynthPlus)
- [join_logo_scp 改造版](https://github.com/nekopanda/join_logo_scp)(DivFileコマンドを追加しました([改造元](https://mevius.5ch.net/test/read.cgi/avi/1484985868/794)))
- [chapter_exe 改造版](https://github.com/nekopanda/chapter_exe)（VFWに依存しないでAvisynthスクリプトを読めるように改造しています）
- [Livet](http://ugaya40.hateblo.jp/entry/Livet)
- [MP4Box](https://gpac.wp.imt.fr/mp4box/)
- [Caption.dll](https://github.com/nekopanda/TVCaptionMod2)（使いやすいように少し改造しています）
- [mkvmerge](https://github.com/mbunkus/mkvtoolnix)
- [OpenSSL](https://www.openssl.org/)
- VC14ランタイム

同梱AviSynthプラグイン

- [LSMASH Works](https://github.com/VFR-maniac/L-SMASH-Works)
- [QTGMC](http://avisynth.nl/index.php/QTGMC)
- [RgTools](https://github.com/pinterf/RgTools)
- [NNEDI3](https://github.com/jpsdr/NNEDI3)
- [mvtools](https://github.com/pinterf/mvtools)
- [masktools](https://github.com/pinterf/masktools)
- [AvsCUDA,KTGMC,KNNEDI3,KFM](https://github.com/nekopanda/AviSynthCUDAFilters)
- [SMDegrain](http://avisynth.nl/index.php/SMDegrain)
- [D3DVP](https://github.com/nekopanda/D3DVP)

Amatsukazeと同梱&依存ライブラリはすべて64bitに統一されています。

## ビルド方法

FFmpeg（ライブラリ）が必要です。Windowsビルドのdev版を落として、
libの中身をプロジェクトのlib/x64(or x86)フォルダへコピーします。

Ut Video Codec Suite（ライブラリ）が必要です。ソースを落として、
ビルドしてください。必要なのはutv_core.libだけです。これを
lib/x64(or x86)へコピーしてください。ビルドにはcygwinが必要なので注意。

AvisynthNeoが必要です。ソースを落として、ビルドしてください。
ビルドにはCMakeが必要です。AviSynth.libをlib/x64(or x86)へコピーしてください。

単体テストプロジェクト(AmatsukazeUnitTest)は、他にgoogletestのライブラリが必要です。
サブモジュールでgoogletestは追加してあるので、git submodule updateでコードを落として、
googletest/googletest/msvc/gtest-md.slnを開いて、ビルドしてください。
できたgtest.lib/gtestd.libをlib/x64(or x86)へコピーしてください。

単体テストプロジェクト(AmatsukazeUnitTest)は、他にOpenCVも使っているので、OpenCV 3.2.0をビルドしてこれもlibをlib/x64(or x86)へコピーしてください。

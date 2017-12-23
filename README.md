# Amatsukaze
Automated MPEG2-TS Transcoder

# 概要

## これは何？

TSファイルをエンコしてmp4やmkvにするソフトです。

## 対応入力フォーマット

- コンテナ: MPEG2-TS 188バイトパケット
- 映像: MPEG2, H264
- 音声: MPEG2-AAC

これだけです。

## 対応エンコーダ

x264, x265, QSVEnc, NVEnc

## 実装されている機能

- 各出力mp4(mkv)が単一フォーマットになるように必要に応じて分割
- タイムスタンプを元にAAC音声を無劣化で再構築
- デュアルモノAACを２つのモノラルAACに無劣化分離
- 字幕をSRTやASSに変換（mp4はSRTのみ）
- 独自Avisynthソースクリップによる安定したチャプター・CM解析
- CMと本編の分離出力
- CMに本編とは別のビットレートを適用（一部エンコーダで制限あり）
- ロゴあり区間を自動認識してロゴファイル生成
- TSファイルからチャンネルや日時を取得してロゴを自動選択
- 複数映像チャンネルが入ってるTSファイルは全チャンネル認識してエンコード
- AviSynthスクリプトによるフィルタ機能
- 高ビットフィルタ処理およびエンコード
- 複数エンコーダの並列実行

## 対応環境

Windows 7以降 (64bit)
（Windows 10でしかテストしてないから他で動くか分からん）

## 高品質インタレ解除を利用する場合の推奨環境

GTX 970または1060以上のNVIDIA GPUを搭載したPC

# 使い方

## インストール方法

配布zipを解凍

## アンインストール方法

フォルダごと削除

## セットアップ＆エンコード手順

初回起動からエンコードまでの手順を説明します。

### 1. "Amatsukaze.vbs"を起動

<img src="https://i.imgur.com/SFTasoX.png" width="206">

### 2. 「基本設定」タブで一時フォルダを設定

<img src="https://i.imgur.com/EPOVPUy.png" width="387">

設定したら「適用」ボタンを忘れずに。

※一時フォルダは中間データが吐かれるフォルダです。SSDなどのなるべく速いディスクのフォルダを設定してください。

他の設定も必要なら設定してください。

### 3. 「キュー」タブにフォルダorファイルをドラッグ&ドロップ

<img src="https://i.imgur.com/wfjuE7o.png" width="729">

これで通常はエンコードが始まりますが、最初はロゴファイルがないので、エンコードが始まりません。以下の手順で、ロゴを生成します。

### 4. ロゴを生成

4-1. 「ロゴ生成」を起動

キューのペンディング状態となってしまったファイルを右クリック→ロゴ生成

<img src="https://i.imgur.com/iR79QvA.png" width="429">

※ワンセグは対応していないので、常に失敗します。

4-2. ロゴ位置を選択

ロゴを囲むようにマウスドラッグで選択（必要に応じて下部のタイムバーを操作）

<img src="https://i.imgur.com/1Pnbhnp.png" width="149">

4-3. ロゴスキャン

「ロゴスキャン開始」をポチって、待つ

<img src="https://i.imgur.com/6IrmuvF.png" width="151">

4-4. 結果を確認して「採用」or「キャンセル」

<img src="https://i.imgur.com/GFfZSba.png" width="285">

「採用」するとロゴが追加されてエンコードが始まります。

### 5. DRCS外字マッピングを設定

5-1. DRCS外字マッピングを追加

字幕のあるTSファイルだと、DRCS外字マッピングがないというエラーが出ることがあります。
その場合はDRCS外字パネルにマッピングのない文字の画像が出てくるので、マッピングする文字を入力して「マッピング追加」してください。

<img src="https://i.imgur.com/vt6wVfU.png" width="238">←マッピング設定例

「マッピングのない文字」を見つけたら、この操作を行ってください。

5-2. リトライ

失敗した項目は右クリック→リトライでリトライできます。

<img src="https://i.imgur.com/JO1UxAM.png" width="490">

処理が開始されると、フォルダにencoded,failed,succeededという3つのフォルダが作られ、encodedにエンコード済みmp4ファイル、succeededに成功したTSファイル、failedに失敗したTSファイルが入れられます。

### QSVEnc,NVEncのパス設定

QSVEnc,NVEncを使いたい場合は、別途入手してパスを設定してください。

<img src="https://i.imgur.com/aEDxg7r.png" width="463">←NVEncのパス設定

## AmatsukazeCLI

実際のエンコードはAmatsukazeCLI.exeがバックエンドで動いています。
コマンドラインやバッチファイルから AmatsukazeCLI.exe を直接使うこともできます。
オプションや使い方は、引数なしでコマンドを打つと表示されるヘルプを見てください。

# 設定や動作について
下に行くほど、あまり重要じゃない項目になっています。

## [エンコード設定例](https://github.com/nekopanda/Amatsukaze/wiki/%E3%82%A8%E3%83%B3%E3%82%B3%E3%83%BC%E3%83%89%E8%A8%AD%E5%AE%9A%E4%BE%8B)

## チャプター・CM解析

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

## チャンネルごとの設定

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

## 字幕

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

MKVで出力すればSRTとASS両方が出力されます。MP4はASSに対応していないためSRTのみが出力されます。
SRTはレイアウト情報やルビがないため、情報を失いたくない場合はMKVで出力してください。

## メイン/ポストフィルタ

AviSynthによるフィルタ処理です。
デフォルトで利用可能になっているメインフィルタ用スクリプトは
QTGMCという激重のインタレ解除フィルタです。

CUDA版を使えばかなり速くなります(GTX1060でフルHDが110fpsくらい）。
が、Compute Capability 3.5以上と、比較的新しいそれなりのGPUが必要です。
CUDA版が使えない場合は、CPU版は遅すぎるのでフィルタなしインタレ保持でのエンコードを推奨します。

24fps部分の部分の画質を上げたい場合は、「24pありNR」が使えます。
出力は60fpsですが、24fpsだと分かる部分は、24fps化した上で2項分布マージ版SMDegrainでノイズリダクションします。
ただし、24fps化の精度は100%ではないので、稀に誤爆することがあります。
24fps処理が必要ない場合は、「24pなし」を選択してください。NRなし「24pあり」はあまりメリットがありません。

デフォルトで利用可能になっているポストフィルタ用スクリプトは
時間軸平均化（独自）+デバンド（AviUtlのバンディング低減MT移植版）+
エッジ強調（AviUtlのエッジレベル調整の移植＆選択的に適用するようにした改造版）です。
14bitで処理して10bitで出力します。
10bitでエンコードする想定で作っているので、8bitでエンコすると
ソースよりもバンディングはひどくなるかもしれません。

メインフィルタ→ポストフィルタの順にフィルタ処理されます。
また、メインフィルタの前に内部ロゴ消しフィルタがあります。

## AviSynthフィルタ

同梱スクリプトとは違うフィルタ処理をしたい場合は
スクリプトを"avs"フォルダに入れれば選択できるようになります。
ただし、メインフィルタは"メイン_"、ポストフィルタは"ポスト_"というプリフィックスで
パターンマッチしてるので、そういう名前にしてください。

同梱スクリプトを見れば分かりますが、入力は AMT_SOURCE という変数で渡されます。
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

## 同梱フィルタの詳細

同梱フィルタのCUDA版は、ほぼすべての処理をCUDAで行うため、高速です。

### メインフィルタ

メインフィルタはQTGMCによるインタレ解除がメインです。
Preset="Faster"でインタレ解除します。CUDA版はKTGMCです。

NNEDI3（Bob化）ベースのQTGMCなので、そのままだと細かい文字等が潰れます。
KStaticAnalyze+KStaticMergeで止まっている細かい文字等をソースから補間しています。

24pパスありの場合、KFMFrameAnalyze+KFMCycleAnalyzeで24p化したクリップを作成し、
確実に24pであると分かる部分はKFMSwitchでマージしています。

「24pありNR」は、24pクリップに2項分布マージONでKSMDegrainを適用します。
これはQTGMCのMDegrainに相当する処理です。NRなしの「24pあり」は、24pクリップに
QTGMCのMDegrain系処理が一切ないため、QTGMCで処理した場合に比べてノイズが目立つようになります。
24pありにする場合は、NRが必須だと思います。

### ポストフィルタ

ポストフィルタは、デバンドとエッジ強調を行うフィルタです。
最初に14bit化して、14bitで処理します。

まず、KTemporalNRで、時間軸方向のディザっぽい成分を安定化させてから、
KDebandで、デバンドします。KDebandはAviUtlのバンディング低減MTフィルタを
移植したものです。

次に、KEdgeLevelでエッジ強調します。KEdgeLevelは
AviUtlのエッジレベル調整フィルタを移植して、改造したものです。
オリジナルの処理に比べて以下の部分を改善しています。
- 文字とかのすでに十分シャープなエッジに適用すると、アンチエイリアスが失われてギザギザになってしまうのを回避するため、上限の閾値を設けて、十分シャープなエッジは除外
- 演出でボケている部分に適用しても、シャープになってしまうことがあるので、下限の閾値を設けて、ボケすぎてる部分は除外
- 適用部分が「荒くなる」のを防ぐため、RgToolsのRepair相当の処理で適用
- エッジ付近に本来存在しない色が出現するのを防ぐため、chroma成分にも適用

最後に14bit→10bit変換して出力します。10bitでエンコードすれば、
デバンドが保たれたままエンコードできると思います。

## ハッシュチェック

入力TSファイルがネットワーク上にある場合、デフォルトでハッシュチェックが有効になっています。
これは、エンコードする時のネットワーク転送でデータ化けが発生していないかをチェックするAmatsukazeGUIの機能です。
（AmatsukazeCUIにこの機能はありません。）

この機能を使うには、事前にBatchHashChecker.exeでハッシュ値ファイルを生成しておく必要があります。
必要ない場合は、設定でハッシュチェックを無効にしてください。

### BatchHashChecker

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

### Amatsukazeのハッシュチェック機能

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

## 制限、未実装項目

- VFRな入力（ワンセグなど）には対応していません
（NVEncのvpp-afsを使ったVFR出力には対応しています。）
- 「通常」出力でQSVEnc,NVEncを使用した場合、CMビットレート倍率は適用されません
（x264,x265はzoneオプションで適用します）
- HEVCはインタレ保持に対応していないので、
インタレ解除しないでHEVCを使おうとするとエラーになります。
- QSVEncやNVEncの古いバージョンは高ビット入力に不具合があるので、注意してください。

# ライセンス

GPLのライブラリを組み込んでいるので、全体にGPLが適用されています。私の書いたコードはMITライセンスで提供します。
各プロジェクトと利用可能なライセンスは以下の通り。
- Amatsukaze: 一部GPLやその他のライセンスのコードを使っています。各ソースコードファイルにライセンスが書かれているのでそれを見てください。
- AmatsukazeCLI: MITライセンス
- AmatsukazeGUI: MITライセンス
- AmatsukazeUnitTest: MITライセンス
- BatchHashChecker: MITライセンス
- FileCutter: MITライセンス
- libfaad2: GPL

# 同梱&依存ライブラリ
- [FFmpeg](https://www.ffmpeg.org/)
- [FAAD2](http://www.audiocoding.com/faad2.html)
- [L-SMASH](https://github.com/l-smash/l-smash)
- [x264](https://www.videolan.org/developers/x264.html)
- [x265](https://bitbucket.org/Nekopanda/x265)（本家版はzonesを多く指定すると落ちるバグがあるので改造しています）
- [Ut Video Codec Suite](http://umezawa.dyndns.info/wordpress/)
- [AviSynthPlus CUDA](https://github.com/nekopanda/AviSynthPlus)
- [join_logo_scp ver 2.1](https://www.axfc.net/u/3458102.zip)
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
- [KTGMC,KNNEDI3,KFM](https://github.com/nekopanda/AviSynthCUDAFilters)
- [SMDegrain](http://avisynth.nl/index.php/SMDegrain)

すべて64bitです。

# ビルド方法

FFmpeg（ライブラリ）が必要です。Windowsビルドのdev版を落として、
libの中身をプロジェクトのlib/x64(or x86)フォルダへコピーします。

Ut Video Codec Suite（ライブラリ）が必要です。ソースを落として、
ビルドしてください。必要なのはutv_core.libだけです。これを
lib/x64(or x86)へコピーしてください。ビルドにはcygwinが必要なので注意。

AvisynthPlusCUDAが必要です。ソースを落として、ビルドしてください。
ビルドにはCMakeが必要です。AviSynth.libをlib/x64(or x86)へコピーしてください。

単体テストプロジェクト(AmatsukazeUnitTest)は、他にgoogletestのライブラリが必要です。
サブモジュールでgoogletestは追加してあるので、git submodule updateでコードを落として、
googletest/googletest/msvc/gtest-md.slnを開いて、ビルドしてください。
できたgtest.lib/gtestd.libをlib/x64(or x86)へコピーしてください。

単体テストプロジェクト(AmatsukazeUnitTest)は、他にOpenCVも使っているので、OpenCV 3.2.0をビルドしてこれもlibをlib/x64(or x86)へコピーしてください。

# hisdp
A simple high memory ISD/ISM player for Human68k/X680x0

---

## About This

ハイメモリ専用のISD(ISPR-V3.0/4.0)動画プレーヤです。

 - fpsを優先するため逐次読み込み再生ではなくすべてハイメモリに一括バッファ
 - VDISKからハイメモリへのダイレクト高速ロード対応
 - 15kHz/31kHzモード両対応
 - リピート再生対応 
 - PCMドライバとしてPCM8A.X / PCM8PP.X を使用

既存のISD/ISMプレーヤーはディスクからの逐次読み込み再生を前提にしており、大容量ハイメモリへのフルバッファリングに対応しているものは無かったので作成してみました。

---

## 動作環境

* PhantomX 1.03d以上 ライトスルーモード推奨
* 68030モード ハイメモリ768MB推奨
* TS16DRVp.X ハイメモリドライバ
* VDISK必須 (HDS非対応)
* MercuryUnit 推奨

注意：ライトバックモードだと再生が停止してしまい、ハードリセットが必要になる場合があるのでライトスルーでの利用を推奨します。

---

## 対応フォーマット

* ISPR-V3.0 形式 (無圧縮, ADPCM, 拡張子.ISD)
* ISPR-V4.0 形式 (無圧縮, 16bit PCM, 拡張子.ISD/.ISM)

正方形ピクセル形式や圧縮形式には対応していません。また、横幅は128以上256以下で8の倍数のみ対応しています。

---

## 使い方

PCM8A.X または PCM8PP.X をあらかじめ常駐させておきます。

        usage: hisdp [options] <movie-file(.isd|.ism)>
        options:
              -l[n]         ... リピート回数指定。数字省略で無限リピート。(0:endless, 1:default)
              -c<15|31>     ... 15/31kHz mode (31:default)
              -q            ... quiet mode
              -h            ... show help message

注意：-c15で水平15kHzモードにした場合、再生がより滑らかになりますが、MercuryUnitで48kHzのデータを再生した場合にノイズが乗ることがあります。32kHz音声の場合は問題ありません。

---

## 環境変数

環境変数 `HISDP_OPTS` にコマンドラインオプションをあらかじめ設定しておくことが可能です。

例: `SET HISDP_OPTS=-c15`

この場合水平15kHzモードで起動するようになります。

---

## ISD/ISMデータ作成方法

WindowsであればAVIからの変換ツールが公開されています。
Windows以外でも使える拙作 [xmkisd](http://github.com/tantanGH/xmkisd/) などもあります。

---

## 上限FPS

手元の環境での測定です。

* XVI(10MHz) + PhantomX(RP4B) + 68030 WB + ハイメモリ768MB + MercuryV3

216 x 168 x 30.0 FPS + 16bit PCM 32.0kHz stereo がコマ落ち無く再生できます。

* XVI(16MHz) + PhantomX(新ファーム,RB4B) + 68030 WB + ハイメモリ768MB + MercuryV3

240 x 186 x 30.0 FPS + 16bit PCM 32.0kHz stereo がコマ落ち無く再生できます。

---

## History

* 0.1.1 (2023/11/18) ... 縦サイズの上限を240から256にした
* 0.1.0 (2023/11/18) ... 初版

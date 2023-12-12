# hisdp
A simple high memory ISD player for Human68k/X680x0

---

## About This

ハイメモリ専用のISD(ISPR-V3.0/4.0)動画プレーヤです。

 - fpsを優先するため逐次読み込み再生ではなくすべてハイメモリに一括バッファ
 - VDISKからハイメモリへのダイレクト高速ロード対応
 - 15kHz/31kHzモード両対応
 - リピート再生対応
 - 正方形ピクセルモード対応
 - PCMドライバとしてPCM8A.X / PCM8PP.X を使用

既存のISDプレーヤーはディスクからの逐次読み込み再生を前提にしており、大容量ハイメモリへのフルバッファリングに対応しているものは無かったので作成してみました。

---

## 動作環境

* PhantomX version 1.03e 推奨
* 68030 ライトスルーモード ハイメモリ768MB推奨
* TS16DRVp.X ハイメモリドライバ
* VDISK必須 (HDS非対応)
* MercuryUnit 推奨

---

## 対応フォーマット

* .ISD 長方形ピクセル ISPR-V3.0 形式 (無圧縮, ADPCM)
* .ISM 長方形ピクセル ISPR-V4.0 形式 (無圧縮, 16bit PCM)
* .ISS 正方形ピクセル ISPR-V3.0/ISPR-V4.0 形式 (無圧縮, ADPCM または 16bit PCM)

注意：

- 圧縮形式には対応していません。
- 長方形ピクセルの場合、横幅は128以上256以下で4の倍数のみ対応しています。
- 正方形ピクセルの場合、横幅は128以上384以下で4の倍数のみ対応しています。
- 縦幅はいずれも1以上256以下のみ対応しています。

---

## 使い方

PCM8A.X または PCM8PP.X をあらかじめ常駐させておきます。

        usage: hisdp [options] <ispr-movie-file(.isd|.ism|.iss)>
        options:
              -l[n]         ... リピート回数指定。数字省略で無限リピート。(0:endless, 1:default)
              -k            ... 再生前にキー入力を待つ
              -c<15|31>     ... 15/31kHz mode (31:default)
              -q            ... quiet mode
              -h            ... show help message

注意：-c15で水平15kHzモードにした場合、再生がより滑らかになりますが、MercuryUnitで44.1kHz/48kHzのデータを再生した場合にノイズが乗ることがあります。32kHz音声の場合は問題ありません。-c15は長方形ピクセルデータのみ有効です。

再生中にカーソル左を押すと、先頭に戻って改めて再生します。

---

## 環境変数

環境変数 `HISDP_OPTS` にコマンドラインオプションをあらかじめ設定しておくことが可能です。

例: `SET HISDP_OPTS=-c15`

この場合水平15kHzモードで起動するようになります。

---

## ISD/ISM/ISSデータ作成方法

BOOTHにてWindows/Macで使えるクロスISDデータビルダxmkisd, プレーヤHISDP.X, セレクタMCSEL.Xの使いこなし方をまとめた技術書を頒布しています。

* [X680x0 / X68000 Z 最新動画テクニック 2.X680x0実機編](https://booth.pm/ja/items/5306356)

---

## History

* 0.3.2 (2023/12/12) ... 横128ドットの時X方向に2倍拡大展開するISPR仕様が未実装だったのを修正
* 0.3.1 (2023/12/11) ... -kオプションで再生前にキー待ちするようにした。再生中にカーソル左で先頭に戻るようにした。
* 0.3.0 (2023/12/02) ... メモリ確保チェック強化。エラーリカバリ強化。
* 0.2.5 (2023/11/26) ... 縦が奇数ドットの時に画像が乱れていた不具合修正。横4ドット単位のデータに対応
* 0.2.4 (2023/11/26) ... 1ラインスキップ形式のデータ再生に対応した
* 0.2.3 (2023/11/25) ... 正方形ピクセルモードでのVSYNCタイミング不具合修正
* 0.2.2 (2023/11/25) ... 画面プライオリティ設定の不具合修正
* 0.2.1 (2023/11/24) ... 正方形ピクセルモードの対応する幅を増やした
* 0.2.0 (2023/11/24) ... 正方形ピクセルモード(.ISS)に対応した
* 0.1.2 (2023/11/20) ... 60の約数以外のfpsにも対応したつもり 
* 0.1.1 (2023/11/19) ... 縦サイズの上限を240から256にした
* 0.1.0 (2023/11/18) ... 初版

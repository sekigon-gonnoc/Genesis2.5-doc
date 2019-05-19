# Genesis2.5ビルドガイド	
質問は[discord](https://discordapp.com/invite/zXCss8T) または作者Twitter(@_gonnoc)まで

## 購入が必要な部品

|部品|数量|備考|
|---|---|---|
|moduloペンダント|1|同梱版を購入した場合は不要です|
|TRRSケーブル|2|TRSではなくTRRSケーブルが必須です|
|MXスイッチ用PCBソケット|60||
|MX互換スイッチ|60|マウントプレートがないので5ピンスイッチ推奨です|
|MX用キーキャップ(1U)|60|スイッチの取り付け向きが90度回転しているので、ステムの十字が均等な幅のキャップを推奨します|

## 必要なもの
- 時間
- 折れない心

## あると便利なもの
- マスキングテープ
- プレートマウントのスイッチ(10個くらい、組み立て中に取り外しやすいため)

## 行基板の組み立て
大体1時間くらいの工程です
![row](https://user-images.githubusercontent.com/43873124/57151899-08a25f80-6e0d-11e9-98ee-3e33569ddc3a.JPG)
1. IOエキスパンダをはんだ付けする  
 モールドの角が面取りされている側が1番ピン
1. ソケットをはんだ付けする
1. アドレス設定用ランドをジャンパする  
 左手1行目から順にV-D, V-C, V-G, D-V, D-C, D-G...という順にジャンパするのが標準です。  
 わからなくならないようにマジックでメモするとよいです。あとから補強基板を重ねるので雑に書いても隠れます。
 アドレス設定を間違えたとしてもソフト側で対応することはできます。
1. スイッチとキーキャップをとりつける  
  四角いランドがある側が手前です  
  両サイドのスイッチはあとで一度取り外すので、マウントプレートのスイッチを仮止めするのがおすすめです  

## 枠の組み立て
大体15分くらいの工程です
  ピンヘッダを取り付ける際には向きに注意してください
1. 底面用基板にピンヘッダをはんだ付けする
   ![base_row](https://user-images.githubusercontent.com/43873124/57151996-3e474880-6e0d-11e9-8d68-8953591b07fb.JPG)
1. TRRSジャックを側面用基板にはんだ付けする
1. 側面基板と底面用基板をはんだ付けする
	- 仮組してマスキングテープで固定する
	- 一か所ずつはんだ付けする  
	- 一つのピンヘッダについて4か所を一気にはんだ付けするのではなく、1か所ずつはんだ付けしていく  
	![base_assemble](https://user-images.githubusercontent.com/43873124/57152257-e230f400-6e0d-11e9-9121-0b0c8140fe59.JPG)


1. 手前側の底面用基板の余ったピンヘッダを切断する。奥側のピンヘッダはBT Pendant対応用の拡張基板を取り付けられるように残しておいてください

## 行基板の取り付け
色んなパターンを試してみるといつまでたっても終わりません。
自分が一番使いやすい傾斜を探しましょう
1. ピンヘッダの短いほうを側面基板の好きな位置に差し込む
	  ![side_pinheader](https://user-images.githubusercontent.com/43873124/57152299-05f43a00-6e0e-11e9-93da-8a844fd78313.JPG)

1. 行基板を差し込む  
	  -  ピンヘッダと取付穴に遊びがあるため多少角度を調整できるので、キーキャップの干渉が避けられる場合があります。
1. 満足のいく配置を探す  

## 行基板のはんだ付け
1. 側面側のピンヘッダを1か所ずつはんだ付けする
  ![side_view](https://user-images.githubusercontent.com/43873124/57152407-6be0c180-6e0e-11e9-9ec1-09e53e0178bd.JPG)

1. 両サイドのスイッチを取り外し、行基板側のピンヘッダを1か所ずつはんだ付けする  
 面倒くさがってスイッチを取り外さずにはんだ付けするとキーキャップやスイッチを溶かしてしまう恐れがあります
  ![top_view](https://user-images.githubusercontent.com/43873124/57152468-8c108080-6e0e-11e9-8534-d335f8a29432.JPG)
  
1. 干渉が発生しないことを確認出来たら、残りの部分もはんだ付けする
1. 補強用行基板を各行に重ね、ネジを通してナットで止める
1. 両サイドのスイッチを取り付ける

## moduloペンダントの用意
現時点ではBiacco42さんの[modulo blackpill pendant](https://github.com/Biacco42/modulo-ergo42#modulo-black-pill-pendant-beta)に対応しています。
Blackpill Pendantを使う場合、事前にブートローダを書き込む必要があります。書き込む方法としてはstlinkやcmsis-dapなどの書き込み機を使う方法、FT232などのUSB-UART変換器を使う方法があります。

## ファームウェア書き込み

1. リポジトリを取得する
  nrf52対応のqmk_firmwareは[こちら](https://github.com/sekigon-gonnoc/qmk_firmware/tree/nrf52)

	- 既にqmk_firmwareを使っている場合
	```
        git remote add sekigon https://github.com/sekigon-gonnoc/qmk_firmware.git
        git pull sekigon nrf52
        git checkout nrf52
	```
	
	 	　　　または
	 
		git clone  -b nrf52 https://github.com/sekigon-gonnoc/qmk_firmware.git ble_micro_pro
	  
	- 初めて使う場合

        git clone --depth 1 -b nrf52 https://github.com/sekigon-gonnoc/qmk_firmware.git

1. 必要サブモジュールを用意

		make git-submodule


### BlackPillペンダントへのファームウェア書き込み

1. ファームウェアをビルドする

    ```
    make genesis2_5/rev2:default
    ```

1. dfu-utilを使って書きこむ

	  リセットスイッチを押して青色LEDの点滅がゆっくりになったら以下のコマンドを実行

    ```
    dfu-util -d 1eaf:0003 -a 2 -D <firmware.bin>
    ```

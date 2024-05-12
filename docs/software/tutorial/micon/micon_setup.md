# ArduinoIDEの環境構築
マイコンにプログラムを書き込むソフトウェアの１つでもあるArduinoIDEのインストールについて説明します。


## ソフトのダウンロード
[こちらのリンク](https://www.arduino.cc/en/software)から「Arduino（アルディーノ）IDE」ダウンロードしてください。

Windowsの場合は下の写真の「Windows win10 and newer 64bits」クリックします。

![image](./img/arduino_download.png)

## ESP32を開発するために...
先程ダウンロードしたソフトは通常は名前の通りArduinoと呼ばれるマイコンにしかプログラムの書き込みをすることができません。
今回はダウンロードに加え***ESP32に書き込むため***のことをしなければなりません。
ArduinoIDEを起動（他のソフトと同じようにダブルクリックで開くはず）してください。
そして画面左上にある**File**をクリックしてください。

![image](./img/tool_bar.png)

すると上の写真のようなツールバーが出るはずなので下から３つ目のプリファレンスを選択してください。

そして下のリンクを「Additional boards manager URLs」にコピーしてください。

```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

![image](./img/pref.png)


ターミナルからコマンドプロンプトを開いて以下のコマンドを入力してください。
```sh
python --version
```

この実行結果として3.1.1みたいなバージョンを得られた場合は**ボードマネージャ**のダウンロードに行ってください。

なにも出なかった人は引き続きコマンドプロンプトで
```
python
```
と入力してください。するとpythonをインストールできそうな画面が出るはずなのでそのままインストールしてください。
インストールが終わったら
```
pip install pyserial
```
と入力したら次に進んでください。


## ボードマネージャーのダウンロード
最後にボードマネージャーをダウンロードします。画面左側のバーのうち、上から２番目を選択するとボードマネージャーが出るはずです。
検索欄に「esp」といれると、**esp32 by Espressif Systems**があるはずなのでインストールしてください。写真だと１番下ですね。

![image](./img/boards_manager.png)

## 終わりに
これで環境構築が終了したので具体的にマイコンがどういうものなのかを勉強していきます。

[マイコンに触れる](./micon_touch.md)
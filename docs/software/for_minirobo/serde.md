# 文字列送るのめんどくせーよの会

## はじめに
PCとマイコン間で通信ができるようになりましたが、実際送っていたのは「Hello,  World!!」みたいな文章です。え？ロボットで使えるわけがなくない？

ということで送信するデータに工夫する会です。

## Json形式
今回はプログラムで用意した構造体をJson形式に変換するという手法をとります。json形式とは**JavaScript Object Notation**という**JavaScript**という言語におけるオブジェクトの定義の仕方を参考にした定義です。なんとRustにはこれらの変換を簡単に行えるクレート（ライブラリみたいなもん）があるので活用していきましょう。

## SerdeとSerde_json
おそらく「さーでぃ」って読むと思います。Serdeとserde_json、これらのクレートを用いて実装していきます。クレートのリンクは以下になります。

[serde - Rust](https://docs.rs/serde/latest/serde/)

[serde_json 0.8.6](https://docs.rs/crate/serde_json/0.8.6)

## Cargo.tomlの設定
```toml
[dependencies]
serde = {version = "1.0.195", features = ["derive"]}
serde_json = "1.0"
```

詳しい書き方はわからないです。コピペしてそういうものなんだなって思ってます。


## 実装
### 構造体の作成
まずメッセージとなる構造体を定義します。今回は**Message**という名前の構造体で人の名前、年齢、好きな数字を入れる構造体にしました。
```rs
use serde::{Serialize, Deserialize};
use serde_json;

#[derive(Serialize, Deserialize)]
pub struct Message
{
    pub name:String,
    pub age:i32,
    pub favorite_number:f32
}
```

**use**で外部クレートを持ってくるのはいつも通りですね。
構造体の上になにやら**#[derive(Serialize, Deserialize)]**というものが書いてます。今回はここが一番のミソです。
**derive**というものは「派生」という意味を指す英語で、**serde**に実装された**Serialize**（構造体を文字列に変換）する機能と**Deserialize**（文字列を構造体に変換）する機能を好きな構造体に派生させてます。要するにこれをつけることで**Message**構造体を変換できるようになったんですね。

### 構造体をメッセージに変換する
それでは使っていきましょう。
```rs
use serde::{Serialize, Deserialize};
use serde_json;

#[derive(Serialize, Deserialize)]
pub struct Message
{
    pub name:String,
    pub age:i32,
    pub favorite_number:f32
}

fn main()
{
    let msg = Message{
        name:"Motii".to_string(),
        age:18
        favorite_number:123.456
    };

    let serialized_msg = serde_json::to_string(&msg).unwrap();

    println!("Serialized :{}", serialized_msg);
}
```

これを実行すると以下のように出ます。
```
Serialized:{"name":"Motii","age":18,"favorite_number":123.456}
```

**Rust**の**println!()**関数はその文字列を出力したあと、自動で改行も出力してくれるすぐれものです。
**{}**はC言語でいう**%d**みたいなもので、変数の中身を出力します。

まず構造体の中身を設定し、**serde_json**の関数を用いて変換を行いました。
ここで最初の**#[derive]**の文を書いてないとエラーがでてしまうので注意してください。

### メッセージを構造体に変換する。
以下のような感じです。
```rs
use serde::{Serialize, Deserialize};
use serde_json;

#[derive(Serialize, Deserialize)]
pub struct Message
{
    pub name:String,
    pub age:i32,
    pub favorite_number:f32
}

fn main()
{
    let msg = Message{
        name:"Motii".to_string(),
        age:18
        favorite_number:123.456
    };

    let serialized_msg = serde_json::to_string(&msg).unwrap();

    println!("Serialized :{}", serialized_msg);

    let deserialized_msg:Message = serde_json::from_str(&serialized_msg).unwrap();

    println!("Deserialized name:{}, age:{}, fav_num:{}", deserialized_msg.name, deserialized_msg.age, deserialized_msg.favorite_number);
}
```

実行するとこんな感じ
```
Serialized:{"name":"Motii","age":18,"favorite_number":123.456}
Deserialized name:Motii, age:18, fav_num:123.456
```

## ESP32でも同じことをする
### ライブラリのインストール
コードを書くために[**ArduinoJson**](https://arduinojson.org/)というライブラリをライブラリマネージャーよりインストールしておいてください。

そしてインクルードします。
```cpp
#include <ArduinoJson.h>
```
そしてJson形式を扱うときにはArduinoでは以下の変数を用意してください。また、Rustと同じような構造体を作成します。

```cpp
#include <ArduinoJson.h>

struct Message
{
    String name;
    int age;
    float favorite_number;
}

JsonDocument doc;

const char *msg = "{"name":"Motii","age":0,"favorite_number":66.66}";

void setup()
{

}

void loop()
{
    DeserializationError err = deserializeJson(doc, msg);

    String name = doc["name"];
    int age = doc["age"];
    float fav_num = doc["favorite_number"];

    Message msg;
    msg.name = name;
    msg.age = age;
    msg.favorite_number = fav_num;
}
```

今回は詳しい使い方は書きませんでしたが、これで受信したメッセージを構造体にすることができました。
通信するときに有効活用してください。

## おわりに
ここまででミニロボへ向けた一通りの資料が終了しました。わからないことがあったら執筆者である[Motii](https://github.com/motii8128)へお願いします。
また、調べれば同じようなことをしている資料が出てくるかもしれないので、いろんな情報を取捨選択して頑張りましょう。
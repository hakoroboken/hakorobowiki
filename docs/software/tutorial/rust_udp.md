# Rustで始めるUDP通信

## はじめに
UDP通信の細かい話については前節で説明したため省略させていただきます。

詳しくは以下をご覧ください。

[２．マイコンでUDP通信を始める](./udp_esp32.md)

## ライブラリを持ってくる
C言語だとincludeの部分ですね

```rs
use std::net::UdpSocket;
```

## 通信の初期化
```rs
fn main()
{
    let sock = UdpSocket::bind("192.168.4.2:8080").unwrap();
}
```

**UdpSocket**という構造体を**use**で持ってきたので、**UdpSocket**にインプリントされた**bind**関数を用いて通信の初期化を行っている。
文字列で**IPアドレス：ポート**というように記述する。今回はIPアドレスは**192.168.4.2**でポートは**8080**にした（テキトー）

## メッセージの送信〜match文を添えて〜
```rs
loop
{
    let msg = "Hello, Rust UDP";
    match sock.send_to(msg.as_bytes(), "192.168.4.1:10000") {
        Ok(size)=>{
            print!("Send size is {}\n", size);
        }
        Err(err)=>{
            print!("{:?}\n", err);
        }
    }
}
```

ここでは**match**文が全く違う使い方をされている。
**match**で比較する文を**Result**型にした場合、成功したとき（つまり**Ok**）と失敗した場合（つまり**Err**）で書き分けることができるのだ。

そして**UdpSocket**構造体内にある**send_to**関数を使っている。これはその名の通り、２つ目の引数の相手に１つ目の引数の情報を送信するというものだ。
２つ目の引数については最初と同じように相手のアドレスとポートを指定しただけである。しかし、１つ目の引数については最初にstr（文字列）として宣言された**msg**をbyte列（要は送りやすい小さいデータ）に変換して送っているんですね。

そんな**send_to**関数の返り値は**Result<**usize, Error**>**なので成功した場合の変数をOk()のなかに書くと送信に成功した文字列のサイズが入る。失敗した場合の方の変数にはエラー文が入る。

## メッセージの受信
```rs
let mut buf = [0_u8;256];

match sock.recv(&mut buf)
{
    Ok(size)=>{
        let get_msg = &buf[0..size];

        let get_msg_str = String::from_utf8_lossy(get_msg).to_string();

        print!("{}\n", get_msg_str);
    }
    Err(err)=>{
        print!("{}\n", err);
    }
}
```

先程と同じようなmatchによる条件分岐です。
詳しい受信についての説明は[２．マイコンでUDP通信を始める](./udp_esp32.md)の**メッセージを受信する**というところをご覧ください。

**Ok**だった場合に受信に成功したサイズを返すので、バッファのうちの受信に成功したサイズのみを**get_msg**という変数に突っ込んでいます。
その後、String(文字列)ライブラリに含まれる**from_utf8_lossy**という関数をもちいて、u8のバイト列から文字列に変換しています。というのも、受信する直前に**buf**という変数をu8(整数型8bit)の配列、サイズは256で宣言しているので、u8のバイト列からの変換を試みているわけです。**from_utf8_lossy**のあとにはちゃんと文字列にするための**to_string**をわすれずにしてください。

## おわりに
これで両者の情報交換については申し分ないでしょう。この先はその他で必要なことについてです。

[５．ゲームコントローラーを扱う](./game_con.md)
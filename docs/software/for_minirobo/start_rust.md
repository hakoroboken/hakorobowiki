# Rustで始めるPC側の準備
ここからはPCさえあればできます。

## はじめに
先ほどまではマイコン開発のためにC言語を利用してきました。函館高専のロボット研究会では**Rust**（ラスト）という言語の使用を推しています。Windowsでも利用することができ、とても安全なプログラムを書くことのできる言語です。ここではRustを使用したUDP通信について解説します。

## 環境構築
以下を参照してください。

[環境構築ガイド一覧](../setup_env/index.md)

## Rustという言語
調べると色々なRustの利点が出てくるはずです。ここではRustを書く上で必要な基本事項を述べます。

### パッケージの作成からHello World
基本的にこれからプログラムを作るときにはパッケージを作るものだと認識してほしい。授業のようにただ「.c」で終わるファイルを一枚書けば良いとは行かないんです。Rustでの開発では**Cargo**というビルドシステムを採用しています。
Cargoによるパッケージは以下のように作成します。

```
<!-- cargo new <パッケージ名>という感じ -->
cargo new test_pkg
```

これでtest_pkgというパッケージが作成できた。ここでいうパッケージはフォルダのことを指し、test_pkgというフォルダができていて、その中に実際のコードを入れる**src**というフォルダと、パッケージの概要を示す**Cargo.toml**が入っているはずです。

```
cd test_pkg
cargo run
```

上記のコマンドでまず、test_pkgフォルダ内に移動して**cargo run**でコードのコンパイル、実行をしています。
上手くいっていれば**Hello, World!!**がみられるはず。

### 関数
もちろんC言語のように関数が存在します。
C言語と比較すると以下のような感じです。
#### C言語
```c
void hello()
{
    printf("Hello, C");
}

int a_plus_b(int a, int b)
{
    return a + b;
}
```

#### Rust
```rs
fn hello()
{
    print!("Hello, Rust");
}

fn a_plus_b(a:i32, b:i32)->i32
{
    // returnを書くなら
    // return a + b;
    a + b
}
```

### 変数
Rustでは変数を宣言するときに型を入れる必要がありません。その代わり、変数の前に**let**というのをつけて宣言します。
すると、変数はその値に束縛されて変更できなくなります。

```rs
let x = 5; 
let y = 2.8; 
let address = "192.168.11.50";
```

型をもし書くなら以下のような感じ
```rs
let x:i32 = 5;
let y:f32 = 2.8;
let address:&str = "192.168.11.50";
```

しかし、後から変数の中身を書き換えたりするときだってありますよね。そんなときはミュータブル（可変的）の意味を持つ**mut**をつけます。

```rs
let mut x = 0;

x = 33.4;

print!("x = {}", x);
// 実行結果
// x = 33.4
```

しれっとprint!の使い方も入ってしまいました。
C言語なら、**%d**とか**%lf**とか書くところを**{ }**に置き換えて書くみたいです。


### if
```rs
let x = 40;

if x > 40
{
    print!("metya big");
}
else if(x > 0)
{
    print!("ma, sokosoko big");
}
else
{
    print!("majide, tisai");
}
```

### for
```rs
for i in 0..5;
{
    let x = 2 * i;
    print!("No.{}: x = {}\n", i, x);
}

// 実行結果
// No.0:x = 0
// No.1:x = 2
// No.2:x = 4
// No.3:x = 6
// No.4:x = 8
```

### loop
```rs
let mut x = 0;
loop
{
    if x == 100
    {
        break;
    }
    print!("x = {}\n", x);
    x += 1;
}

// 実行結果
// x = 0
// x = 1
//////////////////
// x = 98
// x = 99
```

### match
```rs
let value = 567;

match value
{
    566=>{print!("dame");},
    567=>{print!("OK");},
    568=>{print!("dame");},
    _=>{print!("3tu no doredemo naiyo");}
}

// 実行結果
// OK
// valueの中身が40とかだと、
// 3tu no doredemo naiyo
```

### struct（構造体）とimpl（インプリント）
```rs
struct Add
{
    pub x_value:i32,
    pub y_value:i32
}

impl Add {
    pub fn new(a:i32, b:i32)->Add
    {
        let result = Add{
            x_value:a, y_value:b
        };

        result
    }

    pub fn run(self)
    {
        print!("Result is {}\n", self.x_value+self.y_value);
    }
}

fn main()
{
    let test = Add::new(4, 5);

    print!("x_value:{}, y_value:{}\n", test.x_value, test.y_value);

    test.run();   
}

// 実行結果
// x_value:4, y_value:5
// Result is 9
```
構造体とは複数の変数をまとめたものである。また、Rustではインプリントと言って、構造体に関数を付与することができる。
また、構造体内の変数の前に**pub**とついているのは後に外部からその値を触ってもよいということを指す。
なので、**main**関数で**test.x_value**のようにさわることができている。

**main**関数の冒頭で**Add**にimpl（インプリント）したnew関数を使っている。これは引数を構造体内の変数に入れて返すという関数で上記のように使うことができる。ここで、testという変数の型は**Add**になる。


### use

stdライブラリ内のf32(float32)というライブラリ内のconsts（定数ｓ）内にある定数であるPIを使う宣言をする。
```rs
use std::f32::consts::PI;


fn main()
{
    let pi = PI;

    print!("pi = {}\n", pi);
    print!("pi^2 = {}\n", pi.powi(2));
}

// 実行結果
// pi = 3.1415927
// pi^2 = 9.869605
```
また、f32にはpowiという関数がインプリントされているため、ドットで区切って呼び出すことができる。（sqrtとかもある）
ここで気づいたかもしれないが、構造体内の関数だったり、ライブラリ内の構造体だったり、より中身に入っていくときは「::（コロン）」が使われる。

### Result

RustではResultという型が頻出する。これはその名の通りなにかの実行結果を指す型で以下のように使う
```rs
// use std::io::Error;
// use std::io::ErrorKind;
use std::io::{Error, ErrorKind};

fn main() {
    let number = -3;

    let _ = check(number).unwrap();

    let num2 = 4;

    let _ = check(num2).unwrap();
}

fn check(number: i32)->Result<(), Error>
{
    match number > 0
    {
        true=>{
            print!("tadasii\n");
            Ok(())
        },
        false=>{
            print!("DAME\n");
            Err(Error::new(ErrorKind::InvalidData, "負の数の出席番号は存在しない"))
        }
    }
}

// 実行結果
// DAME
// thread 'main' panicked at src/main.rs:6:27:
// called `Result::unwrap()` on an `Err` value: Custom { kind: InvalidData, error: "負の数の出席番号は存在しない" }
// note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

またResult型にはunwrapという関数が備わっていて、これはResultを結果とする関数についてOKであることを前提に後のコードを進めるよという関数です。
しかしエラーが発生すると、以下のように途中でプログラムが終わってしまいます。通信OKじゃないのに送受信するところに行ったりしたら困りますからね。

## おわりに
次からはRustで始めるUDP通信です。

[４．Rustで始めるUDP通信](./rust_udp.md)
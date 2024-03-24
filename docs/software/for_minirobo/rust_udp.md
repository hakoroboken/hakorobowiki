# UDP通信入門（PC側）
ここからはPCさえあればできます。

## はじめに
先ほどまではマイコン開発のためにC言語を利用してきました。函館高専のロボット研究会では**Rust**（ラスト）という言語の使用を推しています。Windowsでも利用することができ、とても安全なプログラムを書くことのできる言語です。ここではRustを使用したUDP通信について解説します。

## 環境構築
以下を参照してください。

[環境構築ガイド一覧](../setup_env/index.md)

## Rustという言語
調べると色々なRustの利点が出てくるはずです。ここではRustを書く上で必要な基本事項を述べます。

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


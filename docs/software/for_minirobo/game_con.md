# DualShock4を扱う by Rust

## はじめに
ここではRustでDualshock4を扱うことができるようになります。

![image](./img/dualshock4.jpg)

## プログラムは楽してなんぼ
ソフトウェアを始めたてのころはもちろんわからないことのほうが多いです。これを書いてる僕も自分一人でパッケージの中身を実装できるようになったのはつい最近で、それまでは他人のコードをコピーしたり先輩に聞いたりしてました笑。

今回は僕が用意したDualshock4を扱うコードを引用してコントローラー部分を実装していきましょう。

## Cargo.tomlの設定
ここでCargoというパッケージ管理システムを使ってきた良さが出てきます。これから**Rustで**他人の作ったライブラリなどを扱うときにはCargo.tomlに**依存関係**（使ってるライブラリとの関係みたいなこと）を記述します。
まずパッケージを作成しましょう。

```bash
cargo new ds_test
```
次にcdコマンドで作ったファイル内に移動してください
```bash
cd ds_test
```

そしてCargo.tomlに手を加えます。以下は初期状態です。
```toml
[package]
name = "ds_test"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]

```
今回手をつけるのは下の**dependencies**の部分です。ここに以下のような形式で使うライブラリを指定します。また、ここでの指定方法はライブラリが**Github**にあるものを前提としているので注意してください。
# ESP32でPWMを出す方法
ESP32でPWMを出す方法が少し特殊なので、ここで別の解説を入れさせていただきます。

```cpp
void setup()
{
    ledcSetup(0, 12800, 8);

    ledcAttachPin(A12, 0);
}

void loop()
{
    // 0 ~ 256
    int power = 128;

    ledcWrite(0, power);
    delay(500);
    ledcWrite(0, 0);
    delay(500);
}
```

setup関数の操作が少し違いますね。ESP32でアナログ出力を扱う際には、チャンネルというのを作成します。
そこでsetup関数内のはじめで、ledcSetupという関数により0番というチャンネルをPWM周波数12800,8ビットで作成しています。 
8ビットなので256段階で強さを調整することができます。
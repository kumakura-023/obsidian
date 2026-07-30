---
created: 2026-07-24T17:37
updated: 2026-07-28T09:45
tags:
  - "#USB"
  - "#interface"
  - "#CDC"
context:
---
## 通信方式

Control transfer は、USB で使われる **制御用の通信方式**です。
USB には用途別にいくつか転送タイプがあります。

```
Control transfer   : 設定・問い合わせ・制御用
Bulk transfer      : 大きめのデータ転送用
Interrupt transfer : 小さい通知・状態通知用
Isochronous        : 音声/映像など周期データ用
```

今回の CDC 仮想 COM でいうと、実データの送受信は **bulk transfer** です。

```
Tera Term の文字データ
  -> USB CDC bulk OUT / bulk IN
```

一方で、COM ポートを開いたときの設定情報、たとえば:

```
ボーレート
データビット
パリティ
ストップビット
DTR/RTS
```

こういう「通信条件の設定」は **control transfer** で送られます。
USB control transfer は基本的に **Endpoint 0、つまり EP0** を使います。


## CDC設定から通信可能になるまで

```
1. CDC interface を含む USB descriptor を登録
2. USB 接続開始
3. Host が enumeration し、CDC ACM デバイスとして認識
4. Host から SET_CONFIGURATION
5. CyCx3AppConfigureEndpoints() で CDC endpoint を実体設定
6. CyCx3UartBulkBridgeInit() で UART 初期化 + DMA channel 作成
7. Host が COM ポート open / 設定変更
8. CDC class request を CyCx3AppUSBSetupCB() 経由で処理
9. SET_LINE_CODING なら EP0 から line coding を読み、CyU3PUartSetConfig() で UART 設定へ反映
10. Bulk OUT/IN と UART の間でデータ転送可能
```

**{{date}}**（ここにメモ内容を記述）

---

*関連リンク・コンテキスト:* [[（関連ノートへのリンク）]]
---
created: 2026-07-24T17:37
updated: 2026-07-24T17:50
tags:
  - "#C言語"
context:
---

**{{date}}**（ここにメモ内容を記述）

#バグ #デバイスマネージャ #Descriptor
## I2Cで読み取った値をDescriptorに反映する機能の追加でバグ

### I2Cでリードしているのに、デバイスマネージャでデバイス名に反映されない。
→IProductだけを動的に更新していたが、iSerialNumberが静的に設定されていた。
　windowsは同じPID/VID/serialを同一デバイスinstanceとして扱うため、表示名がそのままになってしまった。
　iSerial Numberも動的に更新することで解消。
---

*関連リンク・コンテキスト:* [[（関連ノートへのリンク）]]
---
created: 2025-12-12T09:58
updated: 2025-12-12T09:58
---
# Stackとは

- **追加（Push）**と**削除（Pop）**は常に「トップ（一番上）」からしか行えない。
- **Peek()**でトップの要素を覗くことができる（取り出さずに確認）。
- 操作の時間計算量：Push/Pop/PeekはすべてO(1)（超高速）。
- C#のStack<T>は内部で配列を使って実装されている（List<T>と同じく動的配列）。

## 使い方の例

ここからは普通にMarkdownが効くはずです。

- リストも
- ちゃんと
- 表示される

**太字**も効くし、コードブロックも：

```csharp
var stack = new Stack<int>();
stack.Push(1);
stack.Push(2);
Console.WriteLine(stack.Pop()); // 2
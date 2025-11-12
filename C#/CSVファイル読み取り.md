---
created: 2025-11-12T11:10
updated: 2025-11-12T11:11
---
## ⚙️ クラス構造
```C#
public sealed class InfraredWordLabelProvider : IInfraredWordLabelProvider
{
    // ---- 定数群 ----
    public const int FirstInfraredWordIndex = Frame73.SyncWordCount + Frame73.GimbalWordCount;
    public const int InfraredWordCount = Frame73.InfraredWordCount;

    private readonly string[] _labels = new string[InfraredWordCount];
    private readonly IReadOnlyList<string> _readOnlyLabels;
}
```

- `FirstInfraredWordIndex`  
    = フレーム全体の中で赤外ワードが始まるインデックス。つまり22番（Sync=2語, Gimbal=20語の後）。
    
- `InfraredWordCount`  
    = 赤外ワード数（50語）。
    
- `_labels`  
    = CSVから読み取った生の文字列配列。
    
- `_readOnlyLabels`  
    = UIが参照するための読み取り専用ビュー。

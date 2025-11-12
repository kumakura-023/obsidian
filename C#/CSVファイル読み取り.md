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

## 🪶 CSVロードの流れ

### 1. コンストラクタで初期化

```C#
public InfraredWordLabelProvider(string? csvPath = null) {

 _readOnlyLabels = Array.AsReadOnly(_labels);
 Array.Fill(_labels, string.Empty); 
 
 string? resolvedPath = csvPath ?? ResolveDefaultPath();
 ... 
  }`
```

まず `_labels` を空文字で埋める。  
その後 `csvPath` が渡されなければ、`ResolveDefaultPath()` で自動探索する。

### 2. パス探索 (`ResolveDefaultPath`)

```C#
const string relativeFolder = "docs";
const string fileName = "赤外コマンド項目.csv";
string baseDirectory = AppContext.BaseDirectory;
```

- 実行ファイルのディレクトリから始めて、  
    `docs/赤外コマンド項目.csv` が存在するかを6階層上まで探索する。
    
- どこにもなければ `null` を返す。
    

つまり、**アプリがどのフォルダに展開されても、相対的にCSVを探せるようにしている**。
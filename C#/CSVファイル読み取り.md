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
 private static string? ResolveDefaultPath()
 {
     const string relativeFolder = "docs";
     const string fileName = "赤外コマンド項目.csv";
     string baseDirectory = AppContext.BaseDirectory;

     string candidate = Path.Combine(baseDirectory, relativeFolder, fileName);
     if (File.Exists(candidate))
     {
         return candidate;
     }

     DirectoryInfo? directory = new DirectoryInfo(baseDirectory);
     //6階層上まで探索
     for (int i = 0; i < 6 && directory is not null; i++)
     {
         candidate = Path.Combine(directory.FullName, relativeFolder, fileName);
         if (File.Exists(candidate))
         {
             return candidate;
         }

         directory = directory.Parent;
     }

     return null;
 }
```

- 実行ファイルのディレクトリから始めて、  
    `docs/赤外コマンド項目.csv` が存在するかを6階層上まで探索する。
    
- どこにもなければ `null` を返す。
    

つまり、**アプリがどのフォルダに展開されても、相対的にCSVを探せるようにしている**。



### 3. ファイル読み込み

```C#
string[] rawLines = ReadAllLinesWithEncoding(resolvedPath);
int copyCount = Math.Min(rawLines.Length, InfraredWordCount);
for (int i = 0; i < copyCount; i++)
{
	//NormalizeLabel で空行や空白を除去。
    _labels[i] = NormalizeLabel(rawLines[i]);
}

```

#### (a) `ReadAllLinesWithEncoding`

```C#
private static string[] ReadAllLinesWithEncoding(string path)
{
    byte[] bytes = File.ReadAllBytes(path);
    string text;
	//BOM(Byte Order MarkがあればUTF-8)
    if (HasUtf8Bom(bytes))
    {
        text = Encoding.UTF8.GetString(bytes);
    }
    else
    {
        try
        {
            text = Utf8Strict.GetString(bytes);
        }
        catch (DecoderFallbackException)
        {
	        //UTF8じゃなければShiftJisで読む
            text = ShiftJis.GetString(bytes);
        }
    }

    if (text.Length > 0 && text[0] == '\uFEFF')
    {
        text = text[1..];
    }
	//改行コードをすべて\nに統一
    text = text.Replace("\r\n", "\n").Replace('\r', '\n');

    var lines = new List<string>();
    using var reader = new StringReader(text);
    string? line;
    while ((line = reader.ReadLine()) is not null)
    {
        lines.Add(line);
    }

    return lines.ToArray();
}
```

ここがちょっとした技術ポイント。

- ファイル全体をバイトで読む。
    
- もしBOM（Byte Order Mark）があればUTF-8と判断。
    
- それがなければ、まずUTF-8でデコードを試す。
    
    - 失敗したらShift_JISで再トライ。
        

結果的に、**UTF-8 or Shift_JIS どちらのCSVでも正しく読める**ようにしてある。

#### (b) 行単位の正規化

- 改行コードをすべて `\n` に統一。
    
- `NormalizeLabel` で空行や空白を除去。
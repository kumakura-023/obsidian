---
created: 2025-12-12T09:58
updated: 2025-12-12T09:58
---
# Stackとは

- **追加（Push）**と**削除（Pop）**は常に「トップ（一番上）」からしか行えない。
- **Peek()**でトップの要素を覗くことができる（取り出さずに確認）。
- 操作の時間計算量：Push/Pop/PeekはすべてO(1)（超高速）。
- 操作の時間計算量：Push/Pop/PeekはすべてO(1)（超高速）。


## 使い方の例

```C#
using System;
using System.Collections.Generic;

public class MatchingBrackets
{
    public static bool IsPaired(string input)
    {
        var stack = new Stack<char>();
        var opening = "([{";
        var closing = ")]}";
        var matching = new Dictionary<char, char>
        {
            { ')', '(' },
            { ']', '[' },
            { '}', '{' }
        };

        foreach (char c in input)
        {
            if (opening.Contains(c))
            {
                stack.Push(c);                  // 開き括弧はスタックに積む
            }
            else if (closing.Contains(c))
            {
                if (stack.Count == 0)           // 閉じ括弧が来ているのにスタックが空
                    return false;

                char lastOpen = stack.Pop();
                if (lastOpen != matching[c])    // 対応する開き括弧と一致しない
                    return false;
            }
            // それ以外の文字（文字、数字、記号など）は無視
        }

        return stack.Count == 0;                // 最後まで開き括弧が残っていなければtrue
    }

    // テスト用
    public static void Main()
    {
        Console.WriteLine(IsPaired("{what is (42)}?")); // True
        Console.WriteLine(IsPaired("[text}"));          // False
        Console.WriteLine(IsPaired("[({]})"));          // False ← これがAssert.Falseになるケース
        Console.WriteLine(IsPaired("[({})]"));          // True
        Console.WriteLine(IsPaired("[(])"));            // False
        Console.WriteLine(IsPaired("{[()]()}"));        // True
    }
}
```

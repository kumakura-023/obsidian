---
created: 2025-12-04T10:50
updated: 2025-12-04T11:11
---
# HashCodeとは

~~インスタンスを識別するためのコード。~~

HashCode は “インスタンスを直接識別するもの” ではなく  
**“ハッシュテーブルで効率よく探すための分類用コード” 。**

GetHashCode()は、同一のインスタンスには同一のコードを返す。
ただし、***コードは一意じゃない***。 
つまり、　Aというインスタンスに対して12345が割り当てられている状態で、Bというインスタンスに対して12345というハッシュコードを返すこともある。
# Equals

そこで、Equals()を使って本当に同一であるかを確認する。

# ReferenceEquals

インスタンスが完全に同一化を判別する。
→ メモリ上の同じオブジェクトを指しているか

# 使い方

Identityクラス
```C#
public class Identity
{
    public string Email { get; }
    public FacialFeatures FacialFeatures { get; }

    public Identity(string email, FacialFeatures facialFeatures)
    {
        Email = email;
        FacialFeatures = facialFeatures;
    }

    public override bool Equals(object? obj)
    {
        if (obj is not Identity other)
            return false;

        return Email == other.Email
            && FacialFeatures.Equals(other.FacialFeatures);
    }

    public override int GetHashCode()
    {
        return HashCode.Combine(Email, FacialFeatures);
    }
}
```

認証クラス
```C#
public class Authenticator
{
    private static readonly Identity AdminIdentity =
        new Identity("admin@exerc.ism", new FacialFeatures("green", 0.9m));

    // 登録された Identity の集合
    private readonly HashSet<Identity> _registered = new();

    public bool IsAdmin(Identity identity)
        => identity.Equals(AdminIdentity);

    public bool Register(Identity identity)
        => _registered.Add(identity); // すでにあれば false, なければ true

    public bool IsRegistered(Identity identity)
        => _registered.Contains(identity);
}
```

ポイント：
 `HashSet<T>.Add` は
- その値がまだ無ければ追加して `true`
- すでに同じものがあれば追加せず `false` を返してくれる
 `Contains` も同じく `Equals` / `GetHashCode` を使って判定してくれる

だから、**自分で「ハッシュのリスト」とか「あの3つの値のセット」を管理する必要はない**。
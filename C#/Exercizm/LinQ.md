---
created: 2025-12-10T17:47
updated: 2025-12-10T17:47
---
```C#
using System;
using System.Linq;
using System.Collections.Generic;

public class GradeSchool
{
    private Dictionary<string, int> _roster = new();
    
    public bool Add(string student, int grade) 
    => _roster.TryAdd(student, grade);

    public IEnumerable<string> Roster() =>
        _roster.OrderBy(s => s.Value).ThenBy(s => s.Key).Select(s => s.Key);

    public IEnumerable<string> Grade(int grade) =>
        _roster.Where(s => s.Value == grade).
        OrderBy(s => s.Key).Select(s => s.Key);
}
```

```C#
using System;
using System.Collections.Generic;
using System.Linq;

public class GradeSchool
{
    // 学年ごとに生徒の集合を持つ
    private readonly SortedDictionary<int, SortedSet<string>> _grades
        = new SortedDictionary<int, SortedSet<string>>();

    public bool Add(string student, int grade)
    {
        // すでにどこかの学年にいるかチェック
        if (_grades.Values.Any(set => set.Contains(student)))
        {
            // どこかに同じ名前がいたら追加は失敗（false）
            return false;
        }

        // まだこの学年のエントリがなければ作る
        if (!_grades.TryGetValue(grade, out var students))
        {
            students = new SortedSet<string>();
            _grades[grade] = students;
        }

        // SortedSet.Add は、追加できたら true / すでにあれば false
        return students.Add(student);
    }

    // 全学年のロスター（学年昇順＋名前アルファベット順）
    public IEnumerable<string> Roster()
    {
        // SortedDictionary なのでキー（学年）はすでに昇順
        // 各学年の SortedSet も名前順なので、
        // それをそのまま SelectMany すれば条件を満たす
        return _grades
            .SelectMany(kv => kv.Value)
            .ToList(); // 外から変更されないように List にコピーして返す
    }

    // 特定学年の生徒一覧（名前アルファベット順）
    public IEnumerable<string> Grade(int grade)
    {
        if (_grades.TryGetValue(grade, out var students))
        {
            return students.ToList(); // これもコピーして返す
        }

        // その学年がまだ存在しない場合は空
        return Enumerable.Empty<string>();
    }
}

```
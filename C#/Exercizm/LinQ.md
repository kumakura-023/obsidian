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


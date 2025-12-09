---
created: 2025-11-28T15:26
updated: 2025-12-09T10:20
tags:
---
## 日付をテキストからDateTimeへ
### フォーマットが厳しい
```C#
static class Appointment
{
    public static DateTime Schedule(string appointmentDateDescription)
    {
	    //StringからDateTimeへ変換
        string format = "dd/MM/yyyy HH:mm:ss";
        DateTime date = DateTime.ParseExact(appointmentDateDescription,format,null);
        return date;
    }
```

### フォーマットが寛容
```C#
return DateTime.Parse(appointmentDateDescription);
```

## 時刻の比較

### DateTimeどうしで比較
```C#
    public static bool HasPassed(DateTime appointmentDate)
    {
	    //時刻の比較
        return appointmentDate.CompareTo(DateTime.Now) == -1
    }
```

### 定刻と比較
```C#
    public static bool IsAfternoonAppointment(DateTime appointmentDate)
    {
	    //DateTimeの時間だけ取得、比較
        return appointmentDate.TimeOfDay.Hour >= 12
    }
```

## DateTimeからStringへ

```C#
    public static string Description(DateTime appointmentDate)
    {
	    //
        return $"You have an appointment on {appointmentDate.ToString("M/d/yyyy h:mm:ss tt")}.";
    }
    
    //簡単に書くと
    return $"You have an appointment on {appointmentDate}.";
```

## 特定の日付を毎年更新

```C#
    public static DateTime AnniversaryDate()
    {
         return new DateTime(DateTime.Now.Year, 9, 15, 0, 0, 0);
    }
}
```

```C#
public static bool HasDaylightSavingChanged(DateTime dt, Location location)
    {
        string locationString = location switch
        {
            Location.NewYork => "Eastern Standard Time",
            Location.London  => "GMT Standard Time",
            Location.Paris   => "W. Europe Standard Time",
            _ => throw new ArgumentOutOfRangeException()
        };
    
        var tz = TimeZoneInfo.FindSystemTimeZoneById(locationString);
    
        bool? prev = null;
    
        for (int i = 0; i <= 7; i++)
        {
	        //dt.AddDays()はイミュータブル（不変）
	        //
            var check = dt.AddDays(-i);
            bool isDst = tz.IsDaylightSavingTime(check);
    
            if (prev != null && prev != isDst)
            {
                return true; // 切り替わった瞬間をキャッチ
            }
    
            prev = isDst;
        }
    
        return false;
    }
```
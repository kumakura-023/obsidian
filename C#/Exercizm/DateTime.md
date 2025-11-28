---
created: 2025-11-28T15:26
updated: 2025-11-28T15:57
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

## フォーマットが寛容
```C#
return DateTime.Parse(appointmentDateDescription);
```

##
```C#
    public static bool HasPassed(DateTime appointmentDate)
    {
	    //時刻の比較
        return appointmentDate.CompareTo(DateTime.Now) == -1
    }
```
```C#
    public static bool IsAfternoonAppointment(DateTime appointmentDate)
    {
	    //DateTimeの時間だけ取得、比較
        return appointmentDate.TimeOfDay.Hour >= 12
    }
```
```C#
    public static string Description(DateTime appointmentDate)
    {
	    //
        throw new NotImplementedException("Please implement the (static) Appointment.Description() method");
    }
```
```C#
    public static DateTime AnniversaryDate()
    {
        throw new NotImplementedException("Please implement the (static) Appointment.AnniversaryDate() method");
    }
}
```
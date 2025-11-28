---
created: 2025-11-28T15:26
updated: 2025-11-28T15:26
---
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

    public static bool HasPassed(DateTime appointmentDate)
    {
        return appointmentDate.CompareTo(DateTime.Now) == -1
    }

    public static bool IsAfternoonAppointment(DateTime appointmentDate)
    {
        return appointmentDate.TimeOfDay.Hour >= 12
    }

    public static string Description(DateTime appointmentDate)
    {
        throw new NotImplementedException("Please implement the (static) Appointment.Description() method");
    }

    public static DateTime AnniversaryDate()
    {
        throw new NotImplementedException("Please implement the (static) Appointment.AnniversaryDate() method");
    }
}
```
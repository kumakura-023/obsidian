---
created: 2025-12-02T13:16
updated: 2025-12-02T13:16
---
### 演習問題回答
```C#
enum LogLevel
{
    Trace,
    Debug,
    Info,
    Warning,
    Error,
    Fatal,
    Unknown
}
static class LogLine
{
    public static LogLevel ParseLogLevel(string logLine)
    {
        int StartIndexError = logLine.IndexOf("[");
        int EndIndexErrror = logLine.IndexOf("]");
        string ErrorOnly = logLine.Substring(StartIndexError+1, EndIndexErrror-1);
        switch (ErrorOnly)
        {
            case "TRC":    return LogLevel.Trace;
            case "DBG":    return LogLevel.Debug;
            case "INF":    return LogLevel.Info;
            case "WRN":    return LogLevel.Warning;
            case "ERR":    return LogLevel.Error;
            case "FTL":    return LogLevel.Fatal;
            default:     return LogLevel.Unknown; 
        }
    }

    public static string OutputForShortLog(LogLevel logLevel, string message)
    {
        int levelnum = 0;
        switch (logLevel)
        {
            case LogLevel.Trace:    levelnum = 1;break;
            case LogLevel.Debug:    levelnum = 2;break;
            case LogLevel.Info:     levelnum = 4;break;
            case LogLevel.Warning:  levelnum = 5;break;
            case LogLevel.Error:    levelnum = 6;break;
            case LogLevel.Fatal:    levelnum = 42;break;
            default:                levelnum = 0;break;
        }
        return $"{levelnum}:{message}";
    }
}
```

### ・switch文を簡潔に書く方法
```C#
public static string OutputForShortLog(LogLevel logLevel, string message)
{
    int levelnum = logLevel switch
    {
        LogLevel.Trace   => 1,
        LogLevel.Debug   => 2,
        LogLevel.Info    => 4,
        LogLevel.Warning => 5,
        LogLevel.Error   => 6,
        _                => 0
    };

    return $"{levelnum}:{message}";
}
```
Title: اهمیت لاگ‌ها - در کدهای TSQL چه طور اطلاعات مورد نظر رو به SQL Server Profiler بفرستیم؟
Slug: How-to-fire-events-from-tsql-code-out-to-sql-server-profiler
Date: 2017-09-01 12:00
Category: Database
Tags: T-SQL, SQL Server Profiler , Log, Trace

[هر گُلی، علت و عیبی دارد گُل بی علت و بی عیب، خداست](https://ganjoor.net/parvin/divanp/mtm/sh126/)

بعضی وقت‌ها برنامه‌هایی که نوشتیم دچار اشکال می‌شن یا بهتره بگم نوشتن نرم‌افزار بدون باگ تقریبا نشدنی است. البته مشخصه که اینجا منظورم از نرم افزار، برنامه‌ای با یکی-دو خط کد نیست! مهم اینه که باگ‌ها رو در زمان مناسب تشخیص داد و به درستی رفع و مدیریت کرد. یکی از مواردی که شناسایی باگ‌ها رو راحت‌تر می‌کنه لاگ‌ها هستن. 
جمله معروفی هست که می‌گه:

> Log early, log often

در لایه دیتابیس ابزاری که برای شناسایی خطاها و بررسی کارایی کد و پایگاه داده استفاده می‌شه [SQL Server Profiler](https://docs.microsoft.com/en-us/sql/tools/sql-server-profiler/sql-server-profiler) هست. 
برای اینکه بتونیم در رویه‌ها[^Stored procedure] و تابع‌ها[^Function] اطلاعات مورد نظر رو به پروفایلر بفرستیم؛ می‌تونیم از [sp_trace_generateevent](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql) استفاده کنیم.

این SP سه تا ورودی داره. `@eventid` و `@userinfo` و `@userdata` که از بین اونها فقط `@eventid` ضروریه. 
این پارامتر به دلخواه خودمون می‌تونه یکی از اعداد ۸۲ تا ۹۱ باشه و برای شناسایی نوع رویداد و فیلتر کردن اونها استفاده میشه.
میشه برای خودمون قرارداد کنیم که هر کدوم از این اعداد نشون دهنده یکی از سطح‌های لاگ‌گیری[^Logging levels] است.

* DEBUG
* ERROR
* FATAL
* WARN
* INFO
* TRACE

از اونجایی که لاگ‌ها بدون پیغام مناسب کاربرد کمی دارن؛ برای فرستادن متن پیام‌ها از `@userinfo` کمک می‌گیریم.

پس در نهایت توی کدهامون چیزی شبیه کد زیر داریم:

```tsql
CREATE PROCEDURE [dbo].[sp_TraceSandbox] @Param AS NVARCHAR(128)
AS
    BEGIN TRY
        
        DECLARE @info NVARCHAR(128);
        SET @info = N'Info: @param = ' + @Param;

        -- Log اطلاعات
        EXEC master..sp_trace_generateevent @eventid = 82, @userinfo = @info;

        THROW 51010, N'مثلا یه خطایی اتفاق افتاده!',1;
    END TRY
    BEGIN CATCH

        DECLARE @error NVARCHAR(128);
        SET @error = N'Error: ' + ERROR_MESSAGE();

	    -- Log خطاها
        EXEC master..sp_trace_generateevent @eventid = 83, @userinfo = @error;
    END CATCH;
GO
```




برای اینکه بتونیم این اطلاعات رو  در پروفایلر ببینیم، رویدادهای مورد نظرمون رو از بخش User configurable انتخاب می‌کنیم.
همتای هر کدوم از پارامترها در جدول زیر مشخص شدن.

| sp_trace_generateevent | SQL Server Profiler |
| ------ | ----------- |
| @eventid 82-91   | User configurable 0-9 |
| @userinfo   | TextData |
| @userdata   | BinaryData |

![SQL Server Profiler configs]({attach}/Images/2017/SQL-Server-Profiler-Captures-sp_trace_generateevent-Events.PNG)

و در نهایت وقتی که مشغول ردگیری خطاها شدیم و پروفایلر رو اجرا کردیم داده‌های مورد نظرمون رو می‌بینیم.
![Trace]({attach}/Images/2017/SQL-Server-Profiler-Trace.PNG)

[^Stored procedure]: Stored procedure
[^Function]: Function
[^Logging levels]: Logging levels
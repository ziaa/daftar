Title: ساختن بُعد تاریخ شمسی برای استفاده در Cube
Slug: Create_and_populate_Persian_date_dimension_to_be_used_in_OLAP_Cubes
Date: 2017-01-24 19:30
Category: Notes
Tags: SSAS, OLAP Cube, BI, SQL Server Analysis Services


![DimPersianDate]({attach}/Images/2017/Cube_DimPersianDate.PNG)

همان‌طور که می‌دانید در مباحث مربوط به هوش تجاری (BI)، یکی از پرکاربردترین  عناصر برای گزارش‌گیری‌های سریع، تلفیقی و چندبعدی OLAP Cubeها هستند (اگر نمی‌دانید، این متن برای شما کاربردی ندارد. وقت ارزشمند خود را تلف نکنید!). معمولاً در Cubeها **بُعد تاریخ (Date dimension)** یکی از اصلی‌ترین ابعاد است و در اکثر گزارش‌ها به آن نیاز خواهد شد.

 برای ایجاد بُعد تاریخ با تاریخ‌های شمسی می‌توان ابتدا تابعی برای تبدیل تاریخ میلادی به شمسی در SQL Server نوشت. سپس با استفاده از این تابع، جدول تاریخ‌ها در انبارداده (Data Warehouse) را برای بازه‌ی زمانیِ خاصی ایجاد کرد.

### تابع تبدیل میلادی به شمسی در SQL Server
برای این کار «چرخ را دوباره اختراع نمی‌کنیم» و از کد [این وبلاگ](http://rastan.parsiblog.com/Posts/381) و البته [نسخه سریع‌تر](http://mamehdi.parsiblog.com/Posts/1) آن (پی‌نوشت هفتم) برای ایجاد  [تابع `GregorianToJalali`](https://github.com/ziaa/SqlPersianDateDimension/blob/master/GregorianToJalali.sql) استفاده می‌کنیم. در عین حال [مشکلات و باگ‌های این کد](https://github.com/ziaa/SqlPersianDateDimension/issues) را می‌دانیم.

### ایجاد روال (Stored Procedure) برای پرکردن بُعد تاریخ در انبار داده
در اینجا [روال `spPopulatePersianDateDimension`](https://github.com/ziaa/SqlPersianDateDimension/blob/master/spPopulatePersianDateDimension.sql) را ایجاد می‌کنیم تا جدولی با عنوان `PersianDateDimension` در انبارداده ما ایجاد کند (اگر از قبل وجود نداشت) و سپس با توجه به ورودی روال، این جدول را با تاریخ‌های شمسی و میلادی متناظر برای *یک بازه زمانی خاص* پُر کند.

این روال، همه تاریخ‌های شمسی و میلادی بین تاریخ شروع `‪@FromDate‬` و پایان `‪@ToDate‬` را در جدول `PersianDateDimension`  وارد می‌کند. همان طور که در کد [مشخص است](https://github.com/ziaa/SqlPersianDateDimension/blob/874230cbcd91c736f53cd7666022b439a8130364/spPopulatePersianDateDimension.sql#L28-L43) ستون‌هایی مانند «روز هفته (`GregorianDayNumberOfWeek` و `PersianDayNumberOfWeek`)» و «روز  سال (`GregorianDayNumberOfYear` و `PersianDayNumberOfYear`)» و ... نیز در این جدول وارد می‌شود که این موارد معمولاً در گزارش‌های جزئی‌تر مورد استفاده قرار می‌گیرند.

### پر کردن بُعد تاریخ برای یک بازه زمانی
پس از ایجاد روال می‌توانیم با توجه به تاریخ‌های موجود در سایر داده‌ها و ابعاد (مثلاً مشتریان، محصولات، فروشندگان و ...) برای بازه‌ی معقولی بُعد تاریخ را ایجاد کنیم.

```
EXECUTE dbo.spPopulatePersianDateDimension @FromDate = '2016-1-1', @ToDate = '2017-1-1'
```

کار تمام است و شما می‌توانید این بُعد را برای سایر مکعب‌های خود نیز استفاده کنید.
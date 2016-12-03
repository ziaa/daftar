Title: راهنمای استفاده از سایت‌ساز پلیکان - بخش دوم - تغییر قالب و تبدیل تاریخ نوشته‌ها از میلادی به شمسی
Slug: Using-Pelican-static-site-generator-part2-changing-theme-converting-dates-to-Jalali
Date: 2016-12-03 22:30
Category: Notes
Tags: Pelican


در بخش قبل، خلاصه‌وار به [نصب و ایجاد یک وبلاگ آزمایشی با استفاده از سایت ساز پلیکان](http://daftar.ziaa.ir/posts/2016/11/Using-Pelican-static-site-generator-part1-installation) اشاره کردم. در این بخش، روش تغییر قالب و تبدیل تاریخ مطالب از میلادی به شمسی را ذکر خواهم کرد.

برای پلیکان [قالب‌های مختلفی](https://github.com/getpelican/pelican-themes) ایجاد شده است. شما به راحتی می‌توانید با نگاهی به آنها و قالب ساده‌ای مثل [simple](https://github.com/getpelican/pelican/tree/master/pelican/themes/simple/templates) و مستندات [jinja](http://jinja.pocoo.org) قالب دلخواه خود را [ایجاد کنید.](http://docs.getpelican.com/en/stable/themes.html) اگر به قالب راست-به-چپ و مناسب نوشته‌های فارسی نیاز دارید می‌توانید از [Pelican-RTL-theme](https://github.com/ziaa/Pelican-RTL-theme) استفاده کنید. این قالب برگرفته از [کار خوب تیم سُبحه](https://github.com/sobhe/pelican-sobhe) است.

### روش تغییر قالب

بهتر است پوشه‌ای با نام دلخواه مثلا `themes` برای نگهداری قالب‌های وبلاگ خود در نظر بگیرید. سپس به هر روشی که دوست دارید؛ فایل‌های مورد نیاز هر قالب را به این پوشه انتقال دهید. مثلا می‌توانید به یکی از روش‌های زیر عمل کنید:

۱. کپی کردن مخزن قالب

```
git clone https://github.com/ziaa/Pelican-RTL-theme.git themes/Pelican-RTL-theme
```

۲. اضافه کردن قالب به عنوان submodule
```
git submodule add -b master https://github.com/ziaa/Pelican-RTL-theme.git themes/Pelican-RTL-theme
```

در مرحله بعد باید مسیر قالب مورد نظر را در فایل تنظیمات پلیکان `pelicanconf.py` وارد کنید.
```
#Theme

THEME = 'مسیر/Pelican-RTL-theme' 
```

همچنین بجای این کار می‌توانید در هنگام تهیه خروجی‌های استاتیک سایت، مسیر قالب را مشخص کنید.
```
pelican content -s pelicanconf.py -t path/to/themes/Pelican-RTL-theme
```

### روش فارسی کردن تاریخ نوشته‌ها

یکی از قابلیت‌های سایت ساز پلیکان امکان [نوشتن پلاگین‌های دلخواه](http://docs.getpelican.com/en/stable/plugins.html) برای آن است. شما می‌توانید فهرستی از پلاگین‌های موجود را در [اینجا](https://github.com/getpelican/pelican-plugins) مشاهده کنید. 
برای فارسی کردن تاریخ‌ مطالب می‌توانید از پلاگین [Pelican Persian date](https://github.com/ziaa/pelican_persian_date) استفاده کنید. برای استفاده از آن ابتدا بهتر است پوشه‌ای با نام دلخواه (مثلا `plugins`) برای نگهداری پلاگین‌ها ایجاد کنید. سپس به هر روشی که دوست دارید فایل‌های پلاگین را در این پوشه قرار دهید. مثلا می‌توانید از یکی از روش‌های زیر استفاده کنید:
```
git clone https://github.com/ziaa/pelican_persian_date.git plugins/pelican_persian_date
```

```
git submodule add -b master https://github.com/ziaa/pelican_persian_date.git plugins/pelican_persian_date
```
در مرحله بعد باید مسیر پوشه `plugins` و پلاگین‌های فعال را در فایل تنظیمات پلیکان `pelicanconf.py` مشخص کنید. برای اینکار خطوط زیر را به آن اضافه کنید.
```
# Plugins
PLUGIN_PATHS = ['مسیر/plugins']
PLUGINS = ["pelican_persian_date"]
```

اگر می‌خواهید شکل خروجی تاریخ‌ها را کنترل کنید، این خطوط را به فایل `pelicanconf.py` اضافه کنید.
```
DATE_FORMATS = {
    'fa': '%A %d %B %Y'
}
```
با این کار می‌توانید مشخص کنید که تاریخ‌های فارسی به چه شکلی نمایش داده شود. مثلا *شنبه ۱۳ آذر ۱۳۹۵* یا *۱۳۹۵/۰۹/۱۳* یا هر شکل دلخواه دیگری. برای آشنایی با انواع فرمت‌ها از این [لینک](https://docs.python.org/3.4/library/datetime.html?highlight=datetime#strftime-and-strptime-behavior) کمک بگرید.

در نهایت خروجی وبلاگ را دوباره بسازید و از قالب و تاریخ فارسی لذت ببرید!
```
pelican content
```
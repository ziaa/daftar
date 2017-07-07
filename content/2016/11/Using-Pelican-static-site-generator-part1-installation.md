Title: راهنمای استفاده از سایت‌ساز پلیکان - بخش اول - نصب و راه‌اندازی اولیه
Slug: Using-Pelican-static-site-generator-part1-installation
Date: 2016-11-29 19:30
Category: Notes
Tags: Pelican

اگه علاقه دارید که سایت یا وبلاگ خودتان را با ابزارهایی که سایت استاتیک ایجاد می‌کنند، درست کنید؛ [پلیکان][پلیکان] یکی از [چندین گزینه‌ای](https://www.staticgen.com/) است که شما در اختیار دارید. چرخه استفاده از این سایت‌سازها تقریبا شبیه به هم است:

1. write
2. build
3. publish

شما نوشته مورد نظرتان را می‌نویسید، خروجی html را سایت‌ساز برای‌تان ایجاد می‌کند و در نهایت این خروجی را در هاست خود قرار می‌دهید. البته مشخص است که یک برنامه‌نویس فقط مطلب مورد نظر را می‌نویسد و با ابزارها و کدهای مختلف بقیه مراحل را به کامپیوتر واگذار می‌کند. تا حد امکان ما دوست داریم کارها **خودکار** انجام شوند!

در اینجا به صورت **خلاصه** راهنمای اولیه نصب پلیکان و ایجاد وبلاگ اولیه با استفاده از آن را می‌نویسم. توجه کنید که این مطلب صرفاً یک **یادداشت** است. برای راهنمای کامل به [مستندات پلیکان][مستندات پلیکان] مراجعه کنید.

* نصب پایتون
    *  Python 2.7.x+ یا Python 3.3+

* نصب مدیر بسته‌های پایتون pip
  * اگر در ویندوز از Python 2.7.9 یا بالاتر و Python 3.4 یا بالاتر استفاده می‌کنید؛ pip قبلاً نصب شده و گرنه باید آن‌را [نصب کنید](https://pip.pypa.io/en/stable/installing.html).
  * برای نصب pip در اوبونتو [(راهنمای نصب در سایر توزیع‌های لینوکس)](https://packaging.python.org/guides/installing-using-linux-tools/#installing-pip-setuptools-wheel-with-linux-package-managers)
```
sudo apt-get install python-pip
```

* برای به روز رسانی pip در ویندوز
```
python -m pip install --upgrade pip
```

* به روز رسانی pip در توزیع‌های لینوکس
```
pip install --upgrade pip
```

 در صورت نیاز استفاده از [virtualenv](https://virtualenv.pypa.io/en/stable/userguide/) برای ایزوله کردن محیط توسعه و بسته‌های پایتون. [(راهنمای نصب در اوبونتو)](https://askubuntu.com/questions/244641/how-to-set-up-and-use-a-virtual-python-environment-in-ubuntu)
```
sudo apt install virtualenv
virtualenv ~/virtualenvs/pelican
cd ~/virtualenvs/pelican
source bin/activate

# Do what ever you want
# example: pip install pelican
# And after you've done your job, deactivate virtualenv
deactivate
```

* نصب پلیکان
```
pip install pelican
```

* نصب Markdown‌ (در صورتیکه می‌خواهید نوشته‌ها را با مارک‌داون بنویسید)
```
pip install Markdown
```

* ساختن پوشه سایت و ایجاد ساختار اولیه
```
mkdir your-project-folder
cd your-project-folder
pelican-quickstart
```
* پاسخ‌گویی به سوالات (جواب‌های پیش‌فرض در براکت [] نوشته شده)
```
...
> What is your time zone? [Europe/Paris] Asia/Tehran
...
```

* نوشتن اولین مطلب در یک فایل md و در پوشه content
```
Title: سلام دنیا!
Slug: Hello-World
Date: 2015-07-21 23:20
Category: Test
Tags: Test

سلام دنیا!
```

* ساختن فایل‌های استاتیک وبلاگ (خروجی‌ها در پوشه output)
```
pelican content
```

* راه‌اندازی سرور local
```
cd output
python -m pelican.server
```


الان وبلاگ با قالب پیش‌فرض ساخته شده و از طریق آدرس `http://localhost:8000` قابل دسترسی است. در بخش‌های بعدی به [تغییر قالب، فارسی‌سازی](http://daftar.ziaa.ir/posts/2016/12/Using-Pelican-static-site-generator-part2-changing-theme-converting-dates-to-Jalali) و قرار دادن وبلاگ روی گیت‌هاب اشاره خواهم کرد.

[پلیکان]: http://getpelican.com
[مستندات پلیکان]:http://docs.getpelican.com

<h1 lang="fa" dir="rtl" align="right">راه‌اندازی پراکسی در لینوکس</h1>
<p lang="fa" dir="rtl" align="right">یکی از اولین نیازها برای مهاجرت به لینوکس استفاده از پراکسی هست، بخصوص که بسیاری از شرکت‌ها تحریممون کردند و توسعه نرم‌افزار رو با مشکل مواجه می‌کنند. متاسفانه آموزش‌های مناسبی هم براش سراغ ندارم در نتیجه تصمیم گرفتم این آموزش رو بنویسم، امیدوارم برای تازه کاران مفید باشه.<br>سعی می‌کنم به مرور زمان تکمیلش کنم شما هم می‌تونید کمک کنید ایراداش رو بگید روش‌های دیگه رو آموزش بدید.<br>منتظر ایشو‌ها یا پول ریکوست‌هاتون هستم.</p>
<p lang="fa" dir="rtl" align="right">برای اینکه بتونیم تحریم‌هارو دور بزنیم لازمه با ip غیر از ip ایران به اینترنت وصل بشیم. برای اینکار میشه تونلی به یک سرور خارج ایران زد و بعد ترافیک اینترنت رو از اون تونل انتقال داد. پس آموزش به دو قسمت کلی تقسیم میشه: ساخت تونل و رد کردن ترافیک از توش </p>
<p lang="fa" dir="rtl" align="right">حواستون باشه میرورهای موجود در ایران ممکنه برنامه‌هایی که برای دور زدن استفاده می‌شه رو نداشته باشند در نتیجه از میرورهای خارج ایران استفاده کنید. (یادتون نره لیست مخازنن رو آپدیت کنید)</p>
<h2 lang="fa" dir="rtl" align="right">ساخت تونل</h2>
<p lang="fa" dir="rtl" align="right">نکته: این تونل‌ها تا زمانی که ترمینال باز هست کار می‌کنه و با بستن ترمینال بسته می‌شه. اگر می‌خواید ترمینال رو ببندید می‌تونید اول دستور screen و یا nohup رو بزنید تا تونل با بستن ترمینال باز بمونه (ممکنه لازم باشه screen و nohup رو نصب کنید) ولی من پیشنهاد میدم یه گوشه‌های باز نگهش دارید تا اگر خطایی داد ببنید مشکل چیه</p>
<p lang="fa" dir="rtl" align="right">اگر دقیقا طبق این نوشته‌ها پیش برید stunnel و tor به صورت سرویس بعد از هر بار روشن شدن سیستم روی سیستم شما اجرا میشند ولی بقیه رو باید دستی اجرا کنید.</p>
<p lang="fa" dir="rtl" align="right">پیشنهاد میکنم هر کدوم از سرویس‌هارو رو یک پورت لوکال اجرا کنید (الان همشون رو  1080 هستند) تا بتونید در صورت نیاز همزمان ازشون استفاده کنید. روش‌های دیگه‌ای هم هست که میتونه هر وقت سرعت یه سرویس بالاتر بود ترافیک رو از اون روش انتقال بده که سعی میکنم به زودی اضافش کنم.</p>
<h2 lang="fa" dir="rtl" align="right">راه‌اندازی تونل به وسیله shadow socks</h2>
<p lang="fa" dir="rtl" align="right">شدو ساکس یکی از راه‌های ساخت تونل هست لازمه شما یه سرور در خارج از ایران داشته باشید از سیستم خودتون بهش وصل بشید
سمت سرور رو فکر نکنم لازم باشه بگم چون تصورم اینه که اگر سرور داشته باشید می‌تونید شدو ساکس رو هم نصب کنید ولی اگر خواستید اضافش می‌کنم.</p>
```bash
$ sudo apt install libevent-dev
$ sudo apt install python-pip
$ sudo apt install python-dev
$ sudo apt install python-m2crypto
$ sudo pip install gevent
$ sudo pip install shadowsocks
```
<p lang="fa" dir="rtl" align="right">الان شدو ساکس رو سیستم شما نصب شده و با دستور زیر می‌تونید یک تونل رو سیستم خودتون ایجاد کنید.</p>
```bash
$ sslocal -s “server” -k password -t 600 -p port  -l 1080 -m encryption
```
<p lang="fa" dir="rtl" align="right">به جای "server" آی‌پی یا آدرس سرور، به جای "password" پسورد و جای "port" پورت، جای "encryption" انکریپشن سرویس رو وارد کنید و به جای "local port" پورت لوکال سیستم که خودتون تایینش می‌کنید. بعد تونل رو سیستم شما ایجاد میشه و باید به مرحله رد کردن ترافیک از تونل برید.</p>
<p lang="fa" dir="rtl" align="right">میشه دستورات بالا رو تو یه فایل به این شکل ذخیره کرد</p>
```json
{
	"server": "sever addr",
        "server_port": port,
        "local_port": 1080,
        "password": "password",
        "timeout":600,
        "method":"aes-256-cfb"
}
```
<p lang="fa" dir="rtl" align="right">و دستور رو اینجوری زد</p>
```bash
$ sslocal -c path_file
```
<p lang="fa" dir="rtl" align="right">شدو ساکس در پورت‌های مختلف و انکریپشن‌های متفاوت سرعت‌های مختلفی میده.</p>
<h2 lang="fa" dir="rtl" align="right">تونل با کمک ssh</h2>
<p lang="fa" dir="rtl" align="right">نمی‌دونم اسم این تونل چیه ولی ازش استفاده می‌کنم
اگر دسترسی ssh به یک سرور دارید می‌تونید به این طریق یک تونل رو سیستم خودتون ایجاد کنید</p>
```bash
$ ssh user@server.address -D 1080
```
<p lang="fa" dir="rtl" align="right">به این طریق یک تونل رو پورت ۱۰۸۰ سیستمتون ایجاد میشه</p>
<h2 lang="fa" dir="rtl" align="right">تونل با کمک tor</h2>
<p lang="fa" dir="rtl" align="right">تور یکی از امن ترین شبکه‌های دنیاست</p>
```bash
$ sudo apt install tor
$ sudo apt install python-pip
$ sudo apt install python-dev gcc
$ sudo pip install obfsproxy
```
<p lang="fa" dir="rtl" align="right">سپس تنظیمات تور رو باید انجام بدید</p>
```bash
$ sudo nano /etc/tor/torrc
```
<p lang="fa" dir="rtl" align="right">موارد زیر رو به انتهای فایل اضافه کنید
با کنترل + c و انتخاب y می‌تونید تغییرات رو ذخیره کنید</p>
```bash
SocksPort 1080
SocksListenAddress 127.0.0.1
Bridge obfs3 194.132.209.187:39413
Bridge obfs3 194.68.32.131:56006
Bridge obfs3 107.191.58.23:34344
UseBridges 1
ClientTransportPlugin obfs2,obfs3 exec /usr/local/bin/obfsproxy --managed
```
<p lang="fa" dir="rtl" align="right">تعدادی از پل‌های تور هم در این تنظیمات هستند، پل‌ها ممکن است بسته شوند از <a href="https://bridges.torproject.org">https://bridges.torproject.org</a> می‌تونید پل‌های جدید بگیرید و با موارد بالا جایگزین کنید.</p>
<p lang="fa" dir="rtl" align="right">بعد ذخیره تنظیمات با دستور زیر تور را دوباره راه‌اندازی کنید</p>
```bash
$ sudo service tor restart
```
<p lang="fa" dir="rtl" align="right">حالا تونل در سیستم شما اجرا شده و لازمه ترافیک رو ازش رد کنید</p>

<h2 lang="fa" dir="rtl" align="right">ساخت تونل با yourfreedom</h2>
<p lang="fa" dir="rtl" align="right">باید قبلش برید تو سایتش ثبت‌نام کنید <a href="http://your-freedom.net/">http://your-freedom.net</a><br>از قسمت دانلود نسخه جاوا دانلود کنید<br>از حالت فشرده خارج کنید<br>با cd به محل اکسترکت شده برید و دستور زیر رو وارد کنید</p>
```bash
java -jar freedom.jar
```
<p lang="fa" dir="rtl" align="right">حواستون باشه که لازمه قبلش جاوا رو نصب کرده باشید. اگر برای نصب جاوا به مشکل خوردید بگید شاید برای اونم یه چیزی ردیف کردیم.<br>خوب فیری‌دام باید اجرا شه. در اولین دفعه خودش پنجره کانفیگ رو باز می‌کنه، اگر بعد از مدتی دیدید کنده یا کار نمی‌کنه دوباره از همین قسمت دنبال سرور‌های جدید بگردید.<br>در پنجره کانفیگ چندین بار نکست رو بزنید تا شروع کنه دنبال سرورهاش بگرده (طول میکشه)
بعد از پیدا شدن سرورها یکیش رو انتخاب کنید و نکست رو بزنید.<br>یوزر و پسورد اکانتتون رو وارد کنید، ذخیره کنید و بیاید بیرون<br>استارت کانکشن رو بزنید بعد از چند ثانیه ارتباط برقرار میشه</p>
<h2 lang="fa" dir="rtl" align="right">stunnel</h2>
```bash
$ sudo apt install stunnel4
```
<p lang="fa" dir="rtl" align="right">اگر فایل اصلی کانفیگ وجود داره پاکش کنید</p>
```bash
$sudo rm /etc/stunnel.conf
```
<p lang="fa" dir="rtl" align="right">و دوباره بسازیدش</p>
```bash
$ sudo nano /etc/stunnel.conf
```
<p lang="fa" dir="rtl" align="right">این تنظیمات رو بزارید توش</p>
```bash
 pid = /stunnel4.pid
 client=yes
 [name]
 accept=1080
 connect=serverAdress:serverPort
 ```
<p lang="fa" dir="rtl" align="right">با کنترل + c و انتخاب y ذخیرش کنید.<br>به مسیر زیر برید و ENABLED برابر 1 کنید.</p>
```bash
$ sudo nano /etc/default/stunnel4
```
<p lang="fa" dir="rtl" align="right">ذخیره کنید و stunnel رو ری‌استارت کنید</p>
```bash
$ sudo service stunnel4 restart
```
<p lang="fa" dir="rtl" align="right">تونل رو سیستم شما ایجاد شده و حالا باید ترافیکتون رو ازش رد کنید.</p>
<h2 lang="fa" dir="rtl" align="right">openvpn</h2>
<p lang="fa" dir="rtl" align="right">از openvpn هم میشه در لینوکس استفاده کرد اول نصبش کنید (فایل کانفیگ رو یا خودتون باید تو سرورتون بسازید یا از کسی که دارید از سرویسش استفاده می‌کنید بگیرید)</p>
```bash
$ sudo apt install openvpn
```
<p lang="fa" dir="rtl" align="right">فایل کانفیگ openvpn رو دانلود کنید و به این طریق وصل شید</p>
```bash
$ sudo openvpn filename
```
<p lang="fa" dir="rtl" align="right">با اجرا این دستور کل ترافیک شما میره سمت سرور و لازم نیست پراکسی جایی ست کنید، در نتیجه تمام تنظیمات پراکسی‌هاتون رو مستقیم کنید.</p>

<h2 lang="fa" dir="rtl" align="right">رد کردن ترافیک از تونل</h2>
<p lang="fa" dir="rtl" align="right">اگر دقیقا مثل تنظیماتی که نشون دادیم جلو رفته باشید با هر کدوم از روش‌ها یه تونل از نوع socks5 رو پورت 1080 و آی‌پی سیتمتون 127.0.0.1 دارید.<br>حالا باید هر چیزی رو که می‌خواید از توی این تونل رد کنید.
خود سیستم عامل تو قسمت نتورک تنظیماتی برای پراکسی داره ولی من ازش خوشم نمیاد و فکر کنم بعضی از برنامه‌ها به اون تنظیمات کاری ندارند.<br>روش‌های بهتری هم هست</p>
<h2 lang="fa" dir="rtl" align="right">فایرفاکس</h2>
<p lang="fa" dir="rtl" align="right"><a href="https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/">https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/</a><br>foxyproxy یه پلاگین برای فایرفکس و  فکر کنم کروم هست که می‌تونید خیلی راحت و دم دستی تنظیمات پراکسی رو توش تغییر بدید.<br>پلاگین رو نصب کنید، add new proxy رو بزنید، ip رو 127.0.0.1 و پورت 1080  بزارید، نوع پراکسی رو socks5 انتخاب کنید و ذخیره کنید.<br>از قسمت مود پراکسی‌ای که ساختید رو فعال کنید.</p>
<p lang="fa" dir="rtl" align="right">نکته:‌بدون این پلاگین هم میشه از تنظیمات فایرفکس پراکسی رو تغییر داد ولی این دم دسته</p>
<p lang="fa" dir="rtl" align="right">می‌تونید تنظیمات دیگه‌ای هم بهش اضافه کنید مثلا aparat.com رو از پراکسی رد نکنه تا سرعت بالاتری داشته باشه</p>

<h2 lang="fa" dir="rtl" align="right">کروم</h2>
<p lang="fa" dir="rtl" align="right"><a href="https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=en">https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=en</a><br>اکستنشنی هست به نام SwitchyOmega که می‌تونه تنظیمات پراکسی کروم رو باهاش دست کاری کرد. نصبش کنید.<br>کنار آدرس بار آیکنش اضافه می‌شه از option گزینه new profile رو انتخاب کنید، یه اسم براش وارد کنید و گزینه proxy profile رو بزنید. پروتکل رو socks5 انتخاب کنید. سرور 127.0.0.1 و پورت 1080 باشه در انتها هم apply change رو بزنید.<br>هر وقت خواستید می‌تونید با کلیک رو آیکنش به راحتی ارتباط رو مستقیم کنید یا از پراکسی رد کنید.<br>تنظیمات دیگه‌ای هم داره مثلا بزنید تمام سایت‌هایی که با ir تموم میشند بدون پراکسی باشند.</p>


<h2 lang="fa" dir="rtl" align="right">ترمینال</h2>
<p lang="fa" dir="rtl" align="right">گاهی لازمه ترافیک ترمینال هم از پراکسی رد شه، مثلا زمانی که می‌خواید کروم رو آپدیت کنید یا برای نصب جاوا و … چون این شرکت‌ها مارو تحریم کردند.<br>برنامه‌ای هست به نام proxychains اول هر کامندی که بزنید ترافیکش از پراکسی رد میشه، اول نصبش کنید</p>
```bash
$ sudo apt install proxychains
```
<p lang="fa" dir="rtl" align="right">بعد کامند زیر رو بزنید</p>
```bash
$ sudo nano /etc/proxychains.conf
```
<p lang="fa" dir="rtl" align="right">به آخر فایل برید و اول خط آخر یک # اضافه کنید تا تنظیمات دیفالت کامنت شه
و خط زیر به آخرش اضافه کنید</p>
```bash
socks5  127.0.0.1 1080
```
<p lang="fa" dir="rtl" align="right">خوب حالا می‌تونید با زدن proxychains اول هر دستوری ترافیکش رو از تونل رد کنید.<br>نکته اگر می‌خواید سیستم تون رو آپدیت کنید اول sudo -s و بعدش proxychains apt update</p>


<h2 lang="fa" dir="rtl" align="right">تبدیل socks5 به http</h2>
<p lang="fa" dir="rtl" align="right">هنوز یه مشکلی هست بعضی از برنامه‌ها تو تنظیمات پراکسیشون  socks5 ندارند. مثل اندروید استادیو و sdk! در نتیجه نمیشه sdk رو آپدیت کرد چون گوگل تحریممون کرده، بنابراین ما socks5 رو تبدیل به http می‌کنیم که این برنامه‌ها ازش پشتیبانی می‌کنند. به کمک برنامه polipo که مثل یه شلنگ می‌مونه، از یه ور socks5 می‌گیره از یه ور دیگه http تحویل میده</p>
```bash
sudo apt install polipo
```
<p lang="fa" dir="rtl" align="right">و بعد برای کافیگش دو خط زیر رو</p>
```bash
socksParentProxy = localhost:1080
socksProxyType = socks5
```
<p lang="fa" dir="rtl" align="right">به انتهای فایل</p>
```bash
$ sudo nano /etc/polipo/config
```
<p lang="fa" dir="rtl" align="right">اضافه کنید با کنترل + c و انتخاب y تغییرات رو ذخیره کنید و بعدش Polipo رو ری‌استارت کنید</p>
```bash
$ sudo service polipo restart
```
<p lang="fa" dir="rtl" align="right">حالا یه پراکسی دیگه هم رو سیستم داریم همون آی‌پی 127.0.0.1 ولی رو پورت 8123 و از نوع http که به راحتی می‌تونید رو همه سیستم‌ها از جمله اندروید استادیو اضافش کنید</p>

<h2 lang="fa" dir="rtl" align="right">و در آخر</h2>
<p lang="fa" dir="rtl" align="right">
<p lang="fa" dir="rtl" align="right">اگر جایی نا واضح هست بگید با افزودن عکس یا توضیحات دقیق‌تر بهترش کنیم.<br>اگر چیزی کار نکرد ارور رو به دقت مطالعه کنید سعی کنید برطف کنید، نشد سرچ، نشد کمکتون می‌کنیم.</p>
<p><a href="http://creativecommons.org/licenses/by-sa/3.0/">http://creativecommons.org/licenses/by-sa/3.0/</a>


<div dir="rtl" align="right">
<h1>
گرفتن دامپ یک برنامه باینری
</h1>

ابتدا برنامه‌ی Hello World را به زبان c نوشته و سپس آن را توسط کامپایلر gcc کامپایل کردیم تا خروجی a.exe را (در ویندوز) به ما بدهد. سپس با زدن دستور زیر، وارد محیط radare شدیم تا برروی این برنامه کار کنیم.
</div>

```
radare2 a.exe
```

<div dir="rtl" align="right">
حال با زدن دستور pd، می‌توانیم دامپ برنامه را ببینیم. این دستور تعداد خط دستوراتی که می‌خواهیم مشاهده کنیم را ورودی می‌گیرد و با زدن عبارت زیر کل دستورات برنامه را نمایش می‌دهد. چون نمایش کل دستورات به‌درد ما نمی‌خورد، از برنامه خواستیم تا 400 دستور را نمایش دهد. سپس با جست‌وجو در این دستورات، محلی که عبارت Hello World پرینت می‌شود را پیدا کردیم که در تصویر دور آن خط قرمز کشیده شده است.
</div>

```
$s
```

![pd$s](https://github.com/S4LabTest/S4lab_Walkthrough/blob/master/images/pd$s.jpg)
![helloworld](https://github.com/S4LabTest/S4lab_Walkthrough/blob/master/images/helloworld.jpg)

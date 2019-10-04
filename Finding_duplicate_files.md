<div dir="rtl" align="right">
<h1>
پیدا کردن فایل‌های تکراری در حجم زیاد فایل
</h1>
برای این بخش از منبعی استفاده نکرده‌ام و استدلال‌ها را خودم نوشته‌ام و تقریباً به درستی آن‌ها مطمئن‌ام که آن را سابمیت کرده‌ام.

اول از هر چیز، برای آن‌که بتوانیم فایل‌های مشابه را پیدا کنیم، حداقل نیاز تا هر فایل را یک‌بار موردبررسی قرار بدهیم. اگر فرض کنیم این بررسی از زمان O(1) انجام می‌شود، نیاز به O(n) عملیات داریم که در اینجا n=2**48. این را از این جهت گفتم که مطمئن شویم راهی سریع‌تر از این وجود ندارد.

حال به ارائه‌ی الگوریتم پیدا کردن فایل‌های مشابه میان این تعداد فایل می‌پردازیم. الگوریتم ما دارای پیچیدگی زمانی O(nlgn) است که البته اگر از روش دوم استفاده شود، با توجه به کوچکی پایه‌ی لگاریتم، می‌شود با تقریب خوبی در حالت میانگین آن را از O(n) در نظر گرفت.

ابتدا مشخص است که فایل‌های مشابه، قطعاً سایز مشابهی هم دارند. در نتیجه کدی می‌نویسیم تا فایل‌ها را بر اساس سایز آن‌ها دسته‌بندی کند.

حال برای این‌که تشخیص بدهیم دو فایل هم‌سایز، یکسان هستند یا خیر، به‌جای مقایسه‌ی تک‌تک کاراکترهای آن‌ها که ممکن است بسیار زیاد باشند و کار ما را سخت کنند، از کل فایل یک خروجی hash می‌گیریم. ثابت می‌شود که تولید هش از یک فایل به ازای فایل‌های کمی بزرگ، سریع‌تر از مقایسه‌ی تک‌به‌تک کاراکترهای آن‌هاست. همچنین برای آن‌که سرعت کد خود را باز هم بهبود دهیم، می‌توانیم از الگوریتم درهم‌سازی استفاده کنیم که پیچیدگی زیادی مانند خانواده‌ی sha نداشته باشد اما برای کار ما کافی باشد. الگوریتم md5 (یا حتی md4) به دلیل داشتن فضای حالتی بزرگ‌تر از تعداد فایل‌های ما، امّا نه به بزرگی خانواده‌ی sha برای این کار مناسب هستند.

در روش دوم، می‌توانیم فایل‌های با سایز یکسان را، ابتدا براساس کاراکتر اول‌شان، سپس براساس کاراکتر دوم و همینطور الی آخر به گروه‌های مختلفی تقسیم کنیم. با این‌کار با احتمال بالا پس از نهایتا 5 بار مقایسه، فایل‌ها در دسته‌هایی یکتا تقسیم می‌شوند در نتیجه در حالت میانگین الگوریتم ما دارای پیچیدگی زمانی O(n) است.

تکه کد زیر، روش اول (استفاده از توابع درهم‌ساز) را پیاده کرده است. برای پیاده‌سازی روش دوم کافی است به‌جای مقایسه هش فایل‌ها، کاراکترهای آن‌ها را دوبه‌دو مقایسه کنیم.
</div>

```python
import hashlib
import os

target_files_path = 'files/'
files = os.listdir(target_files_path)
duplicate_files = []
sizes_range = {}


# splitting files to different groups by their size
for file in files:
    file_size = os.stat(target_files_path + file).st_size
    if file_size not in sizes_range.keys():
        sizes_range[file_size] = [file]
    else:
        temp_files_list = sizes_range.get(file_size)
        temp_files_list.append(file)
        sizes_range[file_size] = temp_files_list

#  splitting files to groups with same hash
#  (second way: instead of this part of code,
#  we split files with same size to group which
#  have the same 'first character', 'second character'
#  and so on. this will make a trie data-structure and
#  have complexity of O(nlgn)
for files in sizes_range.values():
    same_hash = {}
    for file_name in files:
        file_handler = open(target_files_path+file_name, 'rb')
        file_hash = hashlib.md5(file_handler.read()).hexdigest()
        file_handler.close()
        if file_hash not in same_hash.keys():
            same_hash[file_hash] = [file_name]
        else:
            prev_files = same_hash.get(file_hash)
            same_hash[file_hash] = prev_files.append(file_name)

        # here we say files with the same hash are equal. but
        # you can add additional check by comparing them line by line
        # or something else.
        for same_files in same_hash.values():
            if same_files is not None:
                duplicate_files.append(same_files)

print(duplicate_files)

```

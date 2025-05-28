# 🔐 تغییر پسورد Administrator لوکال از طریق GPO (با PowerShell و Batch)

این راهنما به شما آموزش می‌دهد چطور از طریق Group Policy Object (GPO) به‌صورت متمرکز و خودکار رمز عبور حساب **Administrator لوکال** را در همه کلاینت‌های دامنه تغییر دهید.

---

## 🌀 روش اول: استفاده از PowerShell (`.ps1`)

### مراحل:

1. وارد کنترلر دامنه (Active Directory) شوید.

2. یک فایل Notepad جدید بسازید.

3. کد زیر را داخل فایل وارد کنید:

    ```powershell
    net user Administrator /active:yes
    net user Administrator *Afzargsi
    ```

    > ⚠️ به‌جای `*Afzargsi` می‌توانید رمز دلخواه خود را بنویسید. استفاده از `*` باعث می‌شود رمز به‌صورت تعاملی وارد شود و در GPO عمل نکند.

4. فایل را با پسوند `.ps1` ذخیره کنید. (مثلاً `ChangeLocalAdminPass.ps1`)

5. فایل را در مسیر زیر قرار دهید:  
    ```
    \\YourDomain\SysVol\YourDomain\Scripts\ChangeLocalAdminPass.ps1
    ```

6. به کنسول Group Policy Management (`gpmc.msc`) بروید.

7. روی دامنه یا OU مورد نظر راست‌کلیک کرده و یک GPO جدید بسازید یا یکی از موجودها را Edit کنید.

8. مسیر زیر را دنبال کنید:

    ```
    Computer Configuration → Policies → Windows Settings → Scripts (Startup)
    ```

9. روی **Startup** دابل‌کلیک کرده و روی **Add** بزنید.

10. در پنجره باز شده:

    - در **Script Name** بنویسید:  
      ```
      powershell.exe
      ```
    - در **Script Parameters** بنویسید:  
      ```
      -ExecutionPolicy Bypass -File "\\YourDomain\SysVol\YourDomain\Scripts\ChangeLocalAdminPass.ps1"
      ```

11. روی یکی از کلاینت‌ها دستور زیر را بزنید و سپس ریستارت کنید:

    ```cmd
    gpupdate /force
    ```

---

## 💾 روش دوم: استفاده از Batch فایل (`.bat`)

### مراحل:

1. وارد کنترلر دامنه شوید.

2. یک فایل Notepad جدید بسازید.

3. کد زیر را داخل آن وارد کنید:

    ```bat
    powershell.exe -ExecutionPolicy Bypass -Command "net user Administrator /active:yes"
    powershell.exe -ExecutionPolicy Bypass -Command "net user Administrator 'Geo@2025Strong'"
    ```

4. فایل را با پسوند `.bat` ذخیره کنید. (مثلاً `ChangeLocalAdminPass.bat`)

5. فایل را در مسیر زیر قرار دهید:  
    ```
    \\YourDomain\SysVol\YourDomain\Scripts\ChangeLocalAdminPass.bat
    ```

6. به کنسول Group Policy Management (`gpmc.msc`) بروید.

7. روی دامنه یا OU مورد نظر راست‌کلیک کرده و یک GPO بسازید یا یکی از GPOها را ویرایش کنید.

8. به مسیر زیر بروید:

    ```
    Computer Configuration → Policies → Windows Settings → Scripts (Startup)
    ```

9. روی **Startup** دابل‌کلیک کرده و روی **Add** بزنید.

10. این‌بار روی **Browse** کلیک کرده و فایل `.bat` را از مسیر `Scripts` انتخاب کنید.

11. روی کلاینت‌ها دستور زیر را اجرا کنید:

    ```cmd
    gpupdate /force
    ```

---

## 📌 نکات مهم:

- نام دامنه را در مسیر فایل‌ها به درستی جایگزین کنید.
- اجرای صحیح GPO را می‌توانید با دستور `gpresult /h result.html` بررسی کنید.
- پیشنهاد می‌شود پس از اجرای موفق، فایل اسکریپت را از مسیر `Scripts` حذف کنید تا در دسترس عموم نباشد.

---

## ✅ نتیجه

با استفاده از این روش، شما می‌توانید رمز عبور حساب لوکال Administrator را به‌صورت امن و متمرکز روی همه سیستم‌های عضو دامنه اعمال و مدیریت کنید.


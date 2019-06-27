
<div dir="rtl" >
**باسمه تعالی**
در ابتدا کتابخانه را به پروژه خود اضافه کنید. برای اضافه کردن کتابخانه به برنامه اندرویدیتان ابتدا فایل را در پوشه libs در زیر شاخه app قرار دهید. سپس کدهای زیر را به فایل build.gradle در شاخه app اضافه کنید.
</div>

```
implementation 'com.google.code.gson:gson:2.8.5'
implementation files('libs/raygansms.aar')
```
<div dir="rtl" >
برنامه شما برای دریافت اطلاعات مورد نیاز باید دسترسی به اینترنت هم داشته باشد. برای این منظور خط زیر را در فایل AndroidManifest.xml  اضافه کنید.
</div>

```
<uses-permission android:name="android.permission.INTERNET" /> 
```

اگر ProGuard  را در برنامه تان فعال کرده‌اید کدهای زیر را هم به فایل آن اضافه کنید.

```
-keep public enum ir.trez.raygansms.**{
    *;
}
-keep public class ir.trez.raygansms.**{
    *;
}
```

<div dir="rtl" >
حالا می‌توانید در برنامه اندرویدیتان از آن استفاده کنید.

برای استفاده از کتابخانه ابتدا یک نمونه از کلاس Raygansms ایجاد می کنید. دقت کنید اطلاعات ورودی مورد نیاز برای سازنده این نمونه نام کاربری و رمز سامانه پیامکی خودتان است. در ادامه هر یک از متدهای آنرا شرح می دهیم.

**احراز هویت (متد**  **getAuthHeader**** )**

این متد رشته مورد نیاز برای احراز هویت  (فیلد Authorization) را ایجاد می کند. این متد مقدار رشته ای بر می گرداند.

**مقدار برگشتی متدها (شی** **Result)**

متدهای دیگر کتابخانه (به غیر از getAuthHeader ) از این نوع بر می گردانند. این نوع شامل سه متغییر است. یکی Code از نوع ResultCode و بیانگر موفقیت آمیز بودن عملیات یا شماره خطا است. نوع Message متن نتیجه را مشخص می کند. نوع Result نتیجه مربوط به متد را مشخص می‌کند و می‌تواند عدد، رشته و از نوع JSON باشد.
</div>
   
| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| Code | ResultCode | کد نتیجه عملیات |
| Message | String | متن نتیجه عملیات |
| Result | JsonElement | اطلاعات دیگر عملیات درخواستی |

<div dir="rtl" >
در کد زیر نمونه کدی برای دریافت اعتبار حساب استفاده شده است. دقت کنید برای اجرا شما باید از کلاس Raygansms یک متغیر ایجاد کرده و متد مربوط به اجرا را در کد فرابخوانید.
 </div>
 
```
**private** Raygansms **raygansms** = **new** Raygansms( **&quot;username&quot;** , **&quot;password&quot;** );

**private** String[] **Mobiles** = { **&quot;09120000000&quot;** , **&quot;09120000001&quot;** };

**private** RecipientsMessage[] **recipientsMessages** = {};

**private** String[] **MessageIDs** = { **&quot;1&quot;** , **&quot;2&quot;** };

**private** String **PhoneNumber** = **&quot;5000000000&quot;** ;

**private** String **UserGroupID** = **&quot;1&quot;** ;

**private** Integer **PORT** = 90;

**private** String _Hello_ = **&quot;** سلام **&quot;** ;

**public class** CallSMS **extends** AsyncTask\&lt;Void, Void, Result\&gt; {

    @Override

    **protected void** onPreExecute() { }

    @Override

    **protected** Result doInBackground(Void... voids) {

        **try** {

            **return**  **raygansms**.GetCredit();

        } **catch** (IOException e) {

            e.printStackTrace();

        }

        **return null** ;

    }

    @Override

    **protected void** onPostExecute(Result result) {

        updateUiWithResult(result);

    }

}

```

در اینجاد در متد updateUiWithResult نتیجه مورد نیاز را بررسی کرده و بر اساس خروجی نتیجه مورد نیاز را به کاربر نمایش دهید. برای نمونه کد زیر را مشاهده کنید.

**private void** updateUiWithResult(Result result) {

    **if** (result != **null** &amp;&amp; **textView**!= **null** &amp;&amp; ! **this**.isFinishing()) {

        String text = **&quot;Code:**** \t ****&quot;** + result.getCode() + **&quot;**** \n ****Message:**** \t ****&quot;** + result.getMessage();

        **if** (result.getResult() != **null** ){

            text += **&quot;**** \n ****Result:**** \t ****&quot;** + result.getResult();

        }

        **textView**.setText(text);

    }

}

در ادامه متدهای کتابخانه را شرح می دهیم.

ارسال پیام

ارسال پیام گروهی ( **متد**  **SendMessage** )

از این متد برای ارسال پیام گروهی استفاده می شود. بدیهی است از این پیام برای ارسال پیام تکی نیز میتوان استفاده نمود.

| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| phoneNumber | String | شماره اختصاصی |
| message | String | متن پیام ارسالی |
| mobiles | String[] | آرایه ای از شماره موبایل ها برای ارسال پیام |
| UserGroupID | String | گروه پیام |
| SendDateInTimeStamp | Long | تاریخ ارسال پیام به صورتTimeStamp  (به ثانیه) |

نمونه کد فراخوانی:

raygansms.SendMessage(PhoneNumber, Hello, Mobiles, UserGroupID, System.currentTimeMillis() / 1000L);

**ملاحضات:**

در صورتی که تاریخ ارسال، از تاریخ فعلی کمتر باشد یا به عبارتی دیگر از زمان مورد نظر عبور کرده باشید، پیام مورد نظر در لحظه ارسال خواهد شد.

ارسال پیام متناظر ( **متد**  **SendCorrespondingMessage** )

از این متد برای ارسال پیام متناظر استفاده می شود.

| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| phoneNumber | String | شماره اختصاصی |
| recipientsMessage | RecipientsMessage[] | آرایه ای از شماره ها و پیام های متناظر |
| UserGroupID | String | گروه پیام |

نمونه کد فراخوانی:

raygansms.SendCorrespondingMessage(PhoneNumber, recipientsMessages, UserGroupID);

ارسال پیام به پورت خاص ( **متد**  **SendMessageToPort** )

از این متد برای ارسال پیام به پورت خاص استفاده می شود.

| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| phoneNumber | String | شماره اختصاصی |
| recievePortNumber | int | شماره پورت دریافت پیام |
| sendPortNumber | int | شماره پورت دریافت پیام |
| UserGroupID | String | گروه پیام |
| recipientsMessage | RecipientsMessage[] | آرایه ای از شماره ها و پیام های متناظر |

نمونه کد فراخوانی:

raygansms.SendMessageToPort(PhoneNumber, PORT, PORT, UserGroupID, recipientsMessages);

مشاهده وضعیت ارسال پیام گروهی ( **متد**  **GroupMessageStatus** )

از این متد برای واکشی، وضعیت لیست پیام های ارسالی استفاده می شود.

| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| groupMessageId | String | شناسه گروه ارسال پیام |

نمونه کد فراخوانی:

raygansms.GroupMessageStatus(UserGroupID);

مشاهده وضعیت ارسال پیام متناظر ( **متد**  **CorrespondingMessageStatus** )

از این متد برای واکشی ، وضعیت لیست پیام های ارسالی استفاده می شود.

| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| messageId | String[] | شناسه گروه ارسال پیام |

نمونه کد فراخوانی:

raygansms.CorrespondingMessageStatus(MessageIDs);

دریافت شناسه گروه پیام ( **متد**  **GetGroupMessageId** )

از این متد برای دریافت ، شناسه گروه پیام ارسالی استفاده می شود.

| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| groupId | String | شناسه ارسال پیام کاربر |

نمونه کد فراخوانی:

raygansms.GetGroupMessageId(UserGroupID);

پیام های دریافتی ( **متد**  **ReceiveMessages** )

از این متد برای واکشی ، لیست پیام های در یافتی استفاده می شود.

| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| phoneNumber | String | شماره اختصاصی |
| startDate | Long | تاریخ شروع به صورت TimeStamp   |
| EndDate | Long | تاریخ پایان به صورت TimeStamp |
| page | int | شماره صفحه |

نمونه کد فراخوانی:

raygansms.ReceiveMessages(PhoneNumber, (System.currentTimeMillis() - (60 \* 60 \* 24 \* 60)) / 1000L,System.currentTimeMillis() / 1000L, 1);

دریافت اعتبار ( **متد**  **GetCredit** )

از این متد برای واکشی ، اعتبار کاربر استفاده می شود.

نمونه کد فراخوانی:

raygansms.GetCredit();

قیمت پیامک ( **متد**  **GetPrices** )

از این متد برای واکشی تعرفه ارسال پیامک توسط کاربر استفاده می شود.

نمونه کد فراخوانی:

raygansms.GetPrices();

بررسی شماره ها در لیست سیاه ( **متد**  **ShowWhiteList** )

خروجی متد زیر لیست شماره موبایل هایی است که در لیست سیاه قرار ندارند.

| نام پارامتر | نوع پارامتر | توضیحات |
| --- | --- | --- |
| Mobiles | String[] | لیستی از شماره موبایل ها برای بررسی |

نمونه کد فراخوانی:

raygansms.ShowWhiteList(Mobiles);

تفسیر کد های خروجی

| نوع ResultCode | کد خطا | توضیح خطا |
| --- | --- | --- |
| Success | 0 | عملیات با موفقیت انجام شد |
| DocError | 1001 | فرمت سند ارسالی صحیح نمی باشد |
| NumberError | 1002 | شماره اختصاصی وارد شده معتبر نمی باشد |
| DateError | 1003 | فرمت تاریخ ارسالی صحیح نمی باشد   |
| ParamError | 1004 | پارامتر های ارسالی برای درخواست مورد نظر معتبر نمی باشد |
| OwnNumberError | 2001 | مالکیت شماره اختصاصی مورد نظر برای کاربری وارد شده معتبر نمی باشد |
| UserError | 2002 | کاربری مورد نظر مجوز استفاده از وب سرویس را ندارد |
| IPError | 2003 | آدرس آی پی ، درخواست دهنده غیر مجاز می باشد |
| DateRangeError | 2004 | تاریخ ارسال در نظر گرفته شده در محدوده مجاز نمی باشد |
| UserListError | 2005 | تعداد مخاطبین حداکثر می تواند50000عدد باشد |
| MessageLengthError | 2006 | طول پیام نمی تواند بیش از10پیام باشد |
| PortError | 2007 | مقدار وارد شده برای شماره پورت غیر مجار می باشد |
| PageError | 2008 | مقدار وارد شده برای شماره صفحه غیر مجاز می‌باشد |
| UserInfoError | 2009 | خطا در واکشی اطلاعات کاربری |
| RegisterInfoError | 3001 | خطا در ثبت اطلاعات |
| GroupError | 3002 | خطا در دریافت گروه پیام |
| CreditError | 3003 | اعتبار کافی نمی باشد |
| ServiceError | 3004 | سرویس مورد نظر برای اپراتور مد نظر ، تعریف نشده است |
| ServerError | 5001 | به دلیل خطای داخلی ، سرور قادر به پاسخگویی نیست |
| SendError | 5002 | در هنگام ارسال پیام خطایی رخ داده است |
| ReceiveError | 5003 | در هنگام دریافت نتیجه ارسال پیام خطایی رخ داده است |
| ParamSendError | 5004 | برخی پیام ها در هنگام ارسال با خطا مواجه شده اند |

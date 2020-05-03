<div dir="rtl">

پکیج اتصال به بانک سامان.

این پکیج با ورژن های
(  6,7  )
 لاراول سازگار می باشد


پشتیبانی تنها از درگاهای زیر می باشد:
 1. SAMAN
 2. PAYPAL 
----------


**نصب**:

دستورات زیر را جهت نصب دنبال کنید :

**مرحله ۱)**

</div>


```php

composer require imerit-pro/gateway

```   

<div dir="rtl">

**مرحله ۲) - انتقال فایل های مورد نیاز**


برای لاراول ۶ به بعد :
</div>

```php

php artisan vendor:publish 

// then choose : GatewayServiceProviderLaravel6

```

<div dir="rtl"> 

**مرحله ۳) - ایجاد جداول**

</div>

```php

php artisan migrate

```


<div dir="rtl"> 
 
**مرحله ۴)**

عملیات نصب پایان یافته است حال فایل gateway.php را در مسیر app/  باز نموده و  تنظیمات مربوط به درگاه بانکی مورد نظر خود را در آن وارد نمایید .

حال میتوایند برای اتصال به api  بانک  از یکی از روش های زیر به انتخاب خودتان استفاده نمایید . (Facade , Service container):
</div>
 
 1. Gateway::make(new Smana())
 2. Gateway::make('saman')
 3. Gateway::saman()
 4. app('gateway')->make(new Saman());
 5. app('gateway')->saman();
 
<div dir="rtl">

 مثال :‌اتصال به بانک ملت (درخواست توکن و انتقال کاربر به درگاه بانک)
توجه :‌ مقدار متد price   به ریال وارد شده است و معادل یکصد تومان می باشد

یک روت از نوع GET با آدرس /bank/request ایجاد نمایید و کد های زیر را در آن قرار دهید .

</div>


```php

try {

   $gateway = \Gateway::make('saman');

   $gateway->setCallback(url('/bank/response')); // You can also change the callback
   $gateway->price(1000)
           // setShipmentPrice(10) // optional - just for paypal
           // setProductName("My Product") // optional - just for paypal
           ->ready();

   $refId =  $gateway->refId(); // شماره ارجاع بانک
   $transID = $gateway->transactionId(); // شماره تراکنش

   // در اینجا
   //  شماره تراکنش  بانک را با توجه به نوع ساختار دیتابیس تان 
   //  در جداول مورد نیاز و بسته به نیاز سیستم تان
   // ذخیره کنید .

   return $gateway->redirect();

} catch (\Exception $e) {

   echo $e->getMessage();
}

```

<div dir="rtl">

 و سپس روت با مسیر /bank/response  و از نوع post  ایجاد نمایید و کد های زیر را در آن قرار دهید :

</div>


```php

try { 

   $gateway = \Gateway::verify();
   $trackingCode = $gateway->trackingCode();
   $refId = $gateway->refId();
   $cardNumber = $gateway->cardNumber();

   // تراکنش با موفقیت سمت بانک تایید گردید
   // در این مرحله عملیات خرید کاربر را تکمیل میکنیم

} catch (\Imerit\Gateway\Exceptions\RetryException $e) {

    // تراکنش قبلا سمت بانک تاییده شده است و
    // کاربر احتمالا صفحه را مجددا رفرش کرده است
    // لذا تنها فاکتور خرید قبل را مجدد به کاربر نمایش میدهیم

    echo $e->getMessage() . "<br>";

} catch (\Exception $e) {

    // نمایش خطای بانک
    echo $e->getMessage();
}

```

<div dir="rtl">
 
در صورت تمایل جهت همکاری در توسعه   :

 1. توسعه مستندات پکیج.
 2. گزارش باگ و خطا.
 3. همکاری در نوشتن ماژول دیگر بانک ها برای این پکیج .


درصورت بروز هر گونه 
 [باگ](https://github.com/imeirt-pro/gateway/issues) یا [خطا](https://github.com/imeirt-pro/gateway/issues)  .
  ما را آگاه سازید .
  
این پکیج از پکیج دیگری بنام  larabook/gateway  مشتق شده است 
</div>

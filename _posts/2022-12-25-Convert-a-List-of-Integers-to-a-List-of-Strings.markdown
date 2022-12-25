---
layout: post
title:  "تبدیل لیست از Integer ها به لیست از String"
date:   2022-12-25 11:07:52 +0330
categories: java collection
---
از نسخه 5، جاوا از جنریک ها(generics) پشتیبانی می کند. یکی از مزایایی که جنریک های جاوا برای ما به ارمغان می آورد ایمنی نوع (type safety) است. به عنوان مثال، وقتی یک شی myList را به عنوان `List<Integer>` اعلام کرده‌ایم، نمی‌توانیم عنصری را که نوع آن غیر از عدد صحیح است در myList قرار دهیم.
با این حال، وقتی با مجموعه‌های عمومی کار می‌کنیم، اغلب می‌خواهیم `Collection<TypeA>` را به `Collection<TypeB>` تبدیل کنیم.
در این آموزش، نحوه تبدیل `List<Integer>` به `List<String>` میپردازیم.
## آماده کردن یک شیء `List<Integer>` به عنوان مثال
برای شروع یک لیست از اعداد صحیح را مقداردهی اولیه می کنیم
{% highlight java %}
List<Integer> INTEGER_LIST = Arrays.asList(1, 2, 3, 4, 5, 6, 7);
{% endhighlight %}
همانطور که کد بالا نشان می دهد، ما هفت عدد صحیح در شی INTEGER_LIST داریم. اکنون، هدف ما این است که هر عنصر عدد صحیح در INTEGER_LIST را به یک رشته تبدیل کنیم، به عنوان مثال، 1 به "1"، 2 به "2" و غیره. در نهایت، نتیجه باید برابر با:
{% highlight java %}
List<String> EXPECTED_LIST = Arrays.asList("1", "2", "3", "4", "5", "6", "7");
{% endhighlight %}

در این آموزش، به سه روش مختلف برای انجام این کار خواهیم پرداخت:
-	با استفاده از جاوا 8، Stream API
-	استفاده از حلقه for در جاوا
-	با استفاده از کتابخانه Guava

### استفاده از استریم ها جاوا 8 متد `map()`
Java Stream API در جاوا 8 و نسخه های جدیدتر موجود است  که به ما این امکان را می‌دهد به راحتی مجموعه‌ها را به عنوان جریان مدیریت کنیم.
به عنوان مثال، یک روش معمولی برای تبدیل `List<TypeA>` به `List<TypeB>` متد ()map Stream است:
{% highlight java %}
theList.stream().map( .. the conversion logic.. ).collect(Collectors.toList());
{% endhighlight %}
در کد پایین نحوه ی تبدیل List<Integer> به List<String> مشاهده میکنید
{% highlight java %}
List<String> result = INTEGER_LIST.stream().map(i -> i.toString()).collect(Collectors.toList());
{% endhighlight %}
همانطور که مثال کد بالا نشان می‌دهد، یک عبارت لامبدا را به map() می‌فرستیم، و از روش `toString()` هر عنصر (Integer) برای تبدیل آن به یک رشته استفاده می‌کنیم.
#### تست
برای راحتی میتوانیم از unit test برای تست خروجی که با لیست مورد انتظار از صحت کد اطمینان حاصل نماییم
{% highlight java %}
assertEquals(EXPECTED_LIST, result);
{% endhighlight %}
### استفاده از حلقه for
اگر شما از نسخه ی پایین تر از 8 استفاده میکنید و به Streamها دسترسی ندارید میتوانید این تبدیل را با استفاده از یک حلقه انجام دهید
برای مثال، می‌توانیم تبدیل را از طریق یک حلقه for ساده انجام دهیم:
{% highlight java %}
List<String> result = new ArrayList<>();
 for (Integer i : INTEGER_LIST) {
 result.add(i.toString()); 
}
{% endhighlight %}
#### تست
{% highlight java %}
assertEquals(EXPECTED_LIST, result);
{% endhighlight %}
### استفاده از کتابخانه Guava
از آنجایی که هنگام کار با مجموعه‌ها، تبدیل نوع مجموعه یک عملیات کاملاً استاندارد است، برخی از کتابخانه‌های معروف روش‌های کاربردی را برای انجام این تبدیل ارائه کرده‌اند.
ابتدا کتابخانه Guava به `pom.xml` اضافه میکنیم
{% highlight java %}
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>31.1-jre</version>
</dependency>
{% endhighlight %}
در ادامه می توانیم از متد `Lists.transform()` این کتابخانه به شکل زیر استفاده نماییم
{% highlight java %}
List<String> result = Lists.transform(INTEGER_LIST, Functions.toStringFunction());
{% endhighlight %}
#### تست
{% highlight java %}
assertEquals(EXPECTED_LIST, result);
{% endhighlight %}

منبع : [Convert-a-List-of-Integers-to-a-List-of-Strings]: https://www.baeldung.com/java-convert-list-integers-to-list-strings

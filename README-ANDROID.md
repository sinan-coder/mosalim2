# مشروع تطبيق أندرويد — رئيس المرحلة الرابعة

هذا مشروع أندرويد جاهز للبناء. التطبيق يفتح **موقعك المنشور على GitHub Pages** داخل نافذة WebView بملء الشاشة، ويدعم:
الدخول بحساب الرئيس، نوافذ التأكيد (حذف/كتم)، رفع صورة الرئيس، فتح روابط واتساب وتيليجرام وإنستغرام في تطبيقاتها، وصفحة «لا يوجد اتصال» مع زر إعادة المحاولة.

> أي تحديث تنشره على الموقع **يظهر في التطبيق تلقائياً** بدون إعادة بنائه.

---

## الخطوة 0 (إجبارية): ضع رابط موقعك

افتح الملف:

`app/src/main/java/com/mohammedsalim/class4/MainActivity.java`

وغيّر هذا السطر إلى رابط موقعك (مع `https://` والشرطة المائلة في النهاية):

```java
private static final String SITE_URL = "https://USERNAME.github.io/president-site/";
```

> يجب أن يكون الموقع منشوراً ويعمل في المتصفح أولاً (انظر دليل الموقع).

---

## الطريقة الأولى: بناء الـAPK على GitHub بدون تثبيت أي برنامج (الأسهل)

1. أنشئ مستودعاً جديداً على GitHub (مثلاً `president-app`).
2. ارفع **كل محتويات هذا المجلد** إليه، بما فيها مجلد `.github` المخفي.
   - من المتصفح: *uploading an existing file* ثم اسحب الملفات. إن لم يظهر المجلد المخفي في نظامك فاستخدم الأوامر:
   ```bash
   cd PresidentApp
   git init
   git add .
   git commit -m "app"
   git branch -M main
   git remote add origin https://github.com/USERNAME/president-app.git
   git push -u origin main
   ```
3. افتح تبويب **Actions** في المستودع. سيبدأ البناء تلقائياً (أو اضغط **Build APK ← Run workflow**).
4. انتظر 3 إلى 6 دقائق حتى تظهر علامة ✅.
5. افتح عملية البناء الناجحة، ومن الأسفل في **Artifacts** حمّل **PresidentApp-APK**.
6. فك الضغط، وستجد `app-debug.apk`. انقله إلى هاتف أندرويد وافتحه، وفعّل **التثبيت من مصادر غير معروفة** عند الطلب.

---

## الطريقة الثانية: Android Studio

1. ثبّت **Android Studio** من <https://developer.android.com/studio>.
2. **File ← Open** واختر مجلد `PresidentApp`، وانتظر انتهاء *Gradle sync* (يحمّل ملفات كثيرة أول مرة).
3. **Build ← Build Bundle(s) / APK(s) ← Build APK(s)**.
4. اضغط **locate** في الإشعار أسفل الشاشة لتجد `app-debug.apk` في:
   `app/build/outputs/apk/debug/`

---

## نسخة Release للنشر على Google Play (اختياري)

ملف الـdebug يصلح للتثبيت المباشر على الأجهزة. للنشر على المتجر تحتاج نسخة موقّعة:
- Android Studio: **Build ← Generate Signed Bundle / APK** وأنشئ مفتاحاً جديداً.
- ⚠️ **احتفظ بملف المفتاح وكلمته** في مكان آمن؛ بدونه لا يمكنك تحديث التطبيق لاحقاً.

## تخصيص التطبيق

| ما تريد تغييره | أين |
|---|---|
| اسم التطبيق | `app/src/main/res/values/strings.xml` |
| لون شريط الحالة | `app/src/main/res/values/styles.xml` |
| أيقونة التطبيق | استبدل `ic_launcher.png` داخل مجلدات `app/src/main/res/mipmap-*` |
| معرّف التطبيق (Package) | `applicationId` و`namespace` في `app/build.gradle` (وغيّر مجلد وسطر `package` في `MainActivity.java` بما يطابقه) |
| رقم الإصدار | `versionCode` و`versionName` في `app/build.gradle` |

## حل المشاكل

| المشكلة | الحل |
|---|---|
| يظهر «لا يوجد اتصال» رغم وجود إنترنت | الرابط في `SITE_URL` خاطئ أو الموقع غير منشور. افتحه في متصفح الهاتف للتأكد |
| لا يعمل دخول الرئيس داخل التطبيق | تأكد أن نطاق موقعك مضاف في Firebase ← Authentication ← Authorized domains |
| فشل البناء على GitHub Actions | افتح الخطوة الحمراء واقرأ الخطأ، وتأكد أن مجلد `.github/workflows` رُفع كاملاً |
| `SDK location not found` في Android Studio | اتركه يحمّل الـSDK من نافذة الإشعار، أو ثبّته من **Tools ← SDK Manager** |
| الهاتف يرفض تثبيت الـAPK | فعّل «التثبيت من مصادر غير معروفة» للتطبيق الذي فتحت منه الملف |

## هيكل المشروع
```
PresidentApp/
├─ .github/workflows/build-apk.yml   ← يبني الـAPK على GitHub
├─ settings.gradle  build.gradle  gradle.properties
├─ gradle/wrapper/gradle-wrapper.properties
└─ app/
   ├─ build.gradle
   └─ src/main/
      ├─ AndroidManifest.xml
      ├─ java/com/mohammedsalim/class4/MainActivity.java   ← الكود الرئيسي (+ رابط الموقع)
      ├─ assets/offline.html                               ← صفحة لا يوجد اتصال
      └─ res/ (أيقونات + اسم التطبيق + الألوان)
```

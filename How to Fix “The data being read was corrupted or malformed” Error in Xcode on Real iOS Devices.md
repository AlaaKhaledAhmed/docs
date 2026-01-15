# مشكلة: The data being read was corrupted or malformed في Xcode

## وصف المشكلة

تظهر هذه المشكلة عند محاولة تشغيل التطبيق على **جهاز iPhone حقيقي** باستخدام Xcode، بينما يعمل التطبيق بشكل طبيعي على **Simulator**.
تظهر الرسالة غالبًا داخل **Devices and Simulators** أو أثناء Run المشروع.

---

## سبب المشكلة

السبب الأكثر شيوعًا هو وجود **تلف أو نقص في ملفات دعم الجهاز (iOS DeviceSupport)** أو ملفات الكاش الخاصة بـ Xcode، ويحدث ذلك عادةً بعد أحد الأمور التالية:

* تحديث iOS على الجهاز
* فشل عملية تحضير سابقة للجهاز (Previous preparation)
* اختلاف إصدارات iOS بين أجهزة متعددة تم توصيلها سابقًا
* انقطاع الاتصال أثناء ربط الجهاز مع Xcode
* مشاكل Pairing (Trust) أو Developer Mode

هذه المشكلة **لا تكون من كود التطبيق**.

---

## الحل المتبع (بالترتيب)

> **مهم:** نفّذ الخطوات بالترتيب دون تخطي أي خطوة

### 1) فصل الجهاز وإغلاق Xcode

* افصل iPhone من الكابل
* أغلق Xcode بالكامل (Quit)

---

### 2) حذف ملفات دعم الجهاز (DeviceSupport)

افتح Terminal ونفّذ الأمر التالي:

```bash
rm -rf ~/Library/Developer/Xcode/iOS\ DeviceSupport
```

---

### 3) حذف كاش البناء (DerivedData)

```bash
rm -rf ~/Library/Developer/Xcode/DerivedData
```

---

### 4) إعادة تشغيل الأجهزة

* أعد تشغيل iPhone
* (مستحسن) أعد تشغيل Mac

---

### 5) إعادة توصيل الجهاز مع Trust

* استخدم كابل USB ثابت أو أصلي
* وصّل iPhone مباشرة بالماك (بدون Hub)
* عند ظهور رسالة **Trust This Computer** اختر **Trust**

---

### 6) التأكد من تفعيل Developer Mode

على iPhone:

Settings > Privacy & Security > Developer Mode > ON
ثم أعد تشغيل الجهاز إذا طُلب ذلك.

---

### 7) إعادة تجهيز الجهاز في Xcode

* افتح Xcode
* من القائمة: **Window > Devices and Simulators**
* اختر جهاز iPhone
* انتظر حتى ينتهي "Preparing…" إن ظهر

---

### 8) تشغيل المشروع من جديد

* شغّل التطبيق على الجهاز الحقيقي
* يجب أن تختفي رسالة الخطأ

---

## في حال استمرار المشكلة (خطة بديلة)

### إعادة ضبط الثقة (Pairing Reset)

على iPhone:

Settings > General > Transfer or Reset iPhone > Reset > Reset Location & Privacy

ثم:

* افصل ووصّل الجهاز
* وافق Trust من جديد
* أعد تشغيل المشروع

---

## ملاحظات مهمة

* Xcode 16.2 يدعم iOS 18 بشكل طبيعي
* ظهور الخطأ يعني وجود بيانات دعم تالفة وليس مشكلة في Flutter أو الكود
* حذف DeviceSupport آمن، وسيُعاد إنشاؤه تلقائيًا من Xcode

---

## التحقق النهائي (اختياري)

بعد نجاح الربط، يمكن التأكد من إنشاء ملفات الدعم عبر:

```bash
ls -la ~/Library/Developer/Xcode/iOS\ DeviceSupport
```

يجب أن يظهر مجلد خاص بإصدار iOS الحالي (مثل 18.5 أو 18.x).

---

✅ **تم حل المشكلة بنجاح عند اتباع الخطوات أعلاه بالترتيب**

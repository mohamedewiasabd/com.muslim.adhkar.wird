# com.muslim.adhkar.wird — Flatpak (Flathub) packaging config

مستودع تهيئة نشر تطبيق **«أذكار المسلم - الورد اليومي»** على [Flathub](https://flathub.org)،
مبنيّاً على حزمة **AppImage** الرسمية المنشورة من مستودع التطبيق:
[`mohamedewiasabd/adhkar-al-muslim`](https://github.com/mohamedewiasabd/adhkar-al-muslim).

## المحتويات

| الملف | الغرض |
| --- | --- |
| `com.muslim.adhkar.wird.json` | ملف Manifest بصيغة Flatpak (مصدر البناء) مع تحديث تلقائي للإصدار عبر Flathub external-data-checker |
| `com.muslim.adhkar.wird.metainfo.xml` | بيانات AppStream (الوصف، اللقطات، التقييم OARS، سجل النشر) |
| `com.muslim.adhkar.wird.desktop` | إدخال سطح المكتب (icon/launchable) |
| `icons/` | أيقونات التطبيق 128 و 512 |

## طريقة النشر (حساب المطوّر)

1. التأكد أن هذا المستودع **عام** وأن مستودع التطبيق فيه إصدار GitHub Release باسم `v{VERSION}`
   يضم ملف `adhkar-al-muslim_{VERSION}_amd64.AppImage`.
2. تسجيل الدخول على [flathub.org/apps/add](https://flathub.org/apps/add) بحساب GitHub الموصول
   وإدخال رابط هذا المستودع — سيكتشف Flathub ملف `com.muslim.adhkar.wird.json` تلقائياً.
3. سيُبنى التطبيق على خوادم Flathub ويُعرض للـ QA (مراجعة الجودة) قبل الظهور في المتجر.
   إن طُلب تحديث `runtime-version` أو تقييد `finish-args`: بعدّل في هذا الملف وأعد الرفع.

## البناء محلياً (اختياري)

```bash
# يتطلب flatpak + flatpak-builder
flatpak install flathub org.gnome.Platform//48 org.gnome.Sdk//48
flatpak-builder --repo=repo build com.muslim.adhkar.wird.json
flatpak build-bundle repo adhkar-muslim.flatpak com.muslim.adhkar.wird
```

## ملاحظات

- التطبيق يُبنى في الأصل بـ **Tauri 2** (Rust + WebKitGTK عبر `org.gnome.Platform`).
- النسخة الحالية `x86_64` فقط؛ دعم `aarch64` يأتي بإنتاج AppImage للينكس ARM في سطر العمل `desktop.yml`.
- قيمة `sha256` في manifest تُحدَّث تلقائياً بواسطة أداة التحقق الخارجي من Sugالتوزيعات (flathub/flatpak-external-data-checker) عند كل إصدار.
# 🚀 خطوات رفع المشروع على GitHub

## الخطوة 1️⃣: إنشاء Repository على GitHub

1. اذهب إلى [GitHub.com](https://github.com)
2. سجل الدخول بحسابك
3. اضغط على **+** الزاوية العلوية اليمنى → **New repository**
4. ملء البيانات:
   - **Repository name**: `JupyterProject1` (أو أي اسم تفضله)
   - **Description**: Energy Consumption Prediction - Multiple Linear Regression Analysis
   - **Public** أو **Private** (حسب رغبتك)
   - ❌ **لا** تختر "Initialize this repository with a README" (عندك واحد بالفعل)
5. اضغط **Create repository**

---

## الخطوة 2️⃣: إعداد Git محلياً

افتح PowerShell أو Command Prompt وانتقل إلى مجلد المشروع:

```powershell
cd C:\Users\ACC\PycharmProjects\JupyterProject1
```

---

## الخطوة 3️⃣: تهيئة Git Repository

```powershell
git init
```

---

## الخطوة 4️⃣: إضافة جميع الملفات

```powershell
git add .
```

---

## الخطوة 5️⃣: عمل Commit الأول

```powershell
git commit -m "Initial commit: Energy consumption prediction with linear regression"
```

---

## الخطوة 6️⃣: إضافة Remote Repository

استبدل `your-username` باسم مستخدمك على GitHub:

```powershell
git branch -M main
git remote add origin https://github.com/your-username/JupyterProject1.git
```

---

## الخطوة 7️⃣: رفع الكود (Push)

```powershell
git push -u origin main
```

سيطلب منك معلومات تسجيل الدخول GitHub.

---

## ✅ تم!

الآن المشروع موجود على GitHub! 🎉

---

## 📝 ملاحظات مهمة

### إذا واجهت مشاكل بـ Authentication:

**الطريقة 1: استخدام Personal Access Token (موصى به)**
1. اذهب إلى GitHub Settings → Developer settings → Personal access tokens
2. اضغط Generate new token
3. اختر `repo` scope
4. انسخ التوكن
5. عند طلب كلمة المرور، استخدم التوكن

**الطريقة 2: إعداد SSH (متقدم)**
```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

---

## 🔄 تحديثات مستقبلية

بعد التعديل على الملفات، استخدم:

```powershell
git add .
git commit -m "وصف التعديلات"
git push
```

---

## 📚 أوامر Git مفيدة

```powershell
# عرض حالة المشروع
git status

# عرض السجل
git log --oneline

# التراجع عن آخر commit (قبل push)
git reset --soft HEAD~1
```

---

**هل احتجت مساعدة في أي خطوة؟** 😊


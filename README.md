# 🍲 سفره

اپ ساده‌ی ثبت وعده‌های غذایی خانواده، با همگام‌سازی لحظه‌ای بین دو گوشی.

---

## قدم ۱ — دیتابیس Supabase (یه بار، حدود ۵ دقیقه)

۱. برو به **supabase.com** و ثبت‌نام کن (رایگان).
۲. **New Project** بزن، یه اسم بده (مثلاً `sofreh`)، یه رمز دیتابیس بذار و Region نزدیک رو انتخاب کن. چند دقیقه صبر کن تا ساخته شه.
۳. از منوی چپ → **SQL Editor** → **New query** → این رو Run کن:

```sql
create table meals (
  day text primary key,
  data jsonb
);
alter table meals enable row level security;
create policy "allow all" on meals for all using (true) with check (true);
alter publication supabase_realtime add table meals;
```

۴. برو **Project Settings → API** و این دو تا رو کپی کن:
   - **Project URL**
   - کلید **anon public**

---

## قدم ۲ — گذاشتن روی گیت‌هاب

۱. روی **github.com** یه ریپو جدید بساز (مثلاً `sofreh`). می‌تونه Private باشه.
۲. این پوشه رو آپلود کن. ساده‌ترین راه از ترمینال:

```bash
cd sofreh
git init
git add .
git commit -m "اولین نسخه سفره"
git branch -M main
git remote add origin https://github.com/USERNAME/sofreh.git
git push -u origin main
```

> ⚠️ فایل `.env` خودکار آپلود نمی‌شه (توی `.gitignore` هست) — این درسته و امنه. کلیدها رو توی Vercel وارد می‌کنیم، نه گیت‌هاب.

---

## قدم ۳ — اتصال Vercel به گیت‌هاب

۱. برو **vercel.com** → **Add New → Project**.
۲. ریپو `sofreh` رو از گیت‌هاب انتخاب و **Import** کن.
۳. Vercel خودش می‌فهمه Vite هست. قبل از Deploy، بخش **Environment Variables** رو باز کن و سه تا متغیر اضافه کن:

| Name | Value |
|------|-------|
| `VITE_SUPABASE_URL` | همون Project URL از Supabase |
| `VITE_SUPABASE_KEY` | همون کلید anon public |
| `VITE_PIN` | رمز ۴ رقمی دلخواهت (مثلاً 1373) |

۴. **Deploy** بزن. چند ثانیه بعد یه لینک می‌گیری.
۵. اون لینک رو برای مامان‌بزرگ بفرست. اولین بار رمز رو وارد می‌کنه، بعد یادش می‌مونه.

---

## نکته‌ها

- **چراغ سبز** کنار اسم اپ یعنی اتصال به دیتابیس برقراره. اگه قرمز شد، اینترنت یا کلیدها رو چک کن.
- برای تست روی کامپیوتر خودت: یه کپی از `.env.example` بساز به اسم `.env`، پرش کن، بعد `npm install` و `npm run dev`.
- این اپ برای لیست غذای خونگیه. policy دیتابیس روی «همه دسترسی دارن» تنظیمه، پس اطلاعات حساس توش نذار.

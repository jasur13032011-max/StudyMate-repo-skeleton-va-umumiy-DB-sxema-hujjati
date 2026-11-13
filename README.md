Men to'g'ridan-to'g'ri sizning GitHub hisobingizga ulanib, u yerda public repository yarata olmayman, lekin loyiha uchun barcha kerakli fayllar tuzilmasini, .gitignore faylini hamda to'liq DB sxemasini o'z ichiga olgan README.md hujjatini tayyorlab beraman. Siz ushbu kodlarni kompyuteringizga saqlab, bir necha buyruq orqali GitHub'ga yuklashingiz mumkin.

1. Loyiha tuzilmasi (Monorepo Skeleton)
Kompyuteringizda studymate nomli asosiy papka oching va uning ichida quyidagi tuzilmani yarating:

Plaintext
studymate/
├── django_backend/
│   └── .gitkeep
├── frontend/
│   └── .gitkeep
├── telegram_bot/
│   └── .gitkeep
├── .gitignore
└── README.md
(Eslatma: Git bo'sh papkalarni kuzatmaydi, shuning uchun django_backend/, frontend/ va telegram_bot/ papkalari GitHub'ga yuklanishi uchun ularning ichiga bo'sh .gitkeep faylini qo'shib qo'yish kerak).

2. .gitignore fayli
Loyihaning asosiy papkasida .gitignore faylini yarating va quyidagi kodni joylashtiring:

Code snippet
# Python / Django / Telegram Bot
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
venv/
ENV/
.venv/
db.sqlite3
media/
staticfiles/

# Node.js / Frontend
node_modules/
dist/
build/
.next/
out/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment variables (Maxfiy kalitlar)
.env
.env.local
.env.*.local

# IDE va OS fayllari
.vscode/
.idea/
.DS_Store
Thumbs.db
3. README.md fayli (To'liq hujjat va DB sxemasi bilan)
Asosiy papkada README.md faylini yarating va quyidagi matnni nusxalang:

Markdown
# StudyMate 📚

**StudyMate** — talabalar va o'quvchilar uchun o'quv jarayonini boshqarish, fanlarni tartiblash hamda topshiriqlar (vazifalar) muddatini kuzatib borishga mo'ljallangan platforma. Tizim veb-ilova va Telegram bot integratsiyasi orqali ishlaydi.

## 📁 Loyiha tuzilmasi (Monorepo)

Loyiha uchta asosiy qismdan iborat monorepo shaklida tashkil etilgan:

* `django_backend/` — Django va Django REST Framework asosida qurilgan asosiy API server va ma'lumotlar bazasi mantiqi.
* `frontend/` — Foydalanuvchilar uchun veb-interfeys (React / Vue / Next.js).
* `telegram_bot/` — Eslatmalar yuborish va tezkor boshqaruv uchun Telegram bot (Aiogram / PyTelegramBotAPI).

---

## 🗄 Ma'lumotlar bazasi sxemasi (DB Schema)

Tizimning asosiy ma'lumotlar bazasi uchta bog'langan jadvaldan iborat: `users`, `fanlar` (subjects) va `topshiriqlar` (tasks).

### 1. `users` (Foydalanuvchilar jadvali)
Tizimdan foydalanuvchi talabalar ma'lumotlarini saqlaydi.

| Maydon nomi | Turi | Cheklovlar | Tavsif |
| :--- | :--- | :--- | :--- |
| `id` | `BIGINT / UUID` | `PRIMARY KEY` | Foydalanuvchining unikal identifikatori |
| `username` | `VARCHAR(150)` | `UNIQUE, NOT NULL` | Tizimga kirish uchun login |
| `email` | `VARCHAR(255)` | `UNIQUE, NOT NULL` | Elektron pochta manzili |
| `password` | `VARCHAR(255)` | `NOT NULL` | Xeshlangan (shifrlangan) parol |
| `full_name` | `VARCHAR(255)` | `NULL` | Foydalanuvchining to'liq ism-sharifi |
| `telegram_chat_id`| `BIGINT` | `UNIQUE, NULL` | Foydalanuvchining Telegram chat ID raqami |
| `link_kodi` | `VARCHAR(32)` | `UNIQUE, NULL` | Veb-akkauntni Telegram botga ulash uchun bir martalik kod |
| `created_at` | `TIMESTAMP` | `DEFAULT NOW()` | Ro'yxatdan o'tgan vaqt |

#### 🔑 `telegram_chat_id` va `link_kodi` maydonlari nima uchun kerak?
* **`link_kodi` (Bog'lash kodi):** Foydalanuvchi veb-saytda ro'yxatdan o'tgandan so'ng, o'z profilini Telegram bot bilan bog'lashi kerak bo'ladi. Tizim foydalanuvchi uchun unikal va vaqtinchalik `link_kodi` generatsiya qiladi (masalan, `SM-84920`). Foydalanuvchi ushbu kodni Telegram botga yuborish orqali o'z shaxsini tasdiqlaydi.
* **`telegram_chat_id`:** Foydalanuvchi botga to'g'ri `link_kodi` ni yuborganida, bot uning Telegram'dagi unikal `chat_id` raqamini ushlab oladi va bazadagi aynan shu foydalanuvchi qatoriga saqlaydi. Keyinchalik tizim topshiriq muddati (deadline) yaqinlashganda, aynan qaysi foydalanuvchiga xabar (bildirishnoma) yuborish kerakligini shu `telegram_chat_id` orqali bilib oladi.

---

### 2. `fanlar` (Fanlar jadvali)
Har bir foydalanuvchining o'zlashtirayotgan fanlari ro'yxati.

| Maydon nomi | Turi | Cheklovlar | Tavsif |
| :--- | :--- | :--- | :--- |
| `id` | `BIGINT` | `PRIMARY KEY` | Fanning unikal identifikatori |
| `user_id` | `BIGINT` | `FOREIGN KEY (users.id)` | Fanni yaratgan foydalanuvchi (1:N aloqa) |
| `nomi` | `VARCHAR(100)` | `NOT NULL` | Fanning nomi (masalan, *Ma'lumotlar bazasi*) |
| `oqituvchi` | `VARCHAR(150)` | `NULL` | Fan o'qituvchisining ismi |
| `rang_kodi` | `VARCHAR(7)` | `DEFAULT '#3B82F6'` | Interfeysda ajratib ko'rsatish uchun HEX rang |
| `created_at` | `TIMESTAMP` | `DEFAULT NOW()` | Yaratilgan vaqt |

---

### 3. `topshiriqlar` (Topshiriqlar jadvali)
Fanlar kesimida berilgan uy vazifalari, mustaqil ishlar va imtihonlar.

| Maydon nomi | Turi | Cheklovlar | Tavsif |
| :--- | :--- | :--- | :--- |
| `id` | `BIGINT` | `PRIMARY KEY` | Topshiriqning unikal identifikatori |
| `fan_id` | `BIGINT` | `FOREIGN KEY (fanlar.id)` | Qaysi fanga tegishli ekanligi (1:N aloqa) |
| `sarlavha` | `VARCHAR(255)` | `NOT NULL` | Topshiriq nomi (masalan, *1-laboratoriya ishi*) |
| `tavsif` | `TEXT` | `NULL` | Topshiriq shartlari va qo'shimcha izohlar |
| `deadline` | `TIMESTAMP` | `NOT NULL` | Topshirishning oxirgi muddati |
| `status` | `VARCHAR(20)` | `DEFAULT 'jarayonda'` | Holati: `kutilmoqda`, `jarayonda`, `bajarildi` |
| `eslatma_yuborildi`| `BOOLEAN` | `DEFAULT FALSE` | Telegram bot orqali eslatma yuborilganligini belgilash |
| `created_at` | `TIMESTAMP` | `DEFAULT NOW()` | Yaratilgan vaqt |

---

### 🔗 Jadvallararo bog'liqlik (ER Diagramma mantiqi)

```text
[ users ] 1 ────────< * [ fanlar ] 1 ────────< * [ topshiriqlar ]
Bitta user ko'plab fanlar yaratishi mumkin (One-to-Many).

Bitta fan ichida ko'plab topshiriqlar bo'lishi mumkin (One-to-Many).

Agar foydalanuvchi o'chirilsa (ON DELETE CASCADE), unga tegishli barcha fanlar va topshiriqlar ham o'chiriladi.

🚀 Loyihani ishga tushirish (Tez orada)
Backend: cd django_backend -> virtual muhit yaratish va bog'liqliklarni o'rnatish.

Frontend: cd frontend -> npm install va npm run dev.

Telegram Bot: cd telegram_bot -> .env fayliga bot tokenini kiritish va ishga tushirish.


---

### 4. GitHub'ga yuklash bo'yicha qadamlar

Kompyuteringiz terminalida (yoki Git Bash'da) quyidagi buyruqlarni ketma-ket bajaring:

1. **GitHub'da yangi repository yarating:**
   * GitHub.com saytiga kiring va **New repository** tugmasini bosing.
   * Repository nomini `studymate` deb kiriting, **Public** holatini tanlang va **Create repository** tugmasini bosing (README yoki .gitignore qo'shmasdan yarating, chunki ularni o'zimiz tayyorladik).

2. **Lokal papkani GitHub'ga ulang va yuklang:**
   ```bash
   # Asosiy papkaga kiring
   cd studymate

   # Git repozitoriyni ishga tushiring
   git init

   # Barcha fayllarni qo'shing
   git add .

   # Birinchi commitni yozing
   git commit -m "Initial commit: StudyMate repo skeleton va DB sxemasi"

   # Asosiy branch nomini main qiling
   git branch -M main

   # GitHub repozitoriy manzilini ulang (USERNAME o'rniga o'z loginingizni yozing)
   git remote add origin https://github.com/USERNAME/studymate.git

   # Fayllarni GitHub'ga yuklang
   git push -u origin main
Keyingi qadam sifatida django_backend ichida Django loyihasi va modellarini (models.py) generatsiya qilib berishimni xohlaysizmi?

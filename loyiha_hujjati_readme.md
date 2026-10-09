# DedupePro — Dublikat Yozuvlarni Qidirish va Tozalash Tizimi

[![Pure JavaScript](https://img.shields.io/badge/Vanilla-JavaScript-f7df1e?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-0%20(None)-success)](#)
[![Responsive Design](https://img.shields.io/badge/Responsive-Mobile%20%26%20Desktop-blue)](#)
[![Algorithm Complexity](https://img.shields.io/badge/Algorithm-O(N)%20Hash%20Map-purple)](#algoritmik-samadorlik)
[![Vazifa](https://img.shields.io/badge/Vazifa%2021-Chipta%20045-orange)](#)

**DedupePro** — bir xil kodga ega takroriy yozuvlarni (dublikatlarni) tezkor va aniq aniqlash, vizual ajratish, tartibni buzmagan holda dastlabki asl nusxani saqlab tozalash uchun mo'ljallangan yengil va professional veb-ilova.

Ilova hech qanday tashqi kutubxona, framework (React, Vue, Tailwind) yoki npm paketlarisiz, toza **HTML5**, **CSS3** va **Vanilla JavaScript** yordamida yaratilgan. Faqatgina `index.html` faylini istalgan brauzerda ochish orqali to'liq offline rejimda ishlaydi.

---

## Mundarija
- [Majburiy Funksional Talablar](#majburiy-funksional-talablar)
- [Asosiy Imkoniyatlar va UX Qulayliklari](#asosiy-imkoniyatlar-va-ux-qulayliklari)
- [Algoritmik Samadorlik](#algoritmik-samadorlik)
- [Ommaviy Matn Import Formati](#ommaviy-matn-import-formati)
- [Loyiha Tuzilmasi va Ishga Tushirish](#loyiha-tuzilmasi-va-ishga-tushirish)
- [Foydalanish Bo'yicha Yo'riqnoma](#foydalanish-boyicha-yoriqnoma)
- [Texnologiyalar](#texnologiyalar)

---

## Majburiy Funksional Talablar

Loyiha quyidagi 4 ta asosiy talabga 100% javob beradi:

1. **Nomlar va kodlangan namunalar ro'yxatini kiritish**:
   - Har bir yozuv `Nom` + `Kod` juftligidan iborat.
   - 3 xil kiritish usuli mavjud:
     - *(a)* Alohida forma orqali yakka yozuv qo'shish;
     - *(b)* Ko'p qatorli matn maydoni (textarea) orqali ommaviy import;
     - *(c)* Tayyor laboratoriya namunalarini yuklash tugmasi (bunda ataylab bir nechta dublikat kodlar mavjud).
   - Maydonlar bo'sh qolmasligi tekshiriladi.
   - Ommaviy importda xato qatorlar qator raqami va sababi bilan ko'rsatiladi, to'g'ri qatorlar esa baribir saqlanadi.
   - Nom va kod atrofidagi ortiqcha bo'shliqlar (`trim()`) avtomatik tozalanadi.

2. **Bir xil kodlarni aniqlash**:
   - Kodlar simvolma-simvol, katta-kichik harflarga qat'iy sezgir (case-sensitive) holda solishtiriladi (masalan, `VIR-01` va `vir-01` turli kod hisoblanadi).
   - Dublikatlarni qidirish logikasi DOM bilan bog'lanmagan sof (pure) funksiya sifatida `Map` yordamida chiziqli vaqt ichida — $O(N)$ da bajariladi.

3. **Dublikatlarni vizual ajratish**:
   - Barcha bir xil kodga ega yozuvlar yagona rang guruhi bilan belgilanadi (Dark va Light mavzulari uchun moslashtirilgan 8 xil pastel rang palitrasi).
   - Har bir dublikat qatorda nafaqat rang, balki matnli belgilar ham mavjud:
     - `★ Asl nusxa (1/N)` — guruhdagi birinchi kiritilgan yozuv;
     - `⚠ Dublikat (×N)` — keyingi nusxalar va guruh umumiy soni;
     - `✓ Unikal` — takrorlanmagan alohida yozuvlar.
   - 4 ta dinamik animatsiyali statistika kartasi: *Jami yozuvlar*, *Noyob kodlar*, *Dublikat guruhlar*, *O'chiriladigan nusxalar*.
   - Filtrlash tugmalari: `Barchasi` / `Faqat Dublikatlar` / `Faqat Unikallar`.

4. **Tozalash (birinchi nusxani saqlash) va saqlash**:
   - Maxsus **"Tozalash"** tugmasi: har bir dublikat guruhidan faqat eng birinchi kiritilgan yozuv saqlanadi, qolgan nusxalar o'chiriladi.
   - Dastlabki ro'yxat tartibi buzilmaydi.
   - Standart `window.confirm` o'rniga, nechta yozuv o'chirilishi va qaysi kodlar tozalanayotganini ko'rsatuvchi moslashtirilgan xavfsiz modal oyna chiqariladi.

---

## Asosiy Imkoniyatlar va UX Qulayliklari

- 📱 **To'liq Moslashuvchanlik (Full Responsive)**: Smartfonlar (360px+), planshetlar va katta monitorlarga to'liq moslashadi. Mobil qurilmalarda qulay kiritish uchun alohida Segmented Tab qo'shilgan.
- 🌓 **Dark / Light Mavzular**: Foydalanuvchi tanlagan mavzu brauzer xotirasida (`localStorage`) saqlanadi.
- ⚡ **Amalni Bekor Qilish (Undo Stack)**: Tasodifan tozalangan yoki o'chirilgan ma'lumotlarni 1 bosish bilan asl holatiga qaytarish imkoniyati.
- 📥 **CSV Eksport & Clipboard**: Natijalarni Excel uchun to'liq moslashtirilgan formatda (UTF-8 BOM bilan) yuklab olish yoki clipboardga nusxalash.
- 🔍 **Tezkor Jonli Qidiruv**: Nom yoki kod bo'yicha jadvalni soniyaning ulushlarida filtrlash.
- 🎉 **Canvas Konfetti Effekti**: Dublikatlar muvaffaqiyatli tozalanganda chiroyli vizual bayramona animatsiya.

---

## Algoritmik Samadorlik

Dublikatlarni aniqlash tizimi `detectDuplicates(records)` sof (pure) funksiyasiga tayanadi:

$$O(N) \text{ — Vaqt bo'yicha murakkablik}$$
$$O(N) \text{ — Xotira (Space) bo'yicha murakkablik}$$

```text
[Yozuvlar Ro'yxati] 
        │
        ▼
   Iteratsiya (Map Hash Lookup: O(1)) ──► CodeOccurrencesMap: code -> [index0, index1, ...]
        │
        ▼
   Guruhlarni aniqlash (Hajmi > 1 bo'lganlar dublikat guruhga kiritiladi)
        │
        ▼
   Metadata enrichment: { isDuplicate, isFirstCopy, groupSize, colorIndex }
```

Ichma-ich sikllar ($O(N^2)$) ishlatilmaydi, shu sababli ro'yxatda minglab namunalar bo'lganda ham brauzer interfeysi qotmasdan tez ishlaydi.

---

## Ommaviy Matn Import Formati

Ommaviy kiritish (Bulk Import) quyidagi 3 xil ajratkichlarni avtomatik aniqlaydi:

| Format | Misol |
| :--- | :--- |
| **Vergul bilan (CSV)** | `Qon Plazmasi Zardobi, CHM-BLD-01` |
| **Nuqtali vergul bilan** | `COVID-19 Antigen Testi; VIR-COV19-892` |
| **Tab bilan (TSV/Excel)** | `Genomik DNK Zanjiri	GEN-DNA-405` |

### Xatoliklar Bilan Ishlash Logikasi:
Agar matnda noto'g'ri qator uchrasa (masalan, ajratkich yo'q yoki kod bo'sh qolgan):
1. Tizim to'xtab qolmaydi — barcha to'g'ri qatorlarni import qiladi.
2. Noto'g'ri qatorlar esa qaysi qatorda xatolik yuz bergani va uning aniq sababi bilan ogohlantirish blokida ko'rsatiladi.

---

## Loyiha Tuzilmasi va Ishga Tushirish

Loyiha mustaqil yagona fayldan iborat:

```text
dedupe-pro/
│
├── index.html        # HTML struktura, CSS stillar va JS dastur kodi
└── README.md         # Loyiha texnik hujjati
```

### Ishga tushirish:
1. Repozitoriyani yuklab oling yoki klon qiling:
   ```bash
   git clone https://github.com/username/dedupe-pro.git
   ```
2. `index.html` faylini istalgan brauzerda (Chrome, Firefox, Safari, Edge) oching.
3. Hech qanday `npm install`, server yoki kutubxona talab qilinmaydi.

---

## Foydalanish Bo'yicha Yo'riqnoma

1. **Namunalar bilan tanishish**: Yuqori paneldagi **"Namunaviy Ma'lumotlar"** tugmasini bosing. Ro'yxatda avtomatik ravishda bir nechta dublikat kodli laboratoriya tahlillari paydo bo'ladi.
2. **Yakka yozuv qo'shish**: Chap paneldagi formaga namunaning nomi va aniq kodini kiritib, **"Yozuvni Saqlash"** tugmasini bosing.
3. **Ommaviy ma'lumot yuklash**: Matn maydoniga har bir qatorda bittadan `Nom, Kod` shaklida yozuvlarni kiriting va **"Tahlil Qilish & Import"** tugmasini bosing.
4. **Filtrlash va tahlil**: Jadval yuqorisidagi `Faqat Dublikatlar` tugmasini bosib, faqat takrorlangan namunalarni ko'ring.
5. **Dublikatlarni tozalash**: Yashil **"Tozalash"** tugmasini bosing. Modal oynada nechta nusxa o'chirilishi haqidagi hisobotni tekshirib, tozalashni tasdiqlang.
6. **Bekor qilish (Undo)**: Agar adashib tozalab yuborgan bo'lsangiz, ekranning pastki qismida chiquvchi xabardagi **"Bekor Qilish"** tugmasini bosing.

---

## Texnologiyalar

- **Markup**: Semantik HTML5 (`<header>`, `<section>`, `<table>`, `<dialog>` tamoyillari).
- **Styling**: Zamonaviy CSS3 (CSS Variables, Flexbox, Grid, Glassmorphism effektlari, Media Queries).
- **Accessibility**: WCAG talablariga mos yuqori kontrastli ranglar, ekran o'quvchilar uchun `aria-*` teglari va matnli statuslar.
- **Dasturlash tili**: Toza ECMAScript 6+ (`Map`, `Set`, `requestAnimationFrame`, `Blob API`, `HTML5 Canvas`).

---

## Litsenziya

Ushbu loyiha ochiq kodli bo'lib, o'quv va amaliy maqsadlarda erkin foydalanish uchun taqdim etiladi.
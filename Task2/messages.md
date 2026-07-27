# Bot Messages — Bright Smile Dental Clinic (Niche: Dental Clinic)

All 9 required messages, each in English and Arabic (18 versions total).
`[Service]`, `[Date]`, `[Time]`, `[Name]` are placeholders filled in at runtime.

---

## State 1 — Greeting + Intent

**EN:**
> Hi! 👋 Welcome to Bright Smile Dental Clinic. Would you like to book an appointment or do you have a question?
> 1. Book an appointment
> 2. Ask a question

**AR:**
> مرحباً! 👋 أهلاً بك في عيادة برايت سمايل لطب الأسنان. هل تودّ حجز موعد أم لديك سؤال؟
> 1. حجز موعد
> 2. لديّ سؤال

---

## State 2 — Service Selection

**EN:**
> Great! Which service are you interested in?
> 1. Check-up & Cleaning
> 2. Teeth Whitening
> 3. Braces Consultation
> 4. Emergency / Toothache

**AR:**
> رائع! ما هي الخدمة التي تهمّك؟
> 1. فحص وتنظيف
> 2. تبييض الأسنان
> 3. استشارة تقويم الأسنان
> 4. حالة طارئة / ألم في الأسنان

---

## State 3 — Timing

**EN:**
> When would you like to come in? You can name a day and time (e.g. "Thursday afternoon") or just tell me what works best for you.

**AR:**
> متى يناسبك الحضور؟ يمكنك ذكر اليوم والوقت (مثال: "الخميس بعد الظهر") أو إخبارنا بما يناسبك.

---

## State 4 — Offer Slots (mocked calendar)

**EN:**
> Here are the available slots for [Service]:
> 1. Thursday, 4:00 PM
> 2. Thursday, 6:00 PM
> 3. Friday, 11:00 AM
> Which one works for you?

**AR:**
> إليك المواعيد المتاحة لخدمة [الخدمة]:
> 1. الخميس، الساعة 4:00 مساءً
> 2. الخميس، الساعة 6:00 مساءً
> 3. الجمعة، الساعة 11:00 صباحاً
> أيّها يناسبك؟

---

## State 5 — Confirm Booking

**EN:**
> You're all set! ✅
> Service: [Service]
> Date: [Date]
> Time: [Time]
> We'll send you a reminder before your visit. See you soon at Bright Smile Dental Clinic!

**AR:**
> تم تأكيد موعدك! ✅
> الخدمة: [الخدمة]
> التاريخ: [التاريخ]
> الوقت: [الوقت]
> سنرسل لك تذكيراً قبل الموعد. نراك قريباً في عيادة برايت سمايل لطب الأسنان!

---

## Nudge 1 — +1 hour (free-form, still within 24h window)

**EN:**
> Hi again 👋 Just checking in — are you still there? I can help you finish booking your appointment whenever you're ready.

**AR:**
> مرحباً مجدداً 👋 أردت فقط الاطمئنان — هل ما زلت معنا؟ يسعدني مساعدتك في إتمام حجز موعدك متى ما كنت جاهزاً.

---

## Nudge 2 — +24 hours (⚠️ requires a Meta-approved template message in production)

**EN:**
> Hi [Name], we saved your appointment request at Bright Smile Dental Clinic. Reply anytime to continue booking, or let us know if you'd like to cancel.

**AR:**
> مرحباً [الاسم]، احتفظنا بطلب حجزك في عيادة برايت سمايل لطب الأسنان. راسلنا في أي وقت لإتمام الحجز، أو أخبرنا إن رغبت بالإلغاء.

---

## Nudge 3 — +72 hours, final (⚠️ requires a Meta-approved template message in production)

**EN:**
> Hi [Name], this is our final reminder about your appointment request at Bright Smile Dental Clinic. Reply within the next few hours to keep it active, otherwise we'll close this request for now.

**AR:**
> مرحباً [الاسم]، هذا تذكيرنا الأخير بخصوص طلب حجزك في عيادة برايت سمايل. راسلنا خلال الساعات القادمة للإبقاء عليه فعالاً، وإلا سنقوم بإغلاق الطلب حالياً.

---

## Human Handoff

**EN:**
> Thanks for sharing that. This needs a bit more personal attention, so I'm connecting you with our team — they'll follow up with you shortly. 🙏

**AR:**
> شكراً لمشاركتك ذلك. هذا الأمر يحتاج إلى اهتمام شخصي أكبر، لذا سأقوم بتحويلك إلى فريقنا وسيتواصلون معك قريباً. 🙏

---

## Bonus: Sample FAQ answers (used in the State 1 "Question" branch)

| Question (EN) | Answer (EN) | Question (AR) | Answer (AR) |
|---|---|---|---|
| What are your opening hours? | We're open Saturday–Thursday, 10 AM–8 PM. | ما هي ساعات العمل؟ | نعمل من السبت إلى الخميس، من 10 صباحاً حتى 8 مساءً. |
| Do you accept insurance? | Yes, we accept most major insurance providers. Our team can confirm your specific plan. | هل تقبلون التأمين الصحي؟ | نعم، نقبل معظم شركات التأمين الرئيسية. يمكن لفريقنا تأكيد تغطية خطتك تحديداً. |

## Why the handoff step matters

The bot should never improvise on medical questions, complaints, or price negotiations — those carry real
risk (health, reputation, revenue) if handled by a scripted flow. Escalating to a human keeps the bot inside
its safe lane (scheduling logistics) and keeps trust with the customer intact.

## Bilingual behavior note

Language is detected per incoming message using a simple Arabic-script check (Unicode range `\u0600-\u06FF`).
If the user switches language mid-conversation, the next reply switches with them — the bot does not lock
to the first language detected.

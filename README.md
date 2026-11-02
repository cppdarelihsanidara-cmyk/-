# موقع دار الإحسان التعريفي العام (Public Website)

الموقع الرسمي التعريفي لجمعية أولياء الأطفال المعاقين ذهنياً والمركز النفسي البيداغوجي دار الإحسان - العطف - غرداية.

## محتويات المشروع
- الواجهة التعريفية الكاملة (`index.html`).
- أقسام المراكز الثلاثة: المركز الرئيسي، المزرعة البيداغوجية، ومركز المساعدة على العمل.
- الأنشطة والأخبار، المعرض، وسائل التواصل، والتواصل المباشر.
- ميزة التحقق السريع من رمز بطاقة الطالب QR دون بيانات حساسة (`qr.html`).

## النشر على GitHub Pages
المشروع مهيأ تلقائياً للرفع والنشر عبر GitHub Pages بواسطة سير العمل `.github/workflows/deploy.yml`.


## Security remediation package (2026-11)

- Run `npm run build` to validate JavaScript syntax and local HTML assets; this is a static-site check, not a full browser/integration test.
- The repository contains `18-security-privacy-hardening-rbac-rls.sql`. Review it against the live schema, then run it once in Supabase SQL Editor as database owner. Do not run it against production until the `children.qr_token` and `children.active` column assumptions have been confirmed.
- Set `META_WA_VERIFY_TOKEN` (or the exact environment variable used by the deployed Edge Function) in Supabase Secrets to a newly generated random value. Do not put the value in SQL, Git, HTML, or `cms-config.js`. Update the Meta webhook configuration to match.
- The public QR endpoint now uses `public_child_card_safe`, returning only membership number, name, center, and academic year. Legacy QR links using `?id=` are intentionally rejected; reissue cards using the `?c=` token.
- `cms-config.js` may contain only the public Supabase URL and publishable/anon key. Never commit `service_role`, passwords, personal child records, or real test-message logs.
- If deploying these folders as separate GitHub Pages repositories, replace cross-folder links such as `../01_Parents_Portal/parent.html` with the final HTTPS URL of the parents portal before deployment. They work as relative links only when the three folders are hosted together under one root.
- This package cannot apply migrations to your live Supabase project or deploy to GitHub without connected account access. After migration, run the role-isolation tests documented in `SECURITY-TEST-PLAN.md`.

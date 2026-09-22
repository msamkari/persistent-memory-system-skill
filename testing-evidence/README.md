# Testing Evidence

كل الأدلة الخام (raw) من اختبار الـSkill، مضغوطة zip لتسهيل الرفع والتنظيم. كل ملف يحوي مجلدات المحاكاة الأصلية (sim-storage)، النصوص الكاملة (transcripts)، وملفات التقييم (grading.json/timing.json) لكل سيناريو — نفس البيانات اللي بُنيت منها عارضات HTML بمجلد `reports/`.

## v1

- `v1-iteration-1-full-results.zip` — الاختبار الأول الكامل: 5 سيناريوهات × (with_skill + without_skill)، benchmark.json/md. النتيجة: 100% مقابل 50.3%.
- `v1-targeted-retest-iteration-2.zip` — إعادة اختبار مستهدفة (سيناريو 1 بس، with_skill) بعد إصلاح مشكلة ترقيم الملفات المكتشفة بمراجعة النتائج — تحقق مباشر من نجاح الإصلاح.

## v2

- `v2-description-optimization-full.zip` — كل محاولات تحسين وصف التفعيل: مجموعة الاستعلامات العشرين (`trigger_eval_set.json`)، سجلات محاولة الأداة الآلية run_loop.py (`loop_stdout.log`, `loop_stderr.log`, وملفات debug الخام)، ونتائج التكرارات الأربعة (`results/*/logs/improve_iter_*.json`) اللي أظهرت recall=0% ثابت — الدليل الخام على القيد البيئي الموثَّق بتقرير v2 (الأداة تسجّل الملف كـslash command مو كـskill حقيقي بهذي النسخة من Claude Code). نسخة من الـSkill قبل أي تعديل موجودة أيضاً (`skill-snapshot-before-optim/`).
- `v2-expanded-eval-iteration-3.zip` — الـ15 سيناريو الجديد (9 تدريب + 6 اختبار مخفي)، بالإضافة لملفات fixtures التأسيسية، وbenchmark.json/md المجمَّع. **لا يحوي** السيناريوهات 16-19 (كانت روابط رمزية/symlinks لمجلدي task3-special-cases وtask4-dashboard بالأسفل، تجنباً لتكرار نفس البيانات مرتين).
- `v2-task3-special-cases.zip` — الحالات الثلاث الخاصة (Task 3): محاكاة Notion، محاكاة Microsoft 365 العامة، تدقيق مجلد فوضوي حقيقي (بما فيه ملف بيانات دخول وهمي كاختبار أمان — الـSkill قرأه بدون نسخه لأي مكان ثاني).
- `v2-task4-dashboard.zip` — اختبار Dashboard المركزي: أربع مشاريع وهمية معزولة، كل وحدة بملف 01 خاص، وتحقق من ملخص الحالة الموحَّد.
- `v2-fixtures-and-scripts.zip` — ملفات fixtures الإضافية المستخدمة بسيناريوهات متعددة، وسكربت `gen_grading.py` اللي وُلِّدت فيه ملفات grading.json/timing.json من التحقق المباشر بالقرص.

## ملاحظة عن الأداة الآلية run_eval.py/run_loop.py

نسخة `skill-creator` المستخدمة هنا مأخوذة من بيئة الاختبار كما هي، بدون تعديل جوهري إلا تصحيح خطأ واحد صغير بمنطق `run_single_query` (كان يتوقف عند أول استدعاء أداة غير "Skill"/"Read" بدل الانتظار لنهاية التشغيلة). القيد الأساسي (تسجيل الملف كـslash command) بقي قائماً حتى بعد هذا التصحيح، وهو سبب اعتماد بديل يدوي (حكم Subagents) بدل الاعتماد على نتيجة الأداة الآلية.

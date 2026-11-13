# StudyMate-repo-skeleton-va-umumiy-DB-sxema-hujjati
# 1. Loyiha papkasida turganingizga ishonch hosil qiling (cd studymate)
cd C:\Users\Admin\studymate

# 2. Git repozitoriyni ishga tushiramiz
git init

# 3. Barcha fayllarni indeksga qo'shamiz
git add .

# 4. Birinchi commitni yozamiz
git commit -m "Initial commit: StudyMate repo skeleton, gitignore va DB sxemasi bilan README"

# 5. Asosiy tarmoq nomini main qilamiz
git branch -M main

# 6. GitHub'dagi repository manzilingizni ulaymiz (USERNAME o'rniga o'z loginigizni yozing!)
git remote add origin https://github.com/USERNAME/studymate.git

# 7. Kodlarni GitHub'ga yuklaymiz (push qilamiz)
git push -u origin main

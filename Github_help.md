# Github help [![GitHub](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)](https://github.com/TheTecTeam/)


---

## Remote

**Findes der en forbindelse til dit repository på github?**
```bash
git remote -v
```

**Du skal se noget i denne stil:**
```bash
origin  https://github.com/TheTecTeam/logotest.git (fetch)
origin  https://github.com/TheTecTeam/logotest.git (push)
```

**Tilføj det, hvis det ikke findes. Fx:**
```bash
git remote add origin https://github.com/TheTecTeam/logotest.git
```

---

## Ny branch
**Gå til dit lokale repository**
```bash
cd C:\Users\km\source\repos\TheTecTeam\logotest
```

**Check eksisterende branch**
```bash
git branch
```

**Opret og skift til den nye branch**
```bash
git checkout -b dev
```

**Tilføj ændringer til staging**
```bash
git add .
```

**Commit dine ændringer**
```bash
git commit -m "Start dev branch med initial changes"
```

**Push din branch til GitHub og opsæt tracking**
```bash
git push --set-upstream origin dev
```

**Når tracking er sat op, kan du blot bruge:**
```bash
git push
```

**Skift mellem branches lokalt**
```bash
git checkout main      # tilbage til master
git checkout dev       # tilbage til dev
```

---

## Tags

Tags bruges oftest til release og versions styring. 

**Indsæt ny lightweight tag**
```bash
git tag v1.0
```
**Annotated tag (anbefalet til releases)**
```bash
git tag -a v1.0 -m "Første officielle release"
```

**Tag den ønskede commit: (Se under commits for at finde commit-id)**
```bash
git tag -a v1.1 <commit-id> -m "Bugfix i index.html"
```

**Push et enkelt tag:**
```bash
git push origin v1.0
```

**Push alle tags:**
```bash
git push --tags
```

---
## Commit

**Er der filændringer, så skal du bruge add**
```bash
git add .
```

**Commit med sigende kommentar**
```bash
 git commit -m "added help"
```

**Upload dine ændringer til github**
```bash
git push
```
---

**Se commits med id - kan bruges til at tagge individuelle commits**
```bash
git log --oneline
```
---

## Merge din branch med master
Fx hedder din branch **dev**. Du har lavet nogle ændringer, og nu vil du gerne merge den med master.

**Hvilken branch er du i?**
```bash
git status
```

**Skift branch til master**
```bash
git checkout master
```

**Hent master, så du er sikker på at have den nyeste version**
```bash
git pull origin master
```

**merge dine ændringer i dev ind i master**
```bash
git merge dev
```


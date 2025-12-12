# Git CLI Cheat-Sheet

## 1. Status & info

```bash
git status             # Se ændrede filer
git log                # Se commit-historik
git log --oneline      # Kort commit-historik
git branch             # Se aktuelle branch
```

## 2. Arbejde med filer

```bash
git add <fil>          # Tilføj en fil til staging
git add .              # Tilføj alle ændringer
git reset <fil>        # Fjern fil fra staging
```

## 3. Commit

```bash
git commit -m "Beskrivelse"   # Commit med besked
git commit -am "Beskrivelse" # Commit alle ændringer direkte
```

## 4. Branching

```bash
git branch <branch-navn>        # Opret ny branch
git checkout <branch-navn>      # Skift branch
git checkout -b <branch-navn>   # Opret og skift branch
```

## 5. Push & pull

```bash
git push origin <branch-navn>   # Send commits til remote
git pull origin <branch-navn>   # Hent ændringer fra remote
```

## 6. Andre nyttige

```bash
git diff               # Se ikke-staged ændringer
git diff --staged      # Se staged ændringer
git branch -d <branch> # Slet lokal branch
git merge <branch>     # Flet branch ind i nuværende
git fetch --all        # Hent alle branches fra remote
```

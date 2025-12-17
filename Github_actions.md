# Github Actions - Upload hjemmeside.

### Workflow:
1. Ændr kode
2. Push til GitHub
3. Angular build (dist bliver lavet)
4. Upload dist til server (fx. Simply.com) automatisk

## Forudsætninger
GitHub repository

## Der skal bruges FTP-oplysninger fra server (fx Simply.com)
- **FTP host** (fx ftp.simply.com)
- **FTP brugernavn**
- **FTP password**
- **Web root**, typisk:
```bash
/public_html
```

## Trin
### 1. Gem FTP-oplysninger som GitHub Secrets

I dit GitHub-repo:

Settings → Secrets and variables → Actions → New repository secret

Opret disse secrets:

| Name           | Value               |
| -------------- | ------------------- |
| `FTP_SERVER`   | fx `ftp.simply.com` |
| `FTP_USERNAME` | dit FTP-login       |
| `FTP_PASSWORD` | dit FTP-password    |
| `FTP_TARGET`   | `/public_html`      |


### 2. Opret GitHub Actions workflow
```
.github/
  workflows/
    deploy.yml
```
#### deploy.yml
```
name: Build & Deploy Angular to Simply

on:
  push:
    branches:
      - main   # eller master

jobs:
  build-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 20

    - name: Install dependencies
      run: npm ci

    - name: Build Angular app
      run: npm run build -- --configuration production

    - name: Deploy via FTP to Simply
      uses: SamKirkland/FTP-Deploy-Action@v4
      with:
        server: ${{ secrets.FTP_SERVER }}
        username: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
        local-dir: dist/DIT-PROJEKT-NAVN/
        server-dir: ${{ secrets.FTP_TARGET }}/
        dangerous-clean-slate: true

```

### 🔴 VIGTIGT

**Erstat:**
```
dist/DIT-PROJEKT-NAVN/
```

med det rigtige navn, fx:

```
dist/my-angular-app/
```

(Tjek dit angular.json → outputPath)

### 3: Angular routing (vigtigt på Simply)
Hvis du bruger Angular routing:
Simply (Apache) kræver .htaccess

I src/ opret:
```
.htaccess
```
Indhold:
```
RewriteEngine On
RewriteBase /
RewriteRule ^index\.html$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.html [L]

```

###
4: Push og test
```
git add .
git commit -m "Setup CI/CD deploy to Simply"
git push

```

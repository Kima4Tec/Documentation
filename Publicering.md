# Publicering af kode

## Github actions

### API
Lav en yaml fil fx. .github/workflows/deploy.yml
Filen kan se sådan ud:
```
name: Deploy .NET API

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: '8.0.x'

    - name: Publish
      run: dotnet publish -c Release -o publish

    - name: Deploy to FTP
      uses: SamKirkland/FTP-Deploy-Action@v4
      with:
        server: ${{ secrets.FTP_SERVER }}
        username: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
        local-dir: ./publish/
```
Gem FTP credentials som GitHub Secrets.

### Sådan opretter du GitHub Secrets

- Gå ind på dit repository på GitHub 
- Klik på Settings

#### I venstre menu: 
- Klik Secrets and variables
- Klik Actions
- Tryk New repository secret


| Name           | Value            |
| -------------- | ---------------- |
| `FTP_SERVER`   | ftp.ditdomæne.dk |
| `FTP_USERNAME` | ditbrugernavn    |
| `FTP_PASSWORD` | ditkodeord       |


### Angular App
```
name: Deploy Angular App via FTP

on:
  push:
    branches:
      - main  # eller master, alt efter dit repo

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    # 1️⃣ Tjek koden ud
    - uses: actions/checkout@v3

    # 2️⃣ Setup Node.js
    - uses: actions/setup-node@v3
      with:
        node-version: '20' # eller versionen du bruger

    # 3️⃣ Installer dependencies
    - run: npm install

    # 4️⃣ Build Angular til production
    - run: npm run build -- --prod
      # output ligger typisk i dist/<project-name>

    # 5️⃣ Deploy via FTP
    - name: Deploy via FTP
      uses: SamKirkland/FTP-Deploy-Action@v4
      with:
        server: ${{ secrets.FTP_SERVER }}
        username: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
        local-dir: ./dist/my-angular-app/  # <--- skift til din Angular dist mappe
        server-dir: /public_html/          # <--- hvor filerne skal ligge på serveren
```

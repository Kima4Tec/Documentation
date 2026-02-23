# Publicering af kode

## Github actions

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
- 
#### I venstre menu: 
- Klik Secrets and variables
- Klik Actions
- Tryk New repository secret


| Name           | Value            |
| -------------- | ---------------- |
| `FTP_SERVER`   | ftp.ditdomæne.dk |
| `FTP_USERNAME` | ditbrugernavn    |
| `FTP_PASSWORD` | ditkodeord       |

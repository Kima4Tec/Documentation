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

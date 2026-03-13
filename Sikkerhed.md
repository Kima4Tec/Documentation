# WordPress

Login:
 - Fælles login: Et login, som alle gæster kan logge sig på med. Simpelt og kræver ikke så meget.
 - Eksklusivt medlemskab: Hvert medlem bliver oprettet af os, men hvor de kan skifte deres password. Data skal sikres, ved brud skal det meldes til datatilsynet, og der skal være tjek på GPDR.
 - Åbent medlemskab: Alle kan melde sig til. De samme regler om data gælder.
 


Hvis du gemmer data om medlemmer, der kan logge ind på en hjemmeside i Danmark eller EU, skal du følge reglerne i **General Data Protection Regulation (GDPR)** samt den danske **Databeskyttelsesloven**.

Det gælder også selvom det “bare” er et login-system.

Her er de vigtigste regler/principper:

---

## 1. Du må kun gemme nødvendige data

Du må kun gemme de oplysninger, der er **nødvendige for formålet**.

Eksempel på typiske login-data:

* Brugernavn
* Krypteret adgangskode
* Bruger-ID (fx GUID)
* evt. e-mail
* evt. IP-adresser eller loginhistorik (hvis det bruges til sikkerhed)

Dette kaldes **dataminimering**.

---

## 2. Du skal have et lovligt grundlag

Du skal have en **grund til at gemme dataene**.

Typiske grundlag:

* **Aftale** – brugeren opretter en konto
* **Legitim interesse** – fx sikkerhedslogs
* **Samtykke** – fx nyhedsbrev

Login-systemer falder normalt under **aftale**.

---

## 3. Du skal beskytte dataene

Du skal tage **tekniske og organisatoriske sikkerhedsforanstaltninger**.

Typiske krav:

* Adgangskoder skal **hashes** (fx bcrypt / Argon2)
* HTTPS
* Beskyttelse mod brute force
* begrænset adgang til databasen
* logning af sikkerhedshændelser

Hvis en database med brugere lækker, kan det være et **databrud**.

Databrud skal anmeldes til
**Datatilsynet** inden **72 timer**.

---

## 4. Brugerne har rettigheder

Medlemmer har rettigheder til deres data:

* få indsigt i deres data
* få data rettet
* få data slettet ("retten til at blive glemt")
* få udleveret deres data
* gøre indsigelse

---

## 5. Du skal informere brugerne

Du skal have en **privatlivspolitik** på hjemmesiden.

Den skal forklare:

* hvilke data du gemmer
* hvorfor du gemmer dem
* hvor længe de gemmes
* hvem der har adgang
* hvordan man får slettet data

---

## 6. Opbevaringstid

Data må **ikke gemmes længere end nødvendigt**.

Eksempel:

* aktive brugere → ok
* inaktive brugere → bør slettes efter fx 1-2 år

---

## 7. Databehandlere

Hvis du bruger tredjepart (fx cloud):

* hosting
* analytics
* email-service

skal der være en **databehandleraftale**.

---

## 8. Cookies

Hvis login bruger cookies eller tracking, gælder også:

**ePrivacy Directive**

Derfor kræves ofte cookie-banner.

---

✅ **Kort sagt:**
Hvis du laver et login-system, skal du:

* hash adgangskoder
* bruge HTTPS
* kun gemme nødvendige data
* have en privatlivspolitik
* give brugere mulighed for at få deres data slettet
* håndtere databrud korrekt






Hvis du bruger Simply.com til din hjemmeside eller WordPress, så er **grundsikkerheden normalt ret fornuftig**, men der er stadig nogle ting, du selv skal sørge for.

Her er hvordan sikkerheden typisk er hos Simply:

---

## 1. Server-sikkerhed (hostens ansvar)

Hostingfirmaet står normalt for:

* firewall på serveren
* sikkerhedsopdateringer på serversoftware
* backup-systemer
* DDoS-beskyttelse
* SSL-certifikater (HTTPS)

Hos Simply får du typisk **gratis SSL via Let's Encrypt**, hvilket er vigtigt for login-sikkerhed.

---

## 2. WordPress-sikkerhed (dit ansvar)

Hvis du installerer **WordPress**, er det stadig dit ansvar at:

* opdatere WordPress
* opdatere plugins
* opdatere temaer
* fjerne plugins du ikke bruger

De fleste hacks sker fordi:

* gamle plugins ikke er opdateret
* svage passwords
* for mange plugins

---

## 3. Login-sikkerhed

Jeg vil anbefale at tilføje:

* **2FA (to-faktor login)**
* login begrænsning (brute force protection)

Det kan fx gøres med:

* Wordfence
* iThemes Security

Disse plugins kan:

* blokere hackere
* begrænse loginforsøg
* scanne for malware
* blokere IP-adresser

---

## 4. Backup (meget vigtigt)

Selvom hosten tager backup, er det en god idé også at have egen backup.

Fx via:

* WordPress backup plugin
* download af database
* kopi af filer

Hvis et site bliver hacket, er **backup den hurtigste løsning**.

---

## 5. Ekstra ting der øger sikkerheden meget

Nogle små ændringer kan gøre meget:

* brug **lange passwords**
* skift standard admin-brugernavn
* begræns loginforsøg
* deaktiver XML-RPC hvis du ikke bruger det
* brug 2FA

---

✅ **Kort sagt**

Med Simply.com + WordPress er sikkerheden normalt **helt fin**, hvis du:

* holder WordPress opdateret
* bruger få plugins
* installerer et sikkerhedsplugin
* bruger 2FA

---

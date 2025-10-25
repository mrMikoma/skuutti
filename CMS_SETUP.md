# Decap CMS Kirjautumisen Asennus - Täydelliset Ohjeet

Decap CMS tarvitsee kirjautumispalvelun toimiakseen. Tässä kaksi vaihtoehtoa:

---

## Vaihtoehto A: Netlify Identity + Git Gateway (SUOSITELTU - HELPOIN)

Tämä on helpoin vaihtoehto ei-teknisille käyttäjille. Netlify tarjoaa ilmaisen Identity-palvelun.

### Vaihe 1: Netlify-tilin luonti ja sivuston yhdistäminen

1. Mene osoitteeseen https://netlify.com
2. Klikkaa **Sign up** ja kirjaudu GitHub-tunnuksillasi
3. Klikkaa **Add new site** → **Import an existing project**
4. Valitse **GitHub** ja anna Netlifylle oikeudet
5. Valitse `skuutti` -repositorio
6. Build settings:
   - **Build command:** `bundle exec jekyll build`
   - **Publish directory:** `_site`
   - Klikkaa **Deploy site**

### Vaihe 2: Netlify Identity käyttöönotto

1. Kun sivusto on deployattu, mene **Site settings**
2. Vasen valikko: **Identity** → **Enable Identity**
3. **Settings and usage** -osiossa:
   - **Registration preferences:** Valitse **Invite only** (vain kutsutut voivat rekisteröityä)
   - **External providers:** Klikkaa **Add provider** → Valitse **GitHub** → **Enable**
4. (Valinnainen) **Git Gateway:**
   - Scrollaa alas kohtaan **Services**
   - Klikkaa **Enable Git Gateway**
   - Tämä on jo automaattisesti käytössä

### Vaihe 3: Käyttäjien kutsuminen

1. **Identity**-välilehdellä klikkaa **Invite users**
2. Syötä sähköpostiosoitteet (yksi per rivi) niille henkilöille, jotka saavat muokata sivua
3. Klikkaa **Send**
4. Kutsutut saavat sähköpostin, jossa he voivat luoda salasanan

### Vaihe 4: Netlify Identity Widget sivustolle

Päivitä `admin/index.html` lisäämällä Netlify Identity -skripti:

```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="robots" content="noindex" />
  <title>Content Manager</title>
  <!-- Netlify Identity Widget -->
  <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
</head>
<body>
  <!-- Include the script that builds the page and powers Decap CMS -->
  <script src="https://unpkg.com/decap-cms@^3.0.0/dist/decap-cms.js"></script>

  <!-- Netlify Identity redirect script -->
  <script>
    if (window.netlifyIdentity) {
      window.netlifyIdentity.on("init", user => {
        if (!user) {
          window.netlifyIdentity.on("login", () => {
            document.location.href = "/admin/";
          });
        }
      });
    }
  </script>
</body>
</html>
```

### Vaihe 5: Varmista admin/config.yml asetukset

Tiedoston `admin/config.yml` pitäisi olla:

```yaml
backend:
  name: git-gateway
  branch: main
```

### Vaihe 6: Testaus

1. Push muutokset GitHubiin
2. Odota että sekä Netlify että GitHub Pages -deploymentit valmistuvat
3. Mene osoitteeseen: `https://skuutti.mikoma.fi/admin/`
4. Klikkaa **Login with Netlify Identity**
5. Kirjaudu sähköpostilla ja salasanalla (jonka loit kutsusta)
6. Voit nyt muokata sisältöä!

**HUOM:** Vaikka käytät Netlifyä kirjautumiseen, varsinainen sivusto on GitHub Pagesissa osoitteessa `skuutti.mikoma.fi`. Netlify hoitaa vain kirjautumisen.

---

## Vaihtoehto B: GitHub Backend (OAuth App)

Tämä vaihtoehto vaatii OAuth-välityspalvelimen. **HUOM:** GitHub ei salli OAuth-kirjautumista suoraan staattisilla sivuilla ilman backend-palvelinta.

### Ongelma:
GitHub OAuth vaatii backend-palvelimen, joka käsittelee OAuth-callback:n. Staattiset sivut (kuten GitHub Pages) eivät voi tehdä tätä suoraan.

### Ratkaisu: Käytä Netlify Redirectiä (yhdistetty vaihtoehto)

Tämä on teknisesti monimutkaisempi, mutta ei vaadi Netlify Identityä:

### Vaihe 1: Luo GitHub OAuth App

1. Mene osoitteeseen https://github.com/settings/developers
2. Klikkaa **OAuth Apps** → **New OAuth App**
3. Täytä tiedot:
   - **Application name:** Skuutti CMS
   - **Homepage URL:** `https://skuutti.mikoma.fi`
   - **Authorization callback URL:** `https://api.netlify.com/auth/done`
4. Klikkaa **Register application**
5. Kopioi **Client ID** talteen
6. Klikkaa **Generate a new client secret** ja kopioi se talteen

**HUOM:** Tämäkin käyttää Netlifyä OAuth-välittäjänä!

### Vaihe 2: Netlify-sivuston asetukset

1. Yhdistä repositorio Netlifyyn (kuten vaihtoehdossa A, vaiheet 1-3)
2. Mene **Site settings** → **Access control** → **OAuth**
3. **Install provider** → **GitHub**
4. Liitä GitHub OAuth App:n **Client ID** ja **Client Secret**

### Vaihe 3: Päivitä admin/config.yml

```yaml
backend:
  name: github
  repo: KÄYTTÄJÄNIMI/skuutti  # Vaihda KÄYTTÄJÄNIMI omaan GitHub-käyttäjänimeesi
  branch: main
  base_url: https://api.netlify.com
  auth_endpoint: auth
```

### Vaihe 4: Testaus

1. Push muutokset GitHubiin
2. Mene: `https://skuutti.mikoma.fi/admin/`
3. Kirjaudu GitHub-tunnuksillasi

---

## Vaihtoehto C: Paikallinen kehitys ilman kirjautumista

Jos haluat testata CMS:ää paikallisesti ilman kirjautumista:

### Vaihe 1: Asenna Decap Server paikallisesti

```bash
npm install -g decap-server
```

### Vaihe 2: Päivitä admin/config.yml (VAIN PAIKALLISTA KEHITYSTÄ VARTEN)

```yaml
backend:
  name: git-gateway
  branch: main

# LISÄÄ TÄMÄ RIVI VAIN PAIKALLISTA KEHITYSTÄ VARTEN:
local_backend: true
```

### Vaihe 3: Käynnistä palvelimet

Terminaali 1:
```bash
npx decap-server
```

Terminaali 2:
```bash
bundle exec jekyll serve
```

### Vaihe 4: Avaa selaimessa

```
http://localhost:4000/admin/
```

Nyt voit muokata sisältöä paikallisesti ilman kirjautumista.

**HUOM:** Muista poistaa `local_backend: true` ennen tuotantoon viemistä!

---

## Suositus

**Käytä vaihtoehtoa A (Netlify Identity + Git Gateway)**

Syyt:
- ✅ Helpoin asentaa
- ✅ Ei vaadi OAuth-appin luomista
- ✅ Voit kutsua useita käyttäjiä helposti
- ✅ Käyttäjät voivat kirjautua sähköpostilla + salasanalla
- ✅ Ilmainen (5 käyttäjää/kuukausi ilmaiseksi)
- ✅ Paras vaihtoehto ei-teknisille käyttäjille

---

## Yhteenveto: Valitse vaihtoehto

| Vaihtoehto | Vaikeus | Käyttäjät | Kustannus | Suositus |
|------------|---------|-----------|-----------|----------|
| A: Netlify Identity | Helppo | Sähköposti + salasana | Ilmainen (5 käyttäjää) | ⭐⭐⭐⭐⭐ |
| B: GitHub OAuth | Keskivaikea | GitHub-tunnukset | Ilmainen | ⭐⭐⭐ |
| C: Paikallinen | Helppo | Ei kirjautumista | Ilmainen | Vain kehitystä varten |

---

## Seuraavat askeleet

1. Valitse vaihtoehto (suositus: A)
2. Seuraa vaiheittaisia ohjeita
3. Testaa kirjautuminen osoitteessa `/admin/`
4. Kutsu muut käyttäjät

Jos törmäät ongelmiin, katso **Ongelmanratkaisu**-osio alla.

---

## Ongelmanratkaisu

### "Cannot access admin" tai 404-virhe

- Varmista että `admin/index.html` on olemassa
- Varmista että sivusto on deployattu
- Tarkista että URL on oikein: `https://skuutti.mikoma.fi/admin/`

### "Error loading config.yml"

- Tarkista että `admin/config.yml` on olemassa
- Tarkista YAML-syntaksi (ei välilyöntejä ennen `backend:`)
- Tarkista että Git-repositorion nimi on oikein

### Netlify Identity ei toimi

- Varmista että Identity on enabled
- Varmista että olet kutsunut käyttäjän
- Tarkista että käyttäjä on vahvistanut sähköpostin
- Tyhjennä selaimen välimuisti

### "Not authorized" -virhe

- Varmista että käyttäjällä on kirjoitusoikeudet GitHub-repositorioon
- Varmista että Git Gateway on enabled Netlifyssa
- Kirjaudu ulos ja takaisin sisään

---

Lisätietoa: https://decapcms.org/docs/authentication-backends/

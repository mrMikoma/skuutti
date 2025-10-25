# Skuutti ry - Kotisivujen ylläpito-ohje

Tämä dokumentti opastaa ei-teknisen käyttäjän ylläpitämään Skuutti ry:n kotisivuja.

## Sisällönhallintajärjestelmä (CMS)

Sivut käyttävät Decap CMS -sisällönhallintajärjestelmää, jonka avulla voit muokata sisältöä graafisessa käyttöliittymässä ilman koodin kirjoittamista.

### Sisäänkirjautuminen

**TÄRKEÄÄ:** Käytä CMS:ää Netlify-osoitteessa!

1. Mene osoitteeseen: **`https://skuutticmstest.netlify.app/admin/`**
2. Klikkaa "Login with GitHub"
3. Kirjaudu GitHub-tunnuksillasi
4. Hyväksy tarvittavat käyttöoikeudet (ensimmäisellä kerralla)

**HUOM:**
- CMS-hallinta: `https://skuutticmstest.netlify.app/admin/`
- Julkinen sivusto: `https://skuutti.mikoma.fi`
- ÄLÄ käytä `skuutti.mikoma.fi/admin/` - se ei toimi OAuth-kirjautumisen kanssa!

### Sivujen muokkaaminen

**Etusivu, Tietoa meistä, Yhteystiedot:**
1. Valitse vasemmasta valikosta "Sivut"
2. Klikkaa muokattavaa sivua
3. Muokkaa sisältöä markdown-editorissa
4. Klikkaa "Publish" tallentaaksesi muutokset

**Tapahtumat:**
1. Valitse "Tapahtumat" vasemmasta valikosta
2. **Uuden tapahtuman luominen:**
   - Klikkaa "New Tapahtumat"
   - Täytä otsikko, päivämäärä, paikka ja kuvaus
   - Voit lisätä kansikuvan klikkaamalla "Choose an image"
   - Klikkaa "Publish"
3. **Tapahtuman muokkaaminen:**
   - Klikkaa tapahtumaa listasta
   - Tee tarvittavat muutokset
   - Klikkaa "Publish"
4. **Tapahtuman poistaminen:**
   - Klikkaa tapahtumaa listasta
   - Klikkaa "Delete entry"

**Galleria:**
1. Valitse "Galleria" vasemmasta valikosta
2. **Uuden albumin luominen:**
   - Klikkaa "New Galleria"
   - Anna albumille nimi ja päivämäärä
   - Valitse kansikuva
   - Lisää kuvia "Kuvat"-osioon
   - Klikkaa "Publish"

### Kuvien lisääminen

1. Klikkaa kuvakentässä "Choose an image"
2. **Uuden kuvan lataaminen:**
   - Klikkaa "Upload"
   - Valitse kuva tietokoneeltasi
   - Klikkaa "Insert from Media Library"
3. **Aiemmin ladatun kuvan käyttäminen:**
   - Selaa kuvagalleriaa
   - Klikkaa haluamaasi kuvaa
   - Klikkaa "Insert from Media Library"

### Markdown-muotoilu

CMS käyttää Markdown-kieltä tekstin muotoiluun. Tässä perusteet:

```
# Suuri otsikko
## Keskikokoinen otsikko
### Pieni otsikko

**Lihavoitu teksti**
*Kursivoitu teksti*

- Luettelokohta 1
- Luettelokohta 2

1. Numeroitu kohta 1
2. Numeroitu kohta 2

[Linkin teksti](https://example.com)
```

## Tekninen ylläpito

### Paikallinen testaus (vaatii teknisen osaamisen)

1. Asenna Ruby ja Jekyll
2. Kloonaa repositorio: `git clone [repository-url]`
3. Asenna riippuvuudet: `bundle install`
4. Käynnistä paikallinen palvelin: `bundle exec jekyll serve`
5. Avaa selaimessa: `http://localhost:4000`

### Julkaisu GitHub Pagesiin

Kaikki muutokset julkaistaan automaattisesti kun:
- Tallentelet muutokset CMS:ssä, TAI
- Push'aat muutokset GitHub-repositorioon

Julkaisu kestää 1-3 minuuttia.

### Custom domain -asetukset

1. Lisää `CNAME`-tiedosto repositorion juureen, sisältäen domain-nimen
2. Aseta DNS-asetuksissa:
   - A-tietue: `185.199.108.153`
   - A-tietue: `185.199.109.153`
   - A-tietue: `185.199.110.153`
   - A-tietue: `185.199.111.153`
3. GitHub repositorion Settings → Pages → Custom domain

### Netlify Identity -asetukset (CMS-kirjautumista varten)

Decap CMS vaatii kirjautumispalvelun. Suositus: Netlify Identity

1. Luo ilmainen Netlify-tili osoitteessa netlify.com
2. Liitä GitHub-repositorio Netlifyyn
3. Ota käyttöön Netlify Identity:
   - Site settings → Identity → Enable Identity
4. Muokkaa `admin/config.yml`:
   - Vaihda `git-gateway` → käyttäjätunnuksesi
5. Lisää `admin/index.html`-tiedostoon Netlify Identity widget

## Tuki ja ongelmatilanteet

### Yleiset ongelmat

**Muutokset eivät näy sivustolla:**
- Odota 1-3 minuuttia julkaisun valmistumista
- Tyhjennä selaimen välimuisti (Ctrl+F5 tai Cmd+Shift+R)
- Tarkista GitHub Actions -välilehdeltä, että build onnistui

**En pääse kirjautumaan CMS:ään:**
- Varmista että olet kirjautunut GitHub-tilillesi
- Tarkista että sinulla on kirjoitusoikeudet repositorioon
- Tarkista että Netlify Identity on konfiguroitu oikein

**Kuva ei lataudu:**
- Varmista että kuva on alle 10 MB
- Käytä tuettuja formaatteja: JPG, PNG, GIF
- Tarkista että kuva on tallentunut `/assets/images/uploads/` -kansioon

### Yhteystiedot tekniseen tukeen

[Lisää tähän teknisen ylläpitäjän yhteystiedot]

## Vinkkejä

- **Tee varmuuskopio:** GitHub säilyttää kaiken version historian
- **Testaa ensin:** Voit esikatsella muutoksia ennen julkaisua
- **Optimoi kuvat:** Pienennä isot kuvat ennen latausta (suositus: alle 500 KB)
- **Käytä kuvaavia nimiä:** Anna tapahtumille ja albumeille selkeät nimet
- **Päivitä säännöllisesti:** Poista vanhentuneet tapahtumat ja lisää uusia kuvia

## Lisätietoa

- Jekyll-dokumentaatio: https://jekyllrb.com/docs/
- Decap CMS -dokumentaatio: https://decapcms.org/docs/
- Markdown-opas: https://www.markdownguide.org/basic-syntax/
- GitHub Pages -dokumentaatio: https://docs.github.com/en/pages

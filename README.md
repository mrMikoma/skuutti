# Skuutti ry - Kotisivut

Purjeseuran kotisivut toteutettu Jekyllillä ja Decap CMS:llä.

## Osoitteet

- **Julkinen sivusto:** https://skuutti.mikoma.fi (GitHub Pages)
- **CMS-hallinta:** https://skuutticmstest.netlify.app/admin/ (Netlify)

## Ominaisuudet

- **Jekyll** - Staattinen sivugeneraattori
- **Decap CMS** - Helppokäyttöinen sisällönhallinta
- **GitHub Pages** - Ilmainen hosting
- **Responsiivinen design** - Toimii kaikilla laitteilla
- **Suomenkielinen** - Koko sisältö suomeksi

## Rakenne

```
skuutti/
├── _config.yml          # Jekyll-asetukset
├── _layouts/            # Sivupohjat
├── _events/             # Tapahtuma-artikkelit
├── _gallery/            # Kuvagalleriat
├── admin/               # Decap CMS hallintapaneeli
│   ├── index.html
│   └── config.yml       # CMS-asetukset
├── assets/
│   ├── css/             # Tyylit
│   └── images/          # Kuvat
├── index.md             # Etusivu
├── about.md             # Tietoa meistä
├── events.md            # Tapahtumat-listaus
├── gallery.md           # Galleria-listaus
├── contact.md           # Yhteystiedot
└── OHJEET.md           # Ylläpito-ohjeet ei-teknisille käyttäjille
```

## Alkuasennus

### 1. GitHub Pages -asetukset

1. Mene repositorion **Settings** → **Pages**
2. Source: **GitHub Actions**
3. (Valinnainen) Custom domain: Lisää domain-nimi

### 2. Decap CMS kirjautumisen asennus

Decap CMS tarvitsee kirjautumispalvelun. Suositus on käyttää **Netlify Identity**:

#### Vaihtoehto A: Netlify Identity (suositeltu ei-teknisille)

1. Luo ilmainen tili osoitteessa [netlify.com](https://netlify.com)
2. **New site from Git** → Valitse GitHub-repositorio
3. Build settings:
   - Build command: `bundle exec jekyll build`
   - Publish directory: `_site`
4. **Site settings** → **Identity** → **Enable Identity**
5. **Identity** → **Settings and usage**:
   - Registration preferences: **Invite only**
   - External providers: **Add provider** → GitHub
6. **Identity** → **Invite users** → Kutsu käyttäjät sähköpostilla

7. Päivitä `admin/index.html` lisäämällä Netlify Identity widget ennen `</head>`:
```html
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
```

8. Päivitä `admin/config.yml`:
```yaml
backend:
  name: git-gateway
  branch: main
```

#### Vaihtoehto B: GitHub Backend (teknisille käyttäjille)

1. Luo GitHub OAuth App:
   - Settings → Developer settings → OAuth Apps → New OAuth App
   - Homepage URL: `https://[username].github.io/[repo]`
   - Authorization callback URL: `https://api.netlify.com/auth/done`

2. Päivitä `admin/config.yml`:
```yaml
backend:
  name: github
  repo: [username]/[repo]
  branch: main
```

### 3. Custom domain (valinnainen)

1. Lisää tiedosto `CNAME` repositorion juureen:
```
www.skuutti.fi
```

2. DNS-asetukset domain-palveluntarjoajallasi:
```
A    @    185.199.108.153
A    @    185.199.109.153
A    @    185.199.110.153
A    @    185.199.111.153
CNAME www your-username.github.io
```

3. GitHub: Settings → Pages → Custom domain → `www.skuutti.fi`
4. Odota DNS:n päivittymistä (voi kestää jopa 24h)
5. Valitse **Enforce HTTPS**

## Paikallinen kehitys

### Edellytykset

- Ruby 3.x
- Bundler

### Asennus

```bash
# Kloonaa repositorio
git clone https://github.com/[username]/skuutti.git
cd skuutti

# Asenna riippuvuudet
bundle install

# Käynnistä paikallinen palvelin
bundle exec jekyll serve

# Avaa selaimessa
http://localhost:4000
```

### CMS paikallisesti

Paikallista CMS-käyttöä varten:

1. Lisää `admin/config.yml`:
```yaml
local_backend: true
```

2. Käynnistä backend:
```bash
npx decap-server
```

3. Käynnistä Jekyll toisessa terminaalissa
4. Avaa: `http://localhost:4000/admin/`

## Sisällön muokkaaminen

### CMS:n kautta (suositeltu)

1. Mene osoitteeseen: `https://[domain]/admin/`
2. Kirjaudu sisään
3. Muokkaa sisältöä visuaalisessa editorissa
4. Julkaise muutokset

### Suoraan tiedostoja muokkaamalla

1. Muokkaa `.md` -tiedostoja
2. Commit ja push GitHubiin
3. GitHub Actions rakentaa ja julkaisee automaattisesti

Katso tarkemmat ohjeet tiedostosta [OHJEET.md](OHJEET.md)

## Ylläpito

### Päivitykset

Riippuvuuksien päivitys:
```bash
bundle update
```

### Backup

Kaikki sisältö on versionhallinnassa GitHubissa. Voit palauttaa aiemman version:
```bash
git log
git checkout [commit-hash]
```

## Tuki

- **Jekyll-dokumentaatio:** https://jekyllrb.com/docs/
- **Decap CMS:** https://decapcms.org/docs/
- **GitHub Pages:** https://docs.github.com/en/pages

## Lisenssi

Sivuston sisältö © Skuutti ry. Kaikki oikeudet pidätetään.

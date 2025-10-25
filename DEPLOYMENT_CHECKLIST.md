# Skuutti ry - Käyttöönoton tarkistuslista

Tämä tarkistuslista auttaa sivuston käyttöönotossa GitHub Pagesiin.

## Vaihe 1: GitHub-repositorion asetukset

- [ ] Push'aa kaikki tiedostot GitHub-repositorioon
- [ ] Mene repositorion **Settings** → **Pages**
- [ ] Valitse Source: **GitHub Actions**
- [ ] Odota ensimmäisen deploymenttin valmistumista (Actions-välilehti)
- [ ] Testaa sivusto osoitteessa: `https://[käyttäjänimi].github.io/skuutti/`

## Vaihe 2: Decap CMS -kirjautumisen asennus

### Vaihtoehto A: Netlify Identity (suositeltu)

- [ ] Luo Netlify-tili osoitteessa https://netlify.com
- [ ] Yhdistä GitHub-repositorio Netlifyyn ("New site from Git")
- [ ] Build-asetukset:
  - Build command: `bundle exec jekyll build`
  - Publish directory: `_site`
- [ ] Ota käyttöön Identity: Site settings → Identity → Enable Identity
- [ ] Muuta rekisteröintiasetukset: Identity → Settings → Registration: **Invite only**
- [ ] Lisää GitHub external provider: Identity → Settings → External providers
- [ ] Kutsu käyttäjät: Identity → Invite users
- [ ] Päivitä `admin/index.html` lisäämällä Netlify Identity widget:
  ```html
  <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
  ```
- [ ] Varmista että `admin/config.yml` on asetettu:
  ```yaml
  backend:
    name: git-gateway
    branch: main
  ```
- [ ] Testaa kirjautuminen: `https://[domain]/admin/`

### Vaihtoehto B: GitHub Backend

- [ ] Luo GitHub OAuth App (Settings → Developer settings → OAuth Apps)
- [ ] Päivitä `admin/config.yml` GitHub-backendia varten
- [ ] Testaa kirjautuminen

## Vaihe 3: Custom domain (valinnainen)

- [ ] Luo `CNAME`-tiedosto repositorion juureen sisältäen domain-nimen
- [ ] Aseta DNS-tietueet domain-palveluntarjoajalla:
  ```
  A    @    185.199.108.153
  A    @    185.199.109.153
  A    @    185.199.110.153
  A    @    185.199.111.153
  ```
- [ ] Jos käytät www-alidomainia:
  ```
  CNAME www [käyttäjänimi].github.io
  ```
- [ ] GitHub Settings → Pages → Custom domain → Lisää domain
- [ ] Odota DNS:n päivittymistä (voi kestää 24h)
- [ ] Ota käyttöön "Enforce HTTPS"
- [ ] Testaa sivusto omalla domainilla

## Vaihe 4: Sisällön muokkaus

- [ ] Kirjaudu CMS:ään: `https://[domain]/admin/`
- [ ] Päivitä yhteystiedot (sähköposti, puhelinnumero, osoite)
- [ ] Päivitä "Tietoa meistä" -sisältö
- [ ] Lisää ensimmäinen tapahtuma
- [ ] Testaa kuvien lataus
- [ ] Tarkista että muutokset näkyvät sivustolla

## Vaihe 5: Sosiaalisen median integraatio

- [ ] Päivitä yhteystiedot-sivulle oikeat some-tilit
- [ ] Lisää Open Graph -metatagit (valinnainen)
- [ ] Lisää favicon (valinnainen)

## Vaihe 6: Koulutus

- [ ] Jaa [OHJEET.md](OHJEET.md) ei-teknisille ylläpitäjille
- [ ] Käy läpi CMS:n peruskäyttö
- [ ] Opasta kuvien lisääminen
- [ ] Näytä tapahtumien luominen ja muokkaaminen
- [ ] Kerro GitHub Actionsin seuranta

## Vaihe 7: Jatkuva ylläpito

- [ ] Määritä vastuuhenkilö sisällön päivitykselle
- [ ] Määritä tekninen yhteyshenkilö ongelmatilanteisiin
- [ ] Lisää kalenteriin muistutus: poista vanhentuneet tapahtumat kuukausittain
- [ ] Lisää kalenteriin muistutus: lisää uusia kuvia kaudesta

## Tärkeää muistaa

- Kaikki muutokset tallennetaan Git-historiaan → voit palauttaa aiemman version
- Muutosten julkaisu kestää 1-3 minuuttia
- CMS:ssä tehdyt muutokset luovat automaattisesti Git-commitit
- Kuvat tallennetaan `assets/images/uploads/` -kansioon
- Optimoi kuvat ennen latausta (suositus: alle 500 KB per kuva)

## Tuki

Jos kohtaat ongelmia:

1. Tarkista [README.md](README.md) ja [OHJEET.md](OHJEET.md)
2. Tarkista GitHub Actions -välilehdeltä mahdolliset virheet
3. Tyhjennä selaimen välimuisti
4. Ota yhteyttä tekniseen tukeen: [lisää yhteystiedot]

## Hyödyllisiä linkkejä

- Sivusto: `https://[domain]`
- CMS-hallinta: `https://[domain]/admin/`
- GitHub-repositorio: `https://github.com/[käyttäjänimi]/skuutti`
- GitHub Actions: `https://github.com/[käyttäjänimi]/skuutti/actions`
- Jekyll-dokumentaatio: https://jekyllrb.com/docs/
- Decap CMS -dokumentaatio: https://decapcms.org/docs/
- Markdown-opas: https://www.markdownguide.org/basic-syntax/

# DNS-asetukset domainille skuutti.mikoma.fi

## Vaihe 1: DNS-tietueiden lisääminen

Mene domain-palveluntarjoajasi hallintapaneeliin (jossa hallinnoit `mikoma.fi` -domainia) ja lisää seuraavat DNS-tietueet:

### Vaihtoehto A: CNAME-tietue (SUOSITELTU)

Jos haluat käyttää alidomainia `skuutti.mikoma.fi`:

```
Tyyppi: CNAME
Nimi: skuutti
Arvo: miko.github.io.
TTL: 3600 (tai oletusarvo)
```

**HUOM:** Vaihda `miko` omaksi GitHub-käyttäjänimeksesi!

### Vaihtoehto B: A-tietueet (jos CNAME ei toimi)

Jos CNAME ei jostain syystä toimi, voit käyttää A-tietueita:

```
Tyyppi: A
Nimi: skuutti
Arvo: 185.199.108.153
TTL: 3600

Tyyppi: A
Nimi: skuutti
Arvo: 185.199.109.153
TTL: 3600

Tyyppi: A
Nimi: skuutti
Arvo: 185.199.110.153
TTL: 3600

Tyyppi: A
Nimi: skuutti
Arvo: 185.199.111.153
TTL: 3600
```

## Vaihe 2: GitHub Pages -asetukset

1. Push'aa kaikki muutokset GitHubiin (mukaan lukien `CNAME`-tiedosto)
2. Mene GitHub-repositorioon
3. **Settings** → **Pages**
4. **Custom domain** -kohtaan kirjoita: `skuutti.mikoma.fi`
5. Klikkaa **Save**
6. Odota hetki (GitHub tarkistaa DNS-asetukset)
7. Kun DNS on kunnossa, ruksaa **Enforce HTTPS**

## Vaihe 3: Testaus

1. Odota 5-15 minuuttia DNS:n päivittymistä (voi kestää jopa 24h, mutta yleensä nopeammin)
2. Tarkista DNS-päivitys komennolla:
   ```bash
   nslookup skuutti.mikoma.fi
   ```
   tai
   ```bash
   dig skuutti.mikoma.fi
   ```

3. Testaa sivusto selaimessa: `https://skuutti.mikoma.fi`

## Yleisiä ongelmia

### DNS ei päivity
- Odota pidempään (jopa 24h)
- Tyhjennä DNS-välimuisti: `sudo systemd-resolve --flush-caches` (Linux)
- Tarkista että DNS-tietueet on kirjoitettu oikein

### GitHub näyttää "DNS check unsuccessful"
- Varmista että DNS-tietueet on lisätty oikein
- Odota 10-15 minuuttia ja yritä uudelleen
- Tarkista että GitHub-käyttäjänimesi on oikein CNAME-arvossa

### HTTPS ei toimi
- Varmista että DNS on kunnossa
- Odota hetki (GitHub luo SSL-sertifikaatin automaattisesti)
- Voi kestää muutaman minuutin

### Sivusto näyttää 404
- Varmista että `CNAME`-tiedosto on repositorion juuressa
- Varmista että GitHub Actions -build on onnistunut (Actions-välilehti)
- Odota muutama minuutti deploymenttin valmistumista

## DNS-tarkistustyökalut

Online-työkalut DNS:n tarkistamiseen:
- https://dnschecker.org
- https://www.whatsmydns.net

Syötä domain: `skuutti.mikoma.fi` ja tarkista että se osoittaa GitHubin IP-osoitteisiin tai `miko.github.io`:hin.

## Tärkeää

- `CNAME`-tiedosto pitää olla repositorion juuressa
- Tiedoston sisältö: vain domain-nimi (`skuutti.mikoma.fi`), ei mitään muuta
- Älä poista `CNAME`-tiedostoa, muuten custom domain lakkaa toimimasta
- GitHub luo automaattisesti ilmaisen SSL-sertifikaatin (Let's Encrypt)

## Yhteenveto checklistaksi

- [x] CNAME-tiedosto luotu repositorioon
- [x] _config.yml päivitetty oikealla URL:lla
- [ ] DNS-tietueet lisätty domain-hallintapaneeliin
- [ ] Muutokset push'attu GitHubiin
- [ ] GitHub Pages Custom domain asetettu
- [ ] DNS:n päivittyminen vahvistettu
- [ ] HTTPS otettu käyttöön
- [ ] Sivusto testattu osoitteessa https://skuutti.mikoma.fi

Kun kaikki on valmista, sivustosi on saatavilla osoitteessa: **https://skuutti.mikoma.fi**

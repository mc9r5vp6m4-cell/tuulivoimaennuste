# Tuulivoimaennuste

Tavoite:

Rakennetaan Suomen tuulivoimaennusteeseen perustuva pörssisähkön hintaennuste.

Ensimmäinen vaihe:
- Näytä Fingridin 72 h tuulivoimaennuste.

Toinen vaihe:
- Kerää historiadataa tunnin välein.

Kolmas vaihe:
- Mallinna tuulivoiman ja pörssisähkön hinnan suhde.

Neljäs vaihe:
- Ennusta seuraavan 72 tunnin pörssisähkön hinta.

Historiaan tallennetaan:
- Toteutunut sähkön hinta
- Toteutunut tuulivoimatuotanto
- Tuulivoimaennuste
- Kellonaika
- Viikonpäivä
- Kuukausi

Pitkän aikavälin tavoite:
Jatkuvasti itseään parantava Suomen pörssisähkön hintaennustemalli.




# Tuulivoimaennuste – jatkosuunnitelma

## Nykytilanne

Valmis:

- GitHub Pages toimii
- GitHub Actions toimii
- Fingrid dataset 181 (toteutunut tuulivoima) tallentuu history.json:iin
- Pörssisähkön hinta tallentuu history.json:iin
- Tuulivoiman dashboard toimii
- 24 h keskiarvo näkyy käyttöliittymässä

Kerättävä data:

```json
{
  "timestamp": "...",
  "price": 27.23,
  "windActual": 2639
}
```

---

# Vaihe 1 – Historiadatan kerääminen

Tavoite:

Kerätä vähintään:

- 7 vrk
- 14 vrk
- 30 vrk

historiadataa.

Tällöin saadaan:

- Tuulivoiman vaihtelu
- Sähkön hinnan vaihtelu
- Ensimmäiset korrelaatiot

Lisätään käyttöliittymään:

- 24 h keskiarvo
- 7 vrk keskiarvo
- 30 vrk keskiarvo

---

# Vaihe 2 – Tuuli ↔ hinta analyysi

Lasketaan:

- Keskimääräinen sähkön hinta eri tuulivoimatasoilla

Esimerkki:

| Tuulivoima | Keskimääräinen hinta |
|------------|---------------------|
| 0–2000 MW | 25 snt |
| 2000–4000 MW | 15 snt |
| 4000–6000 MW | 8 snt |
| 6000–8000 MW | 4 snt |
| >8000 MW | 2 snt |

Lisätään dashboardiin:

```text
Tuuli ↔ Sähkö korrelaatio

r = -0.73
```

---

# Vaihe 3 – Tuulivoimaennuste takaisin mukaan

Ratkaistaan dataset 245 -haku.

Tallennetaan historiaan:

```json
{
  "forecast1h": ...,
  "forecast6h": ...,
  "forecast12h": ...,
  "forecast24h": ...,
  "forecast48h": ...,
  "forecast72h": ...
}
```

Tavoite:

Tietää mitä ennuste uskoi kyseisellä hetkellä.

---

# Vaihe 4 – Oppiva historiadata

Tallennetaan:

```json
{
  "timestamp": "...",

  "price": ...,

  "windActual": ...,

  "forecast24h": ...
}
```

Näin voidaan myöhemmin vertailla:

```text
Ennuste
↓
Toteutunut tuuli
↓
Toteutunut sähkön hinta
```

---

# Vaihe 5 – Ensimmäinen hintaennuste

Dashboardiin:

```text
🔮 HINTASUUNTA

Todennäköisesti:
HALPENEE

tai

KALLISTUU
```

Perustuu:

- nykyiseen tuulivoimaan
- viimeisen viikon korrelaatioon
- tuulivoiman trendiin

---

# Vaihe 6 – 72 h sähkönhintaennuste

Kun dataset 245 toimii:

Lasketaan:

```text
Tuulivoimaennuste
↓
Historiassa vastaavan tilanteen hinnat
↓
Ennustettu sähkön hinta
```

Näytetään:

```text
+1 h
+6 h
+12 h
+24 h
+48 h
+72 h
```

Esimerkki:

```text
+1 h   18.2 snt
+6 h   16.4 snt
+12 h  11.2 snt
+24 h   6.7 snt
+48 h   4.3 snt
+72 h   8.8 snt
```

---

# Vaihe 7 – Itseään parantava malli

Kun havaintoja > 1000:

Rakennetaan regressiomalli.

Syötteet:

- Tuulivoima
- Kellonaika
- Viikonpäivä
- Kuukausi
- Tuulivoiman muutos
- Tuulivoimaennuste

Tulos:

```text
Ennustettu pörssisähkön hinta
```

Tallennetaan myös ennustevirhe:

```text
Ennuste: 7.2 snt
Toteutunut: 6.5 snt
Virhe: 0.7 snt
```

Näin malli oppii jatkuvasti historiasta.

---

# Lopputavoite

Rakentaa dashboard:

```text
🌬️ TUULIVOIMA NYT

XXXX MW

🔮 PÖRSSISÄHKÖENNUSTE

+1 h
+6 h
+12 h
+24 h
+48 h
+72 h
```

joka perustuu:

- Fingridin tuulivoimaennusteeseen
- omaan historiadataan
- automaattisesti paranevaan malliin
- toteutuneiden tulosten jatkuvaan seurantaan.

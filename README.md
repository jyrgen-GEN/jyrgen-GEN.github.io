# Lõikeleht — materjalikulu kalkulaator

Lihtne brauseripõhine tööriist, mis loeb sisse detailide lõikelehe `.txt` failid ja arvutab materjalide maksumuse.

Töötab täielikult brauseris — ei vaja serverit ega andmebaasi. Sobib majutamiseks nt **GitHub Pages** peal.

## Kasutamine

1. Ava `index.html` brauseris (või majuta GitHub Pages'is).
2. Lae üles üks või mitu `.txt` faili (võib korraga laadida mitu).
3. Sisesta iga materjali kohta hind:
   - **vineer**-tüüpi materjalidele hind **€ / m³**
   - **HDF**-tüüpi materjalidele hind **€ / m²**
   - iga materjali juures saab muuta ka **prügiprotsenti** (lõikejäägi arvestus) — vaikimisi **20%**, aga täiesti vabalt muudetav
4. Kui sama lõikeleht kordub mitu korda (nt sama toode mitu tükki), saad iga laaditud faili juures muuta **koguse kordajat** (vaikimisi 1) — kõik selle faili detailide kogused korrutatakse selle arvuga.
5. Maksumused arvutatakse automaatselt iga detaili, materjali ja kogu projekti kohta, arvestades nii faili koguse kordajat kui prügiprotsenti.

Hinnad ja prügiprotsendid salvestatakse automaatselt brauseri `localStorage`'isse — järgmine kord, kui lehte avad (samas brauseris, samast aadressist), on eelmised väärtused juba täidetud. Uue väärtuse sisestamine kirjutab vana üle. Faili koguse kordaja EI salvestu (see kehtib ainult jooksva laadimise kohta, kuna failid endid ei salvestu).

## `txt/` kaust — failide automaatne sisselugemine

Projektis on kaust `txt/`, kuhu saad ise lisada `.txt` lõikelehe faile, mis laetakse veebilehel **automaatselt** sisse (nt kui majutad GitHub Pages'is).

**Kuidas lisada fail:**
1. Kopeeri oma `.txt` fail `txt/` kausta.
2. Ava `txt/manifest.json` ja lisa faili nimi massiivi (tähtede/suurtähtede osas täpselt sama nagu failinimi), näiteks:
   ```json
   ["lehed_2026_01.txt", "lehed_2026_02.txt"]
   ```
3. Commiti ja pushi GitHubi. Kui leht avatakse (nt GitHub Pages'i lingilt), loetakse need failid automaatselt sisse.

**Oluline:** `txt/` kaustast automaatselt laetud failid **EI OSALE** kohe hinna arvutuses — need on failide loendis linnukeseta (mitteaktiivsed). Arvutusse kaasamiseks tuleb igal failil eraldi linnuke ette panna. Nii saad hoida kaustas kogu ajaloolist/varu-materjali, ilma et see kogemata kokkuvõtet mõjutaks.

Käsitsi üleslaaditud (drag & drop / kliki) failid on vaikimisi **aktiivsed** (linnuke juba peal), nagu senini.

**Piirang:** see automaatlaadimine töötab ainult siis, kui leht on avatud üle `http(s)://` (nt GitHub Pages, mõni muu veebimajutus, või kohalik server nagu `python3 -m http.server`). Kui avad `index.html` failina otse brauseris topeltklõpsuga (`file://...`), blokeerib brauser turvakaalutlustel sellised päringud ja `txt/` kausta faile automaatselt sisse ei loeta — üleslaadimine käsitsi töötab aga alati.

## Sisendfaili formaat

Tab-eraldajaga `.txt` fail, esimene rida on veerupäis:

```
Detaili_kood	Detaili_nimetus	Kogus	Pikkus	Laius	Paksus	Põhimaterjal	Pealistusmaterjal	Detaili_k
1	C-2501-V09-012	1	220	430	9	vineer 9mm
2	C-2501-V09-016	1	630	430	9	vineer 9mm
```

- Kõik mõõdud (Pikkus, Laius, Paksus) on **millimeetrites**.
- Kõiki detaile käsitletakse **kandiliste (ristkülikukujuliste)** toorikutena — maht/pind arvutatakse Pikkus × Laius × Paksus (või Pikkus × Laius) × Kogus.
- Materjali nimetuse põhjal ("vineer" / "HDF") tuvastatakse automaatselt, kas hind käib m³ või m² kohta. Vajadusel saab ühikut käsitsi muuta.

## Andmete privaatsus

Kogu töötlus toimub kasutaja brauseris. Ühtegi faili ega hinda ei saadeta kuhugi serverisse.

## Piirangud / edasiarendused

- Eeldab kandilisi detaile (servaviimistlust/freesitud kujusid ei arvestata).
- Pealistusmaterjali (Pealistusmaterjal veerg) hinda hetkel ei arvutata — kuvatakse ainult detaili juures infona.
- Hinnad on brauseripõhised (localStorage), mitte jagatud mitme kasutaja/seadme vahel. Kui on vaja hinnakirja jagada meeskonnaga, tuleks lisada eraldi hinnakirja fail (nt JSON) või lihtne backend.
- `txt/manifest.json` tuleb iga uue faili lisamisel käsitsi uuendada — automaatset kausta sisu loendamist staatilise saidi peal (nt GitHub Pages) ei ole tehniliselt võimalik teha.

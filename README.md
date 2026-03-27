# Prisorama

En lokal, databasefri HTML-app for å registrere og visualisere objekter – f.eks. rom, leiligheter, eiendommer eller utstyr – på et bakgrunnsbilde (plantegning, kart e.l.).

**Ingen installasjon. Ingen server. Åpne `index.html` i nettleseren og kjør.**

---

## Funksjoner

### Tabell
- Registrer objekter med tilpassede parametere (tekst, tall, valgliste, formel, ja/nei)
- Sortering og søk på navn
- Per-kolonne filtrering (inneholder / min–maks / valgliste / ja/nei)
- Inline redigering direkte i tabellen
- Dra-og-slipp kolonneoverskrifter for å endre rekkefølge
- Lagrede tabellvisninger – skjul/vis ulike kolonnekombiner og bytt mellom dem
- Aggregeringsrad (Sum / Gjennomsnitt / Antall / Minst / Størst) nederst i tabellen
- CSV-import og -eksport med strukturert filnavn (`prisorama_navn_dato_tid.csv`)

### Grafisk visning
- Last inn et bakgrunnsbilde (plantegning, kart o.l.) via sidepanelet
- Plasser objekter som drabare overlay-kort på bildet
- Støtte for flere bildevisninger med egne bilder og plasseringer
- Shift+klikk for flervalg – bulk-endre verdi, lås, fjern
- Skalér bakgrunnsbildet uavhengig av zoom, med låsefunksjon
- Fargekoding av overlays basert på en valgliste-parameter (24-fargers palett)
- Heatmap med 8 fargeskalaer (grønn→rød, blå→rød, divergerende m.fl.)
- Justerbar gjennomsiktighet og tekststørrelse for overlay-kortene
- Filtrer «uplasserte objekter» i sidepanelet på kolonneinnhold
- Administrer parametere direkte fra grafisk sidepanel

### Rapport
- Grupper etter parameter, velg måling (sum/snitt/antall/min/maks)
- SVG-søylediagram (opptil 30 grupper)
- Totallinje nederst i rapporten
- **Scenariesammenligning** – sammenlign nåværende data med et snapshot, eller to snapshots mot hverandre, med Δ absolut og Δ %
- Eksport til CSV med strukturert filnavn

### Aksjoner
- Juster tallverdier i bulk: fast beløp, prosent, multipliser, sett til verdi
- Legg til **flere AND-filterbetingelser** for å begrense hvilke rader som påvirkes
- Valgfri avrunding (nærmeste 10 / 100 / 1 000 / 10 000 / 100 000 / 1 000 000)
- Live forhåndsvisning av alle berørte objekter med gammel → ny verdi
- **KPI-simulering** – viser beregnet påvirkning på nøkkeltallet i topbaren før utførelse

### Parametere
| Type | Beskrivelse |
|------|-------------|
| Tekst | Fritekst |
| Tall | Numerisk – støtter tusenskille (f.eks. `1 250 000`) |
| Valgliste | Dropdown med forhåndsdefinerte valg; støtter fargekoding per valg |
| Formel | Beregnet felt, f.eks. `pris / areal` – oppdateres automatisk |
| Ja/Nei | Avkrysningsboks |

### Andre funksjoner
- **Nøkkeltall (KPI) i topbar** – velg parameter, funksjon og tusenskille; dobbeltklikk for å konfigurere
- **Snapshots** – lagre navngitte versjoner av innholdet, gi dem nytt navn og gjenopprett med ett klikk
- **Angre/Gjør om** – Ctrl+Z / Ctrl+Y med opptil 50 steg bakover
- **Prosjektnavn** – klikk for å redigere, vises i topbar
- **Bruksanvisning** – innebygd hjelp tilgjengelig via ?-knappen i topbar
- **Auto-lagring** til localStorage etter hver endring
- **JSON-eksport/-import** med strukturert filnavn – inkl. innbygde bilder ved behov

---

## Lagring og bilder

Prosjektet lagres automatisk i nettleserens `localStorage`. For å flytte data til en annen PC eller nettleser:

- **Eksporter som JSON** (topbar) – inkluderer alle data, snapshots og innstillinger
- **Strukturert filnavn**: `prisorama_prosjektnavn_ååååmmdd_ttmm.json/.csv`
- **Bakgrunnsbilder** lagres kun i JSON-eksporten hvis «Bygg inn bilde» er aktivert i grafisk sidepanel. Bilder over ~4 MB lagres ikke i localStorage (nettlesergrense), men inkluderes alltid i JSON-eksporten når de er innebygd.

---

## Kom i gang

1. Last ned eller klon repoet
2. Åpne `index.html` i en nettleser (Chrome, Firefox, Edge, Safari)
3. Klikk **Parametere** for å definere kolonner (egenskaper)
4. Klikk **Legg til objekt** for å registrere data
5. Gå til **Grafisk** for å laste inn et bakgrunnsbilde og plassere objekter
6. Les den innebygde **Bruksanvisningen** (?-knappen) for full oversikt

---

## Hurtigtaster

| Tast | Funksjon |
|------|----------|
| Ctrl+Z | Angre |
| Ctrl+Y / Ctrl+Shift+Z | Gjør om |
| Shift+klikk (grafisk) | Flervalg av overlay-kort |
| Dobbeltklikk på overlay | Åpne redigeringspanel |
| Dobbeltklikk på KPI | Konfigurer nøkkeltall |

---

## Teknisk

- Én enkelt HTML-fil – ingen avhengigheter, ingen byggsteg, ingen server
- Vanilla JavaScript, CSS og HTML
- Fungerer helt offline
- Testet i Chrome, Firefox og Safari

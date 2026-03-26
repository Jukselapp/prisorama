# Prisorama

En lokal, databasefri HTML-app for å registrere og visualisere objekter – f.eks. rom, leiligheter, eiendommer eller utstyr – på et bakgrunnsbilde (plantegning, kart e.l.).

**Ingen installasjon. Ingen server. Åpne `index.html` i nettleseren og kjør.**

---

## Funksjoner

### Tabell
- Registrer objekter med tilpassede parametere (tekst, tall, valgliste, formel, ja/nei)
- Sortering og søk på navn
- Per-kolonne filtrering (inneholder / min-maks / valgliste / ja/nei)
- Inline redigering direkte i tabellen
- Dra-og-slipp for å endre kolonnerekkefølge
- Lagrede tabellvisninger – skjul/vis kolonner per visning
- Aggregeringsrad (Σ / ⌀ / N / ↓ / ↑) som tfoot i tabellen
- CSV-import og -eksport

### Grafisk visning
- Last inn et bakgrunnsbilde (plantegning, kart o.l.)
- Plasser objekter som drabare overlay-kort på bildet
- Støtte for flere bildevisninger med egne bilder og plasseringer
- Shift+klikk for flervalg – bulk-endre verdi, lås, fjern
- Skalér bakgrunnsbildet uavhengig av zoom (med låsefunksjon)
- Fargekoding av overlays basert på en valgliste-parameter (24-fargers palett)
- Heatmap med 8 fargeskalaer (grønn→rød, blå→rød, divergerende m.fl.)
- Justerbar gjennomsiktighet og tekststørrelse for overlay-kortene
- Filtrer "uplasserte objekter" i sidepanelet på kolonneinnhold

### Rapport
- Grupper etter parameter, velg måling (sum/snitt/antall/min/maks)
- SVG-søylediagram (opptil 30 grupper)
- Totallinje nederst i rapporten
- Eksport til CSV

### Aksjoner
- Juster tallverdier i bulk: fast beløp, prosent, multipliser, sett verdi
- Valgfritt filter – kun rader der en annen kolonne har en bestemt verdi
- Valgfri avrunding (nærmeste 10 / 100 / 1 000 / 10 000 / 100 000 / 1 000 000)
- Live forhåndsvisning av alle berørte objekter før utførelse

### Parametere
| Type | Beskrivelse |
|------|-------------|
| Tekst | Fritekst |
| Tall | Numerisk – støtter tusenskille (nb-NO) |
| Valgliste | Dropdown med forhåndsdefinerte valg |
| Formel | Beregnet felt, f.eks. `pris / areal` |
| Ja/Nei | Avkrysningsboks |

### Andre funksjoner
- **Nøkkeltall i topbar** – velg ett aggregert nøkkeltall som alltid vises
- **Snapshots** – lagre navngitte versjoner av innholdet og hopp mellom dem
- **Angre/Gjør om** – Ctrl+Z / Ctrl+Y med opptil 50 steg bakover
- **Prosjektnavn** – klikk for å redigere, vises i topbar
- **Auto-lagring** til localStorage
- **JSON-eksport/-import** – inkl. innbygde bilder (base64) ved behov

---

## Lagring og bilder

Prosjektet lagres automatisk i nettleserens `localStorage`. For å ta med seg dataene:

- **Eksporter som JSON** (topbar) – inkluderer alle data
- **Bakgrunnsbilder** lagres kun i JSON-eksporten hvis "Bygg inn bilde" er aktivert i grafisk sidepanel. Bilder over ~4 MB lagres ikke i localStorage (nettlesergrense), men er alltid med i JSON-eksporten når de er innebygd.

---

## Kom i gang

1. Last ned eller klon repoet
2. Åpne `index.html` i en nettleser (Chrome, Firefox, Edge, Safari)
3. Klikk **Parametere** for å definere kolonner
4. Klikk **Legg til objekt** for å registrere data
5. Gå til **Grafisk** for å laste inn et bakgrunnsbilde og plassere objekter

---

## Teknisk

- Én enkelt HTML-fil – ingen avhengigheter, ingen byggsteg
- All kode: vanilla JavaScript, CSS og HTML
- Fungerer helt offline
- Testet i Chrome, Firefox og Safari

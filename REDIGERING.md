# Redigere nettsiden – guide for ansatte

Denne guiden viser hvordan du kan gjøre enkle tekstendringer på nettsiden direkte i nettleseren, uten å installere noe. Endringer blir synlige på nettsiden i løpet av ca. 1 minutt etter at du lagrer.

## Før du starter

Du trenger en egen GitHub-konto med tilgang til nettsiden. Be den som administrerer nettsiden (eier av kontoen `theavage`) om å legge deg til som "Collaborator" på prosjektet — da får du din egen innlogging i stedet for å dele passord.

## Slik endrer du tekst

1. Gå til [github.com/theavage/PhansyPhysio](https://github.com/theavage/PhansyPhysio) og logg inn.
2. Klikk på filen du vil endre:
   - **index.html** – forsiden (Hjem)
   - **fysioterapeuter.html** – siden "Våre fysioterapeuter"
   - **kontakt.html** – siden "Kontakt oss"
   - **sporsmal.html** – siden "Spørsmål"
3. Klikk på blyant-ikonet (✏️) øverst til høyre i filvisningen for å redigere.
4. Finn teksten du vil endre (bruk Ctrl+F / Cmd+F for å søke i teksten på siden).
5. Endre **kun teksten** som står mellom `>` og `<` — se advarselen under.
6. Scroll helt ned, skriv en kort beskrivelse av hva du endret (f.eks. "Oppdatert åpningstider"), og klikk den grønne knappen **"Commit changes..."** → **"Commit changes"**.
7. Vent ca. 1 minutt, og sjekk at endringen vises på den faktiske nettsiden.

## ⚠️ Viktig – hva du IKKE skal røre

HTML-koden består av "tagger" i vinkelparenteser, f.eks. `<p>` og `</p>`. Alt mellom en åpne-tagg og en lukke-tagg kan du trygt endre. **Ikke** slett eller endre selve taggene, eller tekst inni anførselstegn som `class="..."` eller `href="..."` — det kan ødelegge utseendet eller funksjonaliteten til siden.

**Trygt å endre** (vanlig tekst mellom tagger):
```html
<p>Vi er en fysioterapiklinikk sentralt plassert på Nesttuntorget i Bergen.</p>
```
→ du kan trygt endre teksten mellom `<p>` og `</p>`.

**IKKE rør** (kode/attributter):
```html
<a href="tel:55133483">55 13 34 83</a>
```
→ Her kan du endre selve telefonnummeret to steder (etter `tel:` og i teksten), men ikke slett `<a href="tel:...">` eller `</a>`.

Hvis du er usiktrende: gjør én liten endring om gangen, lagre, og sjekk at siden fortsatt ser riktig ut før du gjør neste endring.

## Fjerne den oransje/kursiverte plassholder-stilen

Tekst som ennå ikke er reell (telefonnummer, åpningstider, navn osv.) er markert med `class="placeholder"`, som gjør at den vises i kursiv, oransje skrift — et signal om at "dette må fylles inn". Når du erstatter en slik plassholder med ekte innhold, bør du også fjerne denne markeringen, slik at teksten vises som vanlig tekst.

Det finnes på to måter i koden:

**1. Som en egen `<span>` rundt teksten:**
```html
<span class="placeholder">[+47 XX XX XX XX]</span>
```
Bytt ut den hakeparentesteksten med det ekte nummeret, og slett også `<span class="placeholder">` i starten og `</span>` på slutten — behold kun selve teksten:
```html
+47 55 12 34 56
```

**2. Som en ekstra klasse på en tagg som allerede har en annen klasse:**
```html
<p class="staff-role placeholder">[Fysioterapeut / spesialisering]</p>
```
Bytt ut hakeparentesteksten, og fjern kun ordet `placeholder` fra `class`-listen (behold `staff-role` — den styrer selve utseendet, ikke plassholder-status):
```html
<p class="staff-role">Fysioterapeut / Manuellterapeut</p>
```
Hvis `placeholder` er den eneste klassen på taggen (f.eks. `<td class="placeholder">`), kan du fjerne hele `class="placeholder"`-biten.

Tommelfingerregel: fjern kun selve ordet `placeholder` — ikke rør andre klassenavn eller selve taggene.

## Vanlige ting å oppdatere

| Hva | Hvilken fil | Hint |
|---|---|---|
| Åpningstider | `kontakt.html` | Søk etter "Åpningstider" — hver dag står i en egen `<tr>...</tr>`-linje |
| Telefonnummer | Alle 3 filer (footer) + `kontakt.html` | Søk etter "55 13 34" |
| E-postadresse | Alle 3 filer (footer) + `kontakt.html` | Søk etter "post@" |
| Fysioterapeutenes navn/bio | `fysioterapeuter.html` | Søk etter "[Navn Etternavn]" |
| "Vi tilbyr"-tekst | `index.html` | Søk etter "Vi tilbyr" |
| Bilder | Last opp ny fil i riktig mappe under `assets/` (se README-filen i hver mappe), og be om hjelp til å oppdatere filnavnet i HTML-en hvis det ikke heter det samme som det gamle bildet |

## Hvis noe går galt

Alle endringer lagres i historikken. Gå til fanen **"History"** for filen (eller **"Commits"** øverst i repoet) for å se tidligere versjoner, og du kan alltid be om hjelp til å tilbakestille en endring.

## Vil dere ha en enklere løsning senere?

Denne metoden krever at man er litt forsiktig med HTML-koden. Om det viser seg upraktisk i det daglige, kan vi sette opp et skjemabasert redigeringsverktøy (Decap CMS) som fjerner behovet for å røre kode i det hele tatt — dette krever litt ekstra oppsett (se `agent/PLAN.md`, Fase 1a).

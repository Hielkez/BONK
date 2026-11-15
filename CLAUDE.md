# BONK – projectcontext voor Claude

BONK is een Nederlands partyspel als mobiele web-app, gemaakt door Hielke (repo `Hielkez/BONK`, live op https://hielkez.github.io/BONK/ via GitHub Pages).
Praat Nederlands met Hielke, kort en direct. Hij test alles op zijn iPhone (vaak als beginscherm-app), dus mobiel en iOS gaan voor.

## Het spel
- Twee teams, of met z'n tweeën (1 tegen 1). Om de beurt legt één speler woorden uit, 30, 45 of 60 seconden per beurt.
- Uitleggen mag **alleen met woorden van één lettergreep**. Een langer woord, of (een stuk van) het woord zelf zeggen = fout: de tegenpartij roept **BONK!** (−1 punt, nieuwe kaart). Een opgerolde krant om mee te meppen mag, hoeft niet.
- Elke kaart: makkelijk woord (+1) en moeilijk woord (+3) die bij elkaar horen. Het makkelijke mag een opstapje naar het moeilijke zijn; per kaart telt één woord. Overslaan bestaat niet.
- Doel 15 / 30 / 50 punten of zonder einde. Score kan niet onder 0.
- Eerlijk einde: haalt een team het doel, dan speelt het de beurt gewoon uit; het andere team krijgt daarna een volle laatste beurt. Wie na die ronde voor staat wint, gelijk = nog een ronde. Haalt het tweede team de stand in tijdens die laatste beurt, dan wint het meteen.

## Techniek
- Alles zit in **één bestand: `index.html`** (HTML + CSS + JS, geen build-stap). Verder: `words.json` (kaarten), `firebase-config.js`, `manifest.webmanifest`, iconen, `mictest.html` (los experiment met spraakherkenning, niet in gebruik).
- **Twee telefoons** via Firebase Realtime Database (compat SDK 10.14.1, project `bonk-b82ff`, regio europe-west1). Pad `games/<CODE>` (4 tekens A–Z/0–9):
  - `host`: geclaimd met een transaction; de host-telefoon is de baas over de stand.
  - `state`: host schrijft de hele stand (gedebounced); gast spiegelt die (`applyState`).
  - `fx`: één overschreven effect-node (aftellen, goed, BONK…).
  - `in`: gast pusht commando's en BONKs, host verwerkt en verwijdert ze (dedupe via kaart-`seq`).
  - `guests/<id>`: aanwezigheid met onDisconnect.
  - Klok via `.info/serverTimeOffset`; aftellen en beurttijd lopen op servertijd zodat beide telefoons gelijk lopen.
  - Database-regels: lezen/schrijven alleen onder `games/$code` met een geldige code. Data blijft klein (~1–2 kB per spel).
- Zonder tweede telefoon werkt alles op één telefoon die je doorgeeft.
- **Taal**: Nederlands (standaard) of Engels, te kiezen op het keuzescherm en in het menu (niet tijdens een beurt). `S.lang` in de stand; elke taal heeft een eigen stapel, de andere wordt bewaard in `S.decks`. Vaste HTML-teksten hebben `data-t`/`data-ta` (Nederlands staat in de HTML, Engels in `EN`), teksten uit de code staan in `TX` en gaan via `T()`. Met twee telefoons volgt de gast de taal van de host.
- Stand en voortgang in `localStorage` (`woordspel-v1`), deck wordt pas opnieuw geschud als alle kaarten op zijn (ook over potjes heen).

## Belangrijke keuzes (niet zomaar terugdraaien)
- Startscherm: duidelijk **Spel maken** / **Meedoen**. Spel maken = stappenplan: namen (+ keuze twee teams / met z'n tweeën) → doel → beurttijd → tweede telefoon koppelen → samenvatting.
- Code-invoer moet overduidelijk een invoerveld zijn (iemand dacht dat "ABCD" de code was).
- Elk team past alleen de eigen naam aan.
- Kaart pas na **1,2 s** aantikbaar (`MIN_CARD_MS`) tegen +3-spammen; host controleert dit ook.
- Na BONK korte lock (`BONK_PAUSE` 1400 ms), BONK telt nooit dubbel.
- Taps via `pointerdown` (`onTap`) zodat er geen tikken wegvallen.
- 3-2-1 met leeglopende ring (`COUNT_STEP` 800 ms); kaarten pas na GO! uitdelen. Een geraden kaart vliegt als geheel (beide woorden) weg.
- BONK-scherm (tegenpartij) toont stand, doel en aftelbalk. In **1 tegen 1** ziet de rader géén woorden (worden ook niet naar zijn telefoon gestuurd).
- 1 tegen 1 op één telefoon: optie **Woorden verbergen**; de rader houdt de telefoon vast en tikt "Laat zien" (4 s).
- iOS: statusbalk `black` (niet `black-translucent`), anders lege strook onderin door WebKit-bug 301108. Spelregels zijn een volledig scherm met vaste kop.
- Geen kaartnummer in beeld, geen schud-/kaartbewerk-functies.
- Spelregels in 5 korte stappen, niet "AI-achtig", geen "klap mee"-zin.
- Microfoon/automatische BONK is geprobeerd en afgewezen (niet betrouwbaar genoeg).

## Kaarten (`words.json`)
Engelse kaarten staan in `words-en.json` (zelfde formaat, eigen `version`, nu versie 1, 388 kaarten); dezelfde regels gelden, met Engelse woorden van één lettergreep.
Formaat: `{"version": N, "info": "...", "cards": [["Makkelijk","Moeilijk"], ...]}` (nu versie 7, 982 kaarten). **Verhoog `version`** bij elke wijziging, anders schudt het spel de nieuwe kaarten niet in.
Regels voor een goede kaart:
- Makkelijk: concreet, kent een kind van 10, goed met korte woorden te omschrijven.
- Moeilijk: iedereen van 15+ kent het, maar de voor de hand liggende uitleg bestaat uit lange woorden. Vaak een situatie, handeling of typisch Nederlands moment (Fiets → Tegenwind, School → Spijbelen, Kerst → Gourmetten, Film → Spoiler).
- Duidelijk verwant, maar **niet dezelfde stam** (fout: Stad → Stadhuis, Trui → Coltrui) en geen synoniem of soort-van (fout: Aap → Chimpansee).
- Geen gekunstelde samenstellingen (Stickervel, Tostiapparaat), geen zinnetjes (Kusje erop); vaste begrippen zoals Rode loper of Eb en vloed mogen.
- Elk woord maar één keer in de hele stapel. Geen merknamen, echte personen of grove inhoud.

## Werkwijze
- Verhoog bij elke wijziging `const VERSION = "vNN · <datum tijd>"` in `index.html`; de footer toont die en de app meldt updates.
- Test met Playwright (headless Chromium): enkel-telefoon, eindspel (nep-klok) en twee telefoons met een nep-Firebase. Bekijk schermen op 390×844.
- Commit naar `main` en push; GitHub Pages zet het binnen een minuut live.

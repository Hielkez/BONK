# BONK!

Woordspel voor twee teams of met z'n tweeën. Omschrijf het woord met **alleen woorden van één lettergreep**. Zeg je een langer woord, of een stuk van het woord zelf? Dan roept de tegenpartij **BONK!** en kost het je een punt.

Speel het op: https://hielkez.github.io/BONK/ (zet hem op je beginscherm voor een app-gevoel).

## Spelen
1. Tik op **Spel maken** en loop het stappenplan door: namen, tot hoeveel punten, hoe lang een beurt duurt.
2. Speel je met twee telefoons? De andere telefoon tikt op **Meedoen** en typt de code (of scant de QR-code). Wie uitlegt ziet de kaart, de andere telefoon wordt het BONK-scherm met de stand en een grote BONK-knop.
3. Eén telefoon kan ook: die geef je door.

Elke kaart heeft een makkelijk woord (+1) en een moeilijk woord (+3) die bij elkaar horen. Per kaart telt één woord, overslaan kan niet. Haalt een team het doel, dan krijgt het andere team nog één volle beurt om in te halen.

## Techniek
Eén bestand (`index.html`), gehost op GitHub Pages. Twee telefoons praten via Firebase Realtime Database (`firebase-config.js`). De kaarten staan in [`words.json`](words.json); verhoog `version` als je de lijst aanpast.

Meer achtergrond en alle ontwerpkeuzes: [`CLAUDE.md`](CLAUDE.md).

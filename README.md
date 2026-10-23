# BONK!

Woordspel voor twee teams. Omschrijf het woord met **alleen woorden van één lettergreep**. Zeg je een langer woord? Dan krijg je een BONK met de opgerolde krant van de tegenpartij.

Speel het op: https://hielkez.github.io/BONK/

## Elk team een eigen telefoon

1. Open de site op de telefoon van team A. Op het startscherm staat een spelcode en een QR-code.
2. Team B opent de site op hun telefoon, tikt op **Meedoen** en typt de code (of scant de QR-code).
3. Wie aan de beurt is ziet de kaarten. De andere telefoon toont het woord, de tijd en een grote BONK-knop voor de tegenpartij. Na elke beurt wisselen de schermen vanzelf.

De telefoon die het spel maakt houdt de stand bij; instellingen en kaarten regel je daar. Zonder tweede telefoon speel je gewoon met één telefoon die je doorgeeft.

De telefoons verbinden direct met elkaar via WebRTC. [PeerJS](https://peerjs.com) koppelt ze aan elkaar met de gratis openbare koppelserver. Er is geen eigen server nodig.

## Eigen kaarten

Menu → Kaarten bewerken. Eén kaart per regel: `makkelijk / moeilijk`.

De standaardkaarten staan in [`words.json`](words.json). Pas die aan (en verhoog `version`), dan krijgt iedereen de nieuwe kaarten.

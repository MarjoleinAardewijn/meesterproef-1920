# Aantekeningen Code en Design Reviews

## Code Review #1 - Joost - 27-05-2020

Vraag:
- Hoe kan je het beste samen in 1 branch werken?

Antwoord: Niet doen, branches zo klein mogelijk maken zodat je er in je eentje aan kan werken.

Afbeeldingen in andere branch
branch voor samenvoegen
afspraken voor commits
https://medium.com/@jonathanmines/the-ultimate-github-collaboration-guide-df816e98fb67

## Design Review #1 - Koop - 28-05-2020

Vraag: 
- Hoe kunnen we de antwoorden die zijn gegeven het beste weergeven, zodat ze niet wegvallen door de scroll?
- Is de flow van de app logisch?


Misschien in de vraag het vorige antwoord verwerken, zodat dit nog terug te lezen is.
Flow zat goed in elkaar.

Design tip Monika: Op het ene scherm is een button in het midden en op de andere niet. Dus nog niet consistent.

https://www.uxmatters.com/mt/archives/2017/03/design-for-fingers-touch-and-people-part-1.php

## Code Review #2 - Laurens - 03-06-2020

Vraag:
- Hoe kan je sockets het beste in een aparte module plaatsen?

Laurens heeft gekeken bij een aantal studenten uit onze klas en zag dat Lennart dit had gedaan.

[Sockets module](https://github.com/lennartdeknikker/wiki-game/blob/828c6b3524a479c2d1bc98a0caf7d62620dc94d9/server/scripts/socket.js) \
[importeerd module hier](https://github.com/lennartdeknikker/wiki-game/blob/5d7821d94ea210f6970651d4018498a0b59e5256/bin/www)

## Design Review #2 - Vasilis - 04-06-2020

Vraag:
- Hoe kan je ervoor zorgen dat het design consistent blijft maar ook niet te druk wordt op bepaalde pagina's?

Hier kon Vasilis geen antwoord op geven, maar misschien werkt dit wel als je gebruik maakt van animaties.

Verder gaf Vasilis nog aan dat we gewoon moeten gaan testen met mensen om zeker te weten of het design goed is. En gaf hij aan dat we het account aanmaken en inloggen helemaal niet hoefden te doen, aangezien dit standaard is en iedereen dit wel weet.

Ook gaf hij nog aan dat de scroll puntjes inprinciepe ook niet nodig zijn. En dan kan je gewoon aan het begin zeggen dat je 5 vragen hebt bijvoorbeeld.

## Code Review #3 - Joost - 10-06-2020

Vraag:
- Hoe kunnen we het beste data opslaan zonder gebruik te maken van een database?

JSON file > FS
LocalStorage
IndexDB

IndexDB icm SW > https://developers.google.com/web/ilt/pwa/live-data-in-the-service-worker


SW Opzetten?

## Design Review #3 - Koop - 11-06-2020

Vraag:
- Feedback scherm media toevoegen

Thomas: vinkje bij buttons als je iets toegevoegd hebt.
Koop: Labels bij buttons, nog kort vertellen wat je kan doen, alle antwoorden blijven altijd zichtbaar, wanneer een stap afgerond wordt er aangegeven dan je ook nog wat anders kan toevoegen.
Wouter / Thomas / Koop: niet duidelijk genoeg waar je nu bent. Koop > aangegeven als titel.


# Debriefing Glory Days

## Inhoudsopgave

* [Introductie opdracht](#Introductie-opdracht)
* [Doel](#Doel)
* [Hoofdvraag](#Hoofdvraag)
* [Deelvragen](#Deelvragen)
* [Core functionality](#Core-functionality)
* [Requirements](#Requirements)
  * [Functional](#Functional)
  * [Non-functional](#Non-functional)
* [MoSCoW](#MoSCoW)
* [Details](#Details)
* [User Scenario](#User-Scenario)

## Introductie opdracht
Wereldwijd zijn er zo’n 50 miljoen Alzheimer patiënten. Ook in Nederland is het veel voorkomend. De ziekte treft mensen van uiteenlopende leeftijden, maar vooral ouderen krijgen ermee te maken. Alzheimer zorgt ervoor dat de hersenen worden aangetast, waardoor de persoonlijkheid vervaagd en herinneringen verdwijnen. Helaas is er op dit moment nog geen geneesmiddel voor deze slopende ziekte. 

Omdat de zoektocht naar een geneesmiddel tot nu toe nog geen succes heeft opgeleverd, hebben farmaceuten ervoor gekozen om alle energie te gaan steken in de preventie in plaats van het genezen.

Ondanks dat een medicijn ver weg is, kunnen we het verloop van de ziekte aangenamer maken met behulp van muziek. Onderzoek heeft namelijk aangetoond dat aan muziek gekoppelde herinneringen op een relatief ‘veilige’ plek in het menselijk brein worden opgeslagen. Het regelmatig beluisteren en beoefenen van muziek heeft een positief effect op de ontwikkeling van Alzheimer. Wanneer muziek persoonlijk en betekenisvol is voor het individu, beloven deze positieve effecten nog groter te worden.


## Doel
Aan ons de opdracht om een applicatie te ontwikkelen waarbij we het leven van Alzheimer patiënten en de mensen eromheen verbeteren. Denk hierbij bijvoorbeeld aan verzorgers, familieleden of vrienden. Muziek, waar persoonlijke herinneringen aan zijn gekoppeld, wordt opgehaald om mooie momenten te creëren in het heden over het verleden. Zo gaan cognitieve functies minder snel achteruit en wordt de emotionele gemoedstoestand mogelijk verbeterd. Daarnaast kunnen we deze ervaring gebruiken om de relatie tussen patiënt en vriend/familie/verzorger te verbeteren.


## Hoofdvraag
Hoe versterk je de band tussen vrienden/familie/verzorgers en Alzheimer patiënten door middel van persoonlijke muziek, gekoppeld aan herinneringen, op te halen en vast te leggen?

## Deelvragen 
- Hoe vinden we persoonlijke muziek bij een Alzheimer patiënt die al in een zodanig vergevorderd stadium zit, dat hij/zij niet meer in staat is om dit zelf aan te geven? 
- Hoe vinden we persoonlijke muziek bij een Alzheimer patiënt waarbij vrienden/familie/verzorgers ook geen idee hebben van wat hij/zij mooie muziek vindt.
- Hoe zorgen we ervoor dat het Spotify account van een verzorger kunnen gebruiken voor de app, zonder dat dit account er door wordt beïnvloed?
  - Account gebruiken van Alzheimer patiënt?
  - Dubbele inlog vermijden
  - Premium account noodzakelijk?
- Hoe kunnen we veilig en vertrouwelijk omgaan met de verzamelde data.
- Hoe zit het met de privacy van de patiënt en hoe gaan wij daarmee om? (AVG)
  - Gaan we ons daar nu op focussen? Bedenken of dit nu noodzakelijk is!
- Hoe kunnen we de Google Home / Amazon Alexa User Voice Interface integreren binnen onze app?
- Hoe kunnen de algoritmes van Spotify gebruikt worden bij het zoeken van muziek?


## Core functionality 
De gebruiker moet via een muziek streaming platform nummers kunnen opzoeken en afspelen. Om deze vervolgens te kunnen liken of op te slaan als herinnering. Bij deze herinneringen kan tekst, audio en foto worden toegevoegd. Deze opgeslagen herinneringen en nummers kunnen terug worden gekeken/geluisterd.

## Requirements
### Functional
- Account aanmaken.
- Inloggen (op Spotify / Apple Music / Deezer).
- Muziek kunnen zoeken.
- Muziek kunnen liken
- Opslaan van muziek als herinneringen.
- Afbeeldingen koppelen aan herinneringen.
- Audio koppelen aan herinneringen.
- Video koppelen aan herinneringen.
- Aanpassen van herinneringen.
- Emoties toevoegen aan herinneringen.
- Playlist samenstellen.
- Muziek zoeken op thema/jaar.
- Suggesties op maat.
- Slide show op ander (tweede) scherm.
- Categorieën van herinneringen / emoties.
- Tijdlijn voor het weergeven van herinneringen 

### Non-functional
- Applicatie moet laagdrempelig zijn.
- Herinneringen moeten snel toegevoegd kunnen worden.

## MoSCoW

| Must have  | Should have | Could have | Won’t have |
| ------------- | ------------- | ------------- | ------------- |
| Account aanmaken | Inloggen (muziekdienst) | Aanpassen herinneringen | Integreren van een VUI |
| Inloggen (account) | Muziek liken | Playlist samenstellen | Suggesties op maat |
| Muziek zoeken | Audio toevoegen aan herinneringen | Video koppelen aan herinneringen | Password hashing / database security |
| Herinneringen aanmaken | Afbeeldingen toevoegen aan herinneringen | Tijdlijn van herinneringen | Voldoen aan de AVG |
| Tekst toevoegen aan herinneringen |  | Categorieën |
| Herinneringen terug kijken |  | Slideshow van herinneringen |

## Details
- Device: Mobiel
- Environment: Voornamelijk binnen, buiten
- Time: Hele dag
- Activity: Zingen, luisteren, dansen
- Individual: Laagdrempelig, simpel, one-primary-action-per-screen
- Location: In een verzorgingstehuis / thuis / tuin
- Social: Vriend/familielid/verzorger en patiënt, inloggen en delen door deze personen

## User Scenario

Wie is de gebruiker?

- Een Alzheimer patiënt, zijn verzorg(st)ers, familieleden en vrienden.

Wat wil de gebruiker 

- Herinneringen opslaan en ophalen door middel van muziek.

Waar gaat de gebruiker het gebruiken?

- Thuis of buiten (in de buurt van het huis).

Waarom wil de gebruiker het gebruiken?

- De band tussen vrienden/familie/verzorger versterken door middel van het ophalen van herinneringen met muziek. Om de emotionele toestand positief te beïnvloeden. Om cognitieve vaardigheden minder hard af te laten glijden.

Hoe gaat de gebruiker het gebruiken?

- De verzorger zal een nummer opzoeken via de telefoon voor de patiënt. De patiënt krijgt optioneel bijbehorende afbeeldingen/video/audio te zien/horen. Bij de juiste muziek kunnen gerelateerde herinneringen op worden gehaald en vast worden gelegd binnen de app. Herinneringen kunnen te allen tijde terug worden gekeken.


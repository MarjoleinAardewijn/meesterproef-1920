# Product Biografie

Tijdens dit project heb ik samen gewerkt met Simone van Zeijl, Marissa Verdonck, Jennifer Slagt en Coen Mathijssen.

## Inhoudsopgave

* [Week 1](#Week-1)
* [Week 2](#Week-2)
* [Week 3](#Week-3)
* [Week 4](#Week-4)
* [Week 5](#Week-5)

## Week 1

In de eerste week hebben we gezamenlijk de [debriefing geschreven](https://github.com/GloryDaysApp/glorydays/wiki/Debriefing), [schetsen gemaakt](https://github.com/GloryDaysApp/glorydays/wiki/Schetsen) en een [MoSCoW diagram opgezet](https://github.com/GloryDaysApp/glorydays/wiki/MoSCoW).

Om goed samen te kunnen werken maakte wij gebruik van GitHub. Aangezien ik de enigste persoon was in mijn team die ervaring had met in teamverband met Git werken heb ik tutorial aan mijn teamgenoten gegeven over hoe je kan werken met Git in teamverband. Hierbij heb ik de uitgelegd hoe je met branches moet werken, aangezien velen hier nog nooit mee gewerkt hadden, en hoe je git kan gebruiken via de terminal. Om mijn teamgenoten hierbij verder te helpen heb ik ook een [cheatsheet](https://github.com/GloryDaysApp/glorydays/wiki/GIT) gemaakt met allemaal git commando's voor de terminal.

En als laatste heb ik ook nog een artikel gelezen over of je kan meten hoeveel muziek je opwindt en hier een [samenvatting](https://github.com/GloryDaysApp/glorydays/wiki/Onderzoek#4-can-you-measure-how-much-music-excites-you-i-tried-heres-the-data) over geschreven.

## Week 2

In de tweede week hebben we weer veel overlegt en schetsen gemaakt (Zie: [Product Biografie Glory Days > pagina 31 'Extra features schetsen'](https://docs.google.com/document/d/1WLTZvnaozR54mjuEAJiTyEz7x85r8EW1nZUzlMftpLw/edit)) aan de hand van de feedback die wij terug kregen van Marnix en Martijn. 

Samen met Marissa en Jennifer hebben we de eerste opzet gemaakt van de conversational UI. En heb ik mij vooral gefocused op het kunnen uploaden van afbeeldingen. In HTMl heb je al een speciaal input field hiervoor met het als type `file`. Hierdoor heb je automatisch al de functionaliteit dat wanneer je op de button klikt je een document kan kiezen. Maar aangezien deze input lastig te stylen is met CSS heb ik ervoor gezorgd dat de functionaliteit van de input doorgegeven wordt aan een normale button die wel mooi gestyled kan worden. Dit heb ik gedaan door de originele input te verbergen en met JavaScript de functionaliteit van de originele button aan een normale button mee te geven. Hierdoor lijkt het alsof je op de normale button klikt, maar eigenlijk is het gewoon de originele input met type `file` die uitgevoerd word.

<details>
    <summary>Code</summary>

add-memory.ejs
```html
<div class="input-button--choose-file">
    <input type="file" id="real-file" hidden="hidden" name="image-upload" />
    <button type="button" id="custom-button" class="button button-add-image">
        Kies een afbeelding
    </button>
    <span id="custom-text">Nog geen afbeelding gekozen.</span>
</div>
<img id="output-image" class="output-image hide" />
```

custom_image_upload.js
```javascript
customButton.addEventListener('click', () => {
    realFileButton.click();
});
```
</details>

Om de afbeelding die gekozen is ook echt weer te geven in de conversational UI heb ik in  de HTMl een `img` element toegevoegd en deze de class `hide` meegeven zodat deze in eerste instantie niet zichtbaar is. Daarnaast heb ik ook nog een `span` toegevoegd waarin de naam van de afbeelding komt te staan.

Om ervoor te zorgen dat de afbeelding en text zichtbaar worden heb ik een `addEventLintener` op de originele input (`realFileButton`) gezet met een `change` event erop. Hierdoor gaan hij de code in de eventLintener uitvoeren wanneer er een afbeelding gekozen word en dus een verandering plaatsvind in de value van de `realFileButton`.

```javascript
realFileButton.addEventListener('change', (event) => {
    ...
});
```

Wanneer `realFileButton` een value heeft haalt hij bij de naam van de afbeelding die gekozen is de path die er nog voor staat weg (`C:\fakepath\`) zodat alleen de naam en extensie van de gekozen afbeelding nog zichtbaar zijn.

```javascript
customText.textContent = realFileButton.value.replace(/^.*[\\\/]/, '');
```

Nadat de naam is aangepast, verwijder ik de class `hide` van het `img` element en voeg ik er de class `show` aan toe zodat de afbeelding ook zichtbaar word.

```javascript
outputImage.classList.remove('hide');
outputImage.classList.add('show');
```

Echter zorgd dit er nog niet voor dat de afbeelding daarwerkelijk zichtbaar is. Om dit voorelkaar te krijgen heb ik door gebruik te maken van de methode `URL.createObjectURL` een tijdelijke URL gegenereerd die aan de `src` tag van het `img` element wordt toegevoegd.

```javascript
outputImage.src = URL.createObjectURL(event.target.files[0]);
```

En om ervoor te zorgen dat de zojuist gegenereerde URL niet in het geheugen blijft staan verwijder ik deze weer uit het geheugen van de browser wanneer de afbeelding geladen wordt.

```javascript
outputImage.onload = () => {
    URL.revokeObjectURL(outputImage.src); // free memory
};
```

<details>
    <summary>Gehele code</summary>

add-memory.ejs
```html
<div class="input-button--choose-file">
    <input type="file" id="real-file" hidden="hidden" name="image-upload" />
    <button type="button" id="custom-button" class="button button-add-image">
        Kies een afbeelding
    </button>
    <span id="custom-text">Nog geen afbeelding gekozen.</span>
</div>
<img id="output-image" class="output-image hide" />
```

custom_image_upload.js
```javascript
const realFileButton = document.getElementById('real-file'),
    customButton = document.getElementById('custom-button'),
    customText = document.getElementById('custom-text'),
    outputImage = document.getElementById('output-image');

realFileButton.addEventListener('change', (event) => {
    if (realFileButton.value) {
        // show file name
        customText.textContent = realFileButton.value.replace(/^.*[\\\/]/, '');

        // show chosen image
        outputImage.classList.remove('hide');
        outputImage.classList.add('show');

        outputImage.src = URL.createObjectURL(event.target.files[0]);
        outputImage.onload = () => {
            URL.revokeObjectURL(outputImage.src); // free memory
        };
    } else {
        // if no file is chosen, reset everything to default values
        customText.textContent = 'Nog geen afbeelding gekozen.';
        outputImage.src = '';
        outputImage.classList.remove('show');
        outputImage.classList.add('hide');
    }
});
```

_global.scss
```scss
.hide {
    display: none;
}

.show {
    display: block;
}
```

_add_media.scss
```scss
.conversational-ui {
    form {
        fieldset {
            .input-button {
                &--choose-media {
                    z-index: 999;
                    margin-left: $indent;
                }

                &--choose-file {
                    display: flex;
                    flex-direction: column;

                    span {
                        margin-top: $indent-xxxs;
                        text-align: left;
                        margin-left: $indent-xl;
                    }
                }
            }

            .buttons--choose-media {
                display: flex;
            }

            .output--add-image {
                display: none;

                &.active {
                    display: block;
                    margin-left: $indent;
                    margin-right: $indent;
                    width: 80vw;
                    position: absolute;
                }

                .output-image {
                    max-width: 250px;
                    max-height: 200px;
                    margin-top: $indent;
                    border-radius: 4px;
                    right: 0;
                    position: absolute;
                }
            }
        }
    }
}
```
</details>

Naast de code schrijven voor het uploaden van afbeeldingen heb ik ook nog bij Simone en Coen meegekeken met hoe je sockets in een aparte module kan zetten. Echter is dit ons niet gelukt en besloot ik om dit tijdens de code review van die week aan Laurens te vragen. Hij gaf mij een link naar de repo van Lennart, die bij het vak Real Time Web (RTW) had gedaan. Dit heb ik weer naar Simone en Coen doorgespeeld, maar tegen die tijd hadden ze al besloten om voor nu geen sockets te gebruike.

<details>
    <summary>Links naar voorbeeld code van Lennart</summary>

[Sockets module](https://github.com/lennartdeknikker/wiki-game/blob/828c6b3524a479c2d1bc98a0caf7d62620dc94d9/server/scripts/socket.js) \
[importeerd module hier](https://github.com/lennartdeknikker/wiki-game/blob/5d7821d94ea210f6970651d4018498a0b59e5256/bin/www)
</details>

Ook heb ik deze week veel geholpen bij de eerste keren committen van mijn teamgenoten via git in de terminal. Hier ging vaak ook nog best veel tijd inzitten, aangezien sommige dingen toch nog iets beter uitgelegt moesten worden.

Tijdens het coachinggesprek met Joost gaf hij ons ook nog de tip om een aparte repo aan te maken voor Glory Days, zodat de overdracht later veel makkelijk zou gaan. Om deze reden heb ik toen een email account aangemaakt voor Glory Days om vervolgens een Github account en een nieuwe repo aan te kunnen maken. Alle wachtwoorden heb ik netjes gedocumenteerd, zodat dit allemaal aan het einde van het project makkelijk overgedragen kan worden aan Marnix en Martijn. Daarna heb ik nog een nieuw projectboard aangemaakt en alle issues overgezet en meteen mooi van labels verzien zodat ook dit allemaal wat overzichtelijker voor ons wordt.

Verder heb ik deze week ook nog linters voor SCSS en JS ingesteld op het project zodat al onze code zo veel mogelijk consistent blijft. Hiervoor heb ik eerst uitgezocht welke linters het fijnst zouden zijn om mee te werken. Uit dit kleine onderzoek en de test die ik daarna heb gedaan om te kijken of de gekozen linter ook lekker werkt, kwam ESLint voor JS en StyleLint voor SCSS.

Nadat ik de linters via NPM had geïnstalleerd en had ingesteld heb ik ook nog [alle regels die we hadden besproken en (gedeeltelijk) opgenomen zijn in de linters gedocumenteerd in de Wiki](https://github.com/GloryDaysApp/glorydays/wiki/Code-Regels#Linters).

<details>
    <summary>Linters</summary>

ESLint:
.eslintrc.json
```json
{
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": [
        "eslint:recommended"
    ],
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaVersion": 11,
        "sourceType": "script",
        "ecmaFeatures": {
            "globalReturn": true
        }
    },
    "rules": {
        "indent": [
            "error",
            4
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "single"
        ],
        "semi": [
            "error",
            "always"
        ]
    }
}
```

StyleLint:
.stylelintrc.json
```json
{
    "extends": "stylelint-config-sass-guidelines",
    "rules": {
        "indentation": 4
    }
}
```
</details>

## Week 3

Deze week ben ik begonnen met het mergen van mijn bench met die van Marissa. Dit ging zonder grote problemen, er waren wat merge conflicts maar die waren makkelijk op te lossen. Marissa heeft hierbij meegekeken, aangezien zij dit nog nooit gedaan had. Om deze reden heb ik het haar ondertussen ook een beetje uitgelegd en vragen van haar beantwoord.

Tijdens alle merges die deze week gedaan werden kwam het probleem naar voren dat er steeds conflicten waren in `package-lock.json`. Aangezien dit een bestand is dat bij iedereen weer anders is, omdat het automatisch gegenereerd wordt, heb ik deze in `.gitignore` gezet om conflicten in de toekomst te voorkomen. Daarnaast heb ik ook meteen alle naamgevingen in het project van bestanden consistent gemaakt, aangezien dit ondertussen een beetje een rommeltje werd. Hiervoor heb ik met mijn team overlegd hoe we de naamgevingen willen doen en aan de hand daarvan het aangepast. Ook heb ik dit meteen gedocumenteerd en daarbij ook meteen alle andere [code regels](https://github.com/GloryDaysApp/glorydays/wiki/Code-Regels) opgeschreven zodat dit soort dingen in de toekomst ook voorkomen kunnen worden.

Verder kwamen er nog een aantal errors zijn voren bij ESLint die voor ons eigenlijk niet zo belangrijk waren, dus heb ik deze regels in overleg met het team uitgezet. Dit om onnodige errors te voorkomen. Ditzelfde probleem ondervonden we ook met Stylelint, dus ook hier heb ik een aantal regels uitgezet.

<details>
    <summary>Linters</summary>

ESLint:
.eslintrc.json
```json
{
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": [
        "eslint:recommended"
    ],
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaVersion": 11,
        "sourceType": "script",
        "ecmaFeatures": {
            "globalReturn": true
        }
    },
    "rules": {
        "indent": [
            "error",
            4
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "single"
        ],
        "semi": [
            "error",
            "always"
        ],
        "no-undef": "off",
        "no-unused-vars": "off",
        "no-useless-escape": "off"
    }
}
```

StyleLint:
.stylelintrc.json
```json
{
    "extends": "stylelint-config-sass-guidelines",
    "rules": {
        "indentation": 4,
        "max-nesting-depth": null,
        "selector-max-compound-selectors": null,
        "order/properties-alphabetical-order": null,
        "selector-no-qualifying-type": null,
        "number-leading-zero": null,
        "property-no-vendor-prefix": null,
        "selector-max-id": null
    }
}
```
</details>

Ook heb ik nog met teamgenoten mee gekeken met de eerste keer committen. En aangezien dit mij steeds veel tijd kost heb ik daar meteen een mooie [guide](https://github.com/GloryDaysApp/glorydays/wiki/Git-Guide-voor-Terminal) voor gemaakt, zodat ze deze bij toekomstige commits kunnen raadplegen en alleen mij om hulp hoeven te vragen wanneer ze er echt niet uit komen. 
 
## Week 4

Week 4 zijn we begonnen met het maken van een goed design, zodat we dit bij het schrijven van SCSS goed konden aanhouden en ook consistent daarin konden zijn. Hiervoor hebben we eerst weer een aantal schetsen gemaakt (Zie: [Product Biografie Glory Days > pagina 52 'Visueel ontwerp schetsen'](https://docs.google.com/document/d/1WLTZvnaozR54mjuEAJiTyEz7x85r8EW1nZUzlMftpLw/edit)) en hieruit weer een aantal keuzes gemaakt. Daarna ben ik begonnen met het maken van het definitieve prototype. Dit heb ik in eerste instantie gedaan met Adobe XD, maar al snel bleek dat dit niet al te handig was aangezien ik een nieuwere versie van XD had dan mijn teamgenoten. Dit ben ik verder gegaan in [Figma](https://www.figma.com/file/gROxPQAopa290sGWgStval/Glory-Days-V3). Doordat ik het ook in XD had gemaakt, kon ik wel meteen mooi de style guides genereren zodat dit voor iedereen duidelijk was. Later zijn ook de anderen uit het team komen helpen, aangzien het erg veel tijd koste om het allemaal in mijn eentje te maken.

Na het maken van het design heb zag ik dat op de `master` bench nog veel Stylelint errors zaten, dus heb ik deze allemaal weggewerkt. En heb ik in overleg met mijn teamgenoten variabelen aangemaakt voor alle afstanden voor `paddings` en `margins` die wij gebruiken. Dit ook om ervoor te zorgen dat alles zo consistent mogelijk bleef en dat als er ooit iets aan moet veranderen, we dat mooi op 1 plek kunnen doen in plaats van op 100 plekken.

Ook heb ik deze week het mediascherm van de conversational UI gemaakt. Dit is te zien in de bench: [dev_62_memory_images_voice_and_text](https://github.com/GloryDaysApp/glorydays/tree/dev_62_memory_images_voice_and_text). Hier ben ik nog best wel een tijdje mee bezig geweest, aangezien veel SCSS-code door elkaar stond en dit de styling van de buttons met icons erin erg lastig maakte. Daarom heb ik eerst uitgezocht hoe het allemaal in elkaar zat, aangezien Marissa die code grotendeels had geschreven en het vrij complex in elkaar zat door alle animaties die erin zaten. Toen ik een idee had van hoe het in elkaar zat heb ik de code die ik eruit kon halen wat betreft de buttons eruit gehaald en in een eigen bestand gezet. Daarnaast heb ik ook nog een aantal mixins gemaakt, aangezien veel code steeds terugkwam en je dit eigenlijk liever niet wilt in verband met onderhoudbaarheid in de toekomst en dus in zo'n geval een mixin wilt gebruiken. Nadat ik dat allemaal had gedaan kon ik eindelijk beginnen met het stylen van de het scherm en de buttons. In eerste instantie heb ik toen aparte pagina's gemaakt voor afbeeldingen, voice of tekst toevoegen. Maar na kort overleg met mijn team, hebben we besloten dit op dezelfde pagina te doen. Dus moest ik mijn code nog iets aanpassen en nog wat JavaScript schrijven om ervoor te zorgen dat de elementen alleen maar zichtbaar waren wanneer er op een button geklikt is.

Nadat ik met het media scherm klaar was heb ik Jennifer even geholpen met het de sliders. Ik had bij [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/range) gelezen dat je ook een `datalist` element kan gebruiken in conbinatie met een input field type `rage` om tickmarkt toe te voegen (dit wilde we hebben om gebruikers een beter indicatie te geven van wat ze hebben ingevult op de slider). Dus hebben we uitgeprobeerd of dit werkt. Helaas kwamen wij tot de conclusie dat dit niet mogelijk is wanneer je de slider een custom styling hebt gegeven. Dit kwam door dat je daarvoor de CSS propertie `appearance: none` moest gebruiken en dit er ook voor zorgt dat de tickmarks verdwijnen. Jennifer is daarna zelf weer opzoek gegaan naar een andere mogelijke oplossing, dit omdat ik toen zelf weer verder moest met mijn eigen taken.

Na het helpen van Jennifer ben ik begonnen met het maken van [navigation states](https://github.com/GloryDaysApp/glorydays/tree/dev_79_navigation_states). We moesten namelijk een manier vinden om op de ene pagina alleen een navigatie te laten zien en op een andere weer alleen een muziek player en weer op een andere de mogelijkheid om beide te zien. Hiervoor moest een scriptje geschreven worden. Ik heb ervoor gekozen om een aantal standaard classes aan te maken: 

- `navigation-state--nav-only`
- `navigation-state--player-only`
- `navigation-state--all`

En met een script te kijken welke class er op de pagina aanwezig is en aan de hand daarvan de juiste navigatie in te laten. Hiervoor moest ik wel een placeholder aanmaken voor onderaan de pagina om alle navigaties in te zetten. En wanneer zowel de navigatie als de muziek player zichtbaar moeten zijn moest ik er wat op vinden dat de muziek player mooi boven de navigatie kwam te staan. Dit heb ik opgelost door met JavaScript te berekenen hoe hoog de navigatie is en deze hoogste als `bottom` waarde aan de muziek player mee te geven.

<details>
    <summary>Placeholder code</summary>

navigation-holder.ejs
```html
<div class="holder--navigation-and-player" id="holder--navigation-and-player">
    <%- include('./spotify-navigation-player') %>
    <%- include('./navigation') %>
</div>
```
</details>

<details>
    <summary>Script</summary>

navigation_states.js
```js
const navigation = document.getElementById('navigation'),
    navigationList = document.getElementById('navigation-list'),
    navigationPlayer = document.getElementById('spotify--navigation-player'),
    onlyNavigation = document.getElementById('navigation-state--nav-only'),
    onlyNavigationPlayer = document.getElementById('navigation-state--player-only');

if (onlyNavigation) {
    // show navigation only
    if (navigationPlayer) {
        navigationPlayer.classList.add('hide');
    }
} else if(onlyNavigationPlayer) {
    // show navigation player only
    if (navigation) {
        navigation.classList.add('hide');
    }
} else {
    // show navigation and navigation player
    if (navigationPlayer && navigationList) {
        const navHeight = navigationList.offsetHeight;
        navigationPlayer.style.bottom = `${navHeight}px`;
    }
}
```
</details>

Na het maken van de navigation states, heb ik samen met Simone een script geschreven die automatisch doorscrolled naar de volgende vraag, wanneer een vraag beantwoord is. Dit was nog een heel gedoe om te bedenken van hoe we dit het beste konden doen. Uiteindelijk hebben we ervoor gekozen om de classes van de naviagtion dots en fieldsets aan de hand van een `change` event op de checkboxes of textfields aan te passen naar active en normaal (fieldsets zonder active zijn hidden by default). Ook moesten we om dit goed voor elkaar te krijgen kijken naar hoe Marissa precies de navigation dots en visibility van de fieldsets had gemaakt.

<details>
    <summary>Script</summary>

bench: [scroll_event](https://github.com/GloryDaysApp/glorydays/tree/scroll_event)

scroll_next_question.js
```js
// Input checkbox
const inputCheckbox = document.querySelector('input[type="checkbox"]');
const checkboxes = document.getElementById('checkboxes');

// Input text
const inputText = document.querySelectorAll('input[type="text"]');
const textFields = document.querySelectorAll('.input-field');

// Current fieldset
const currentFieldset = document.getElementsByClassName('active-fieldset');
const currentDot = document.getElementsByClassName('active-navigation-dot');

if (checkboxes) {
    checkboxes.addEventListener('change', checkboxChecked);
}

// Function to check if checkbox is checked, and go to the next question
function checkboxChecked() {
    const checkbox = event.target;
    const checked = checkbox.checked;

    if (checked) {
        setTimeout(() => {
            changeActiveState();
        }, 1000);
    }
}

if (textFields) {
    for (let i = 0; i < textFields.length; i++) {
        textFields[i].addEventListener('change', nextQuestion);
    }
}

// Function to check if input text has a value, and go to the next question
function nextQuestion(e) {
    const target = e.target || e.srcElement;
    const targetValue = target.value;

    if (targetValue != '') {
        setTimeout(() => {

            changeActiveState();

        }, 1000);
    }
}

function changeActiveState() {
    // Next fieldset
    currentFieldset[0].nextElementSibling.classList.remove('hidden-fieldset');
    currentFieldset[0].nextElementSibling.classList.add('active-fieldset');

    // Current fieldset
    currentFieldset[0].classList.add('hidden-fieldset');
    currentFieldset[0].classList.remove('active-fieldset');

    // Navigation dot sidebar
    currentDot[0].nextElementSibling.classList.add('active-navigation-dot');
    currentDot[0].classList.remove('active-navigation-dot');
}
```
</details>

Verder heb ik deze week ook nog met Coen samen if statements om EventsListeners en variabelen gezet om onnodige errors op pagina's waar deze elementen niet voorkomen te voorkomen en hebben we als team alle brenches samengevoegd en allemaal individueel de app getest. Wanneer er nog errors of bugs gevonden werden, werden deze meteen genoteerd en nadat we het met zijn alle hadden besproken hebben we hoervan nieuwe issues aangemaakt in het project board zodat we wisten wat er allemaal nog gedaan moest worden.

En als laatste heb ik nog een color scheme voor de overlays bedacht. Dit was moeilijker dat je zou denken, eengezien het duidelijke kleuren moesten zijn die goed de emoties en energielevels weer konden geven. Na een aantal scheme's te hebben geprobeerd, opgezocht te hebben welke kleuren wat betekenen in de psychologie en feedback aan mijn team te hebben gevraagd en zij ook meegedacht hadden, hadden we uiteindelijk 3 verschillende scheme's die we aan Marnix en Martijn konden voorleggen. Zij hebben toen uiteindelijk voor de volgende color scheme gekozen voor de overlays:

<img width="826" alt="Definitief color scheme" src="https://user-images.githubusercontent.com/23479038/85858255-9e404180-b7bb-11ea-9bba-4aa6d00499d2.png">

<details>
    <summary>Andere Color Scheme's</summary>

Tests met verschillene color scheme's \
<img width="395" alt="Alle color scheme ideeën" src="https://user-images.githubusercontent.com/23479038/85858485-fecf7e80-b7bb-11ea-98e1-e958917aab1a.png">

<img width="1317" alt="tests met verschillende color scheme's" src="https://user-images.githubusercontent.com/23479038/85858308-b31cd500-b7bb-11ea-80b0-7ca1f862c6e5.png">

Kleur Psychologie \
<img width="206" alt="color psychology" src="https://user-images.githubusercontent.com/23479038/85858476-fd05bb00-b7bb-11ea-9327-c528b66a2327.png">

Uiteindelije top 3 \
<img width="482" alt="top 3 color scheme's" src="https://user-images.githubusercontent.com/23479038/85858287-ab5d3080-b7bb-11ea-9c92-e08b631073f6.png">

</details>

## week 5

De laatste week ben ik begonnen met de color scheme die uitgekozen was voor de overlays toe te voegen aan het project. In het project had Jennifer al eerder overlays toegevoegd, dus aan mij was nu de taak om dit aan te passen.
Om dit te doen heb ik eerst een mixin gemaakt van de gradient, aangezien deze op veel plekken terugkwam met alleen maar een andere kleur erin. Aangezien je aan mixins ook variabelen mee kan geven heb ik daar dus meteen gebruik van gemaakt.

<details>
    <summary>Mixin</summary>

_mixins.scss
```scss
@mixin overlay-gradient($color) {
    background: linear-gradient($color 80%, $color) 10%;
}
```
</details>

Daarna heb ik alle kleuren van de overlay die nog niet bij de colors stonden toegevoegd en de namen van de kleuren een zo aangepast dat het nu allemaal 'glory' kleuren zijn.

<details>
    <summary>Glory kleuren</summary>
    
_colors.scss
```scss
$glory-green: #439d9b;
$glory-blue: #9bd3d4;
$glory-grey: #bababa;
$glory-salmon: #d56a5c;
$glory-red: #c91f08;
```
</details>

Verder heb ik deze week de app gedeployed naar Heroku zodat het ook [live](https://glory-days.herokuapp.com/) te testen is. En heb ik nog een PWA ervan gemaakt door een manifest en een service worker eraan toe te voegen.
Dit zodat het de look en feel krijgt van een app en het in de toekomst ook als app te testen is met de doelgroep zonder dat we er een Android of iOS app van hoefden te maken. In de service worker heb ik ervoor gekozen om voor nu 
alleen de CSS en JS te cachen, aangezien er niet genoeg tijd was om andere pagina's ook te laten cachen. Ik heb nog geprobeerd om een offlinestate erin te maken, maar door de tijdsdruk en error's heb ik voor nu besloten dat niet te doen en dus de `cache only strategy` toe te passen.

Ook heb ik nog geprobeerd om een revision manifest erin te maken zodat de gecachde CSS en JS niet meer gebruikt worden 
wanneer er een nieuwe versie van bestaat op de server. En dus de laatste versie altijd mooi zichtbaar is. Alleen 
ondervond Coen hier problemen mee op zijn computer, waardoor we ervoor gekozen hebben om ook dit voor nu te laten en 
mogelijk een andere keer naar te kijken. Dit ook omdat ik nu niet genoeg tijd meer had om uit te zoeken waarom het 
bij mij wel en bij hem niet werkte.

<details>
    <summary>Service Worker</summary>

```js
const CORE_CACHE_VERSION = 2,
    CORE_CACHE_NAME = `core-v${CORE_CACHE_VERSION}`,
    CORE_ASSETS = [
        '/index.css',
        '/index.js'
    ];

self.addEventListener('install', event => {
    console.log('Installing Service Worker');

    event.waitUntil(
        caches.open(CORE_CACHE_NAME)
            .then(cache => cache.addAll(CORE_ASSETS))
    );
});

self.addEventListener('activate', event => {
    console.log('Activating Service Worker');
    event.waitUntil(clients.claim());
});

self.addEventListener('fetch', event => {
    console.log('Fetch Event: ', event.request.url);
    if (isCoreGetRequest(event.request)) {
        console.log('Core Get Request: ', event.request.url);
        // cache only strategy
        event.respondWith(
            caches.open(CORE_CACHE_NAME)
                .then(cache => cache.match(event.request.url))
        );
    }
});

/**
     * Checks if a request is a core GET request
     *
     * @param {Object} request        The request object
     * @returns {Boolean}            Boolean value indicating whether the request is in the core mapping
     */
const isCoreGetRequest = (request) => {
        return request.method === 'GET' && CORE_ASSETS.includes(getPathName(request.url));
    },

    /**
     * Get a pathname from a full URL by stripping off domain
     *
     * @param {Object} requestUrl        The request object, e.g. https://www.mydomain.com/index.css
     * @returns {String}                Relative url to the domain, e.g. index.css
     */
    getPathName = (requestUrl) => {
        const url = new URL(requestUrl);
        return url.pathname;
    };
```
</details>

En om het manifest compleet te maken heb ik ook nog een logo gemaakt voor Glory Days, zodat deze mooi hierin (en als favicon) verwerkt kon worden.

<details>
    <summary>Manifest</summary>

```json
{
  "name": "Glory Days",
  "short_name": "Glory Days",
  "description": "Preserve and share your loved one's memories",
  "theme_color": "#9bd3d4",
  "background_color": "#ffffff",
  "display": "standalone",
  "Scope": "/",
  "start_url": "/?source=pwa",
  "icons": [
    {
      "src": "images/icons/icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png"
    },
    {
      "src": "images/icons/icon-96x96.png",
      "sizes": "96x96",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-384x384.png",
      "sizes": "384x384",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```
</details>

Ook was er nog een bug met de spacing doordat we op sommige plekken nog `em` gebruikte in plaats van `rem`. Wij hebben ervoor gekozen om `rem` te gebruiken, omdat deze relatief is aan de html (root) lettergrootte wat bij ons `16px` is en `em` relatief is aan de lettergrootte van de directe of dichtstbijzijnde parent en dit dus betekend dat deze waardes steeds kunnen verschillen afhankelijk van waar ze gebruikt worden.
Doordat we dus nog `em` gebruikte was de spacing op sommige plekken nog niet goed en moest dit dus veranderd worden naar `rem`. Ik ben de hele code doorgeggaan en heb overal `em` veranderd naar `rem`.

Verder heb ik ook nog een bug met de voice recorder opgelost. Hier was namelijk het probleem dat en voor de timer `00` 
werd gebruikt als beginwaarde, maar aangezien dit geen `string` was gaf dit steeds de error dat er geen octal literals 
in strict mode gebruikt mochten worden. Dit heb ik eerst geprobeerd op te lossen door dit in de linter uit te zetten, 
alleen kwam toen de error ook in de console erbij. Uiteindelijk heb ik dit opgelost door quotes eromheen te zetten, 
zodat het een `string` was en geen `interger`. En dit werkte ook prima.


Ook was er nog een bug met dat wanneer de navigation zowel de muziek player als de navigatie bevatte de holder, ook wanneer de muziek player niet actief was de ruimte innam alsof er wel een muziek player was. Omdat dit problemen gaf met de positionering met de buttons erboven (deze hadden dan een te grote ruimte tussen de button en de naviagtie wanneer en geen muziek player was) moest hier een oplossing voor gevonden worden.
Om dit op te lossen heb ik het zo gemaakt dat wanneer je beide op een pagina wilt tonen, je in eerste instantie toch gewoon de id `navigation-state--nav-only` op de pagina moet hebben staan. Wanneer de muziek player dan actief wordt, wordt de id veranderd naar `navigation-state-all` zodat ook de muziek player zichtbaar is. En op deze manier is ook het probleem met de witruimte tussen de buttons en de navigatie opgelost.

<details>
    <summary>Code</summary>

spotify-player.js
```js
...
const mainContainer = document.getElementsByClassName('container--main');

...

function showPlayerSmall(song) {
    // Make player visible
    musicPlayer.classList.add('visible');
    // Remove id for showing only the nav and add id for showing nav + music player
    mainContainer[0].removeAttribute('id');
    mainContainer[0].setAttribute('id', 'navigation-state--all');
    
...
```
</details>

Naast de bovenstaande dingen heb ik ook nog de slider toegevoegd aan de schermen voor het toevoegen van een herinnering, de link van de button 
'open de muziekspeler' veranderd van `/` naar `/music-overview` en margins onderaan de pagina's toegevoegd zodat elementen beter zichtbaar waren op mobiele devices.

Ook heb ik nog een redirect gemaakt naar de login pagina wanneer de gebruiker niet ingelogd is. Dit heb ik gedaan door op de beginpagina een check te doen of er een `REFRESH_TOKEN` aanwezig is. Wanneer dit niet het geval is wordt de gebruiker doorgestuurd naar de login pagina van Spotify.

<details>
    <summary>Code redirect naar login pagina</summary>

server.js    
```js
...

.get('/memories-overview', async (req, res) => {
    if (!req.cookies.REFRESH_TOKEN) {
        res.redirect('/login');
    } else {
        if (req.cookies.ACCESS_TOKEN) {
            Memory
                .find({})
                .then((data) => {
                    console.log('data ', data);
                    router.pageWithData(res, 'memories-overview', 'Herinneringen', data, revManifest);
                })
                .catch((err) => {
                    console.log('couldnt get memories from database', err);
                });
            
        } else {
            getRefreshToken(req, res).then(() => {
                Memory
                    .find({})
                    .then((data) => {
                        console.log('data ', data);
                        data = JSON.stringify(data);
                        router.pageWithData(res, 'memories-overview', 'Herinneringen', data, revManifest);
                    })
                    .catch((err) => {
                        console.log('couldnt get memories from database', err);
                    });
            });
        }
    }
})
    
...
```
</details>

En heb ik Coen nog geholpen met data in de DB te zetten en er weer uit te halen. Hij liep hier namelijk een beetje vast en had ff een frisse blik erbij nodig.

Als laatste heb ik de [README](https://github.com/GloryDaysApp/glorydays#readme) geschreven. Hierbij heb ik nog heel leuk een plugin gebruikt genaamd [all-contributors](https://github.com/all-contributors/all-contributors) waarmee je heel makkelijk kan aangeven wie er allemaal meegewerkt hebben aan het project en wat zij hebben gedaan. En een splashscreen gemaakt voor de app.

<details>
    <summary>Splashscreen</summary>

<img width="375" alt="Screenshot 2020-06-26 at 16 15 58" src="https://user-images.githubusercontent.com/23479038/85866872-5d9af500-b7c8-11ea-99bc-db16b1579058.png">
</details>
# Reflectie Op Eigen Niveau

Nu we aan het einde zijn gekomen van het project Glory Days van de Meesterproef zal ik systematisch op mijn werk en 
proces reflecteren. Aan de hand van de vak-rebrics zal ik schrijven welke vakken wel of niet aan bod zijn gekomen en waarom.
Op deze manier krijg ik een goed beeld van mijn eigen niveau, mogelijke aandachtspunten in techniek, interactie en/of 
aspecten van hetdesign-proces waar ik me nog in zou kunnen verbeteren.

## Inhoudsopgave

* [Samenwerking Team](#Samenwerking-Team)
* [Vak Rubrics](#Vak-Rubrics)
  * [Web Apps From Scratch (WAFS)](#Web-Apps-From-Scratch-WAFS)
    * [Criteria](#Criteria)
    * [Reflectie](#Reflectie)
  * [CSS To The Rescue (CSSTR)](#CSS-To-The-Rescue-CSSTR)
  * [Progressive Web Apps (PWA)](#Progressive-Web-Apps-PWA)
    * [Criteria](#Criteria)
    * [Reflectie](#Reflectie)
  * [Browser Technologies (BT)](#Browser-Technologies-BT)
    * [Criteria](#Criteria)
    * [Reflectie](#Reflectie)
  * [Real Time Web (RTW)](#Real-Time-Web-RTW)
    * [Criteria](#Criteria)
    * [Reflectie](#Reflectie)
  * [Web Design (WD)](#Web-Design-WD)
    * [Criteria](#Criteria)
    * [Reflectie](#Reflectie)

## Samenwerking Team

De samenwerking ging echt super tijdens dit project! Ik heb echt nog nooit in zo'n fijne groep gewerkt als nu. Vaak had 
ik tijdens projecten nog wel problemen binnen het team met mensen die niets deden of dat we steeds heftige discussies 
hadden waar veel tijd aan verloren ging, maar dat was allemaal niet het geval tijdens dit project. Iedereen hielp elkaar
goed en luisterde ook goed naar wat anderen te zeggen hadden. En hierdoor werden beslissingen ook snel gemaakt. We zaten
bijna altijd wel met zijn allen op één lijn, wat echt superfijn was!

Ook leerden we veel van elkaar. Zo heb ik iedereen geleerd hoe je met Git kan werken via de terminal en met brenches en 
hebben de anderen mij weer veel geleerd van hoe Spotify werkt en kleine dingetjes tussendoor waar ik nog niets van wist.

Daarnaast heb ik ook veel geleerd van het pair programming. Zo heb ik geleerd hoe anderen programmeren en wat andere manieren 
zijn om dingen aan te pakken. Daarnaast was het vaak dat dan de andere typete, waardoor ik zelf ook echt super goed moest
nadenken van hoe het geschreven moest worden. Vooral van dit laatste heb ik ontzettend veel geleerd.

Ook heb ik geleerd dat je bij Git ook een project board kan maken waar je je issues in kan zetten. Wat echt een eye opener 
voor mij was. Het was echt superfijn dat je hiervoor geen Trello of een ander soortgelijk programma hoeft te gebruiken 
en dat alles gewoon mooi op één plek staat.

Ook heb ik doordat ik iedereen moest uitleggen hoe Git werkt, zelf ook weer nieuwe dingen bijgeleerd. Zo gebruikte ik eerst
eigenlijk nooit de terminal op de website van Github om merge conflicten op te lossen en gebruikte ik ook nooit `git stash` 
om code even weg te halen, master te pullen en dan weer terug te zetten. Dit heb ik tijdens dit project veel moeten doen.
En waren er vaak verschillende errors bij mijn teamgenoten met Git wat mij ook steeds vlink aan het denken zette. Ook hiervan 
heb ik erg veel geleerd!

## Vak Rubrics

In dit deel zal ik per vak aangegeven welke criteria van de rubrics ik heb toegepast in het project en 
mijn reflectie aan de hand van de criteria.

### Web Apps From Scratch (WAFS)

#### Criteria

App structuur en code kwaliteit
- De code bevat geen syntaxfouten en is netjes opgemaakt
- Code is correct gescoped en opgedeeld in modules die overeen komen met het actor diagram.

#### Reflectie

Om ervoor te zorgen dat code consistent is heb ik een linter opgezet die alle regels waaraan onze code moest voldoen 
controleerde. Dit was de eerste keer dat ik linters had geconfigureerd en eigenlijk was dit best makkelijk. Ik had 
namelijk gedacht dat dit best lastig zou zijn en een behoorlijke uitdaging zou worden, maar achteraf was het eigenlijk 
best simpel. Wel wilde ik soms iets te snel aan de slag gaan, terwijl ik dan eigenlijk beter nog iets verder de 
documentatie had kunnen lezen. Hierdoor liep ik soms namelijk onnodig vast.


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

Ook heb ik er zoveel mogelijk op gelet dat alle variabelen in de global scope boven aan het bestand staan 
en dat helper functies onderaan het bestand staan. En wanneer ik meerde variabelen van hetzelfde type had, bijv 
`const`, heb ik deze zoveel mogelijk gechained net zoals bij het gebruik van Express met alle `get` en `set` requesten. 
Dit maakt de code naar mijn mening namelijk net iets leesbaarder.

<details>
    <summary>Chaining</summary>
    
```js

const express = require('express'),
    router = require('./scripts/modules/router'),
    revManifest = require('./static/rev-manifest'),
    app = express();

app
    .set('view engine', 'ejs')
    .set('views', 'views')
    .use(express.static('static'))

    .get('/', async (req, res) => {
        router.basicPage(res, 'splashscreen', 'Glory Days', revManifest);
    })

    // Check if ACCESS_TOKEN exists. If not, fetch a new one with the refresh token.
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
    });
```
</details>

Ook heb ik heel erg opgelet dat ik duidelijke naamgevingen gebruikte. Hier had ik namelijk de feedback op gekregen 
tijdens mijn beoordeling van WAFS dat dat nog wel iets beter kon. 

Ook kreeg ik de feedback dat ik nog wel iets beter in modules kon werken en daarbij ook beter moest letten op wat ik in 
welke module zetten. Soms vielen de functies die ik in modules had geplaatst namelijk iets buiten de scope van die module
en had het beter in een andere module kunnen staan. Hier heb ik dus ook heel erg op gelet tijdens mijn meesterproef dat 
ik niet weer deze fout zou maken.

### CSS To The Rescue (CSSTR)

CSSTR is helaas voor mij niet echt aan bod gekomen tijdens dit project, aangezien ik tijdens dit project niet echt 
meer heb geëxperimenteerd met CSS.

### Progressive Web Apps (PWA)

#### Criteria

Je begrijpt hoe een Service Worker werkt en kan deze in jouw applicatie op een nuttige wijze implementeren. \
Je begrijpt hoe de critical render path werkt, en hoe je deze kan optimaliseren

#### Reflectie
Ik heb een manifest gemaakt en een service worker (SW) opgezet. In de SW, cache ik de CSS en JS. Ook heb ik nog een revision 
manifest gemaakt om wijzigingen in de CSS en JS bij te houden en door te laten komen in de browser, maar aangezien 
sommige teamgenoten hierdoor wat problemen kregen met hun code hebben we ervoor gekozen om het voor nu weer even eruit 
te halen. Dit ook omdat er niet voldoende tijd was om uit te zoeken wat er nou precies fout ging. Het gekke hier was 
namelijk dat het op mijn computer en op die van sommige andere teamgenoten het het wel goed deed.

<details>
    <summary>Manifest</summary>
    
manifest.json
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

<details>
    <summary>Service Worker</summary>
    
service-worker.js
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

<details>
    <summary>Revision Manifest</summary>
   
**Build Scripts**
    
revision_hash.js
```js
const gulp = require('gulp');
const filter = require('gulp-filter');
const rev = require('gulp-rev');
const override = require('gulp-rev-css-url');

gulp.src([
    './static/**/*.{css,js}',
])
    .pipe(filter(file => !file.path.endsWith('/service-worker.js')))
    .pipe(rev())
    .pipe(override())
    .pipe(gulp.dest('./static/'))
    .pipe(rev.manifest('rev-manifest.json'))
    .pipe(gulp.dest('./static/'));
```

revision_replace.js
```js
const gulp = require('gulp');
const revReplace = require('gulp-rev-replace');

gulp.src([
    './static/service-worker.js',
])
    .pipe(revReplace({
        manifest: gulp.src('./static/rev-manifest.json')
    }))
    .pipe(gulp.dest('./static'));
```

package.json
```json
{
    ...
    "scripts": {
        ...
        "postbuild": "node scripts/build/revision_hash.js && node scripts/build/revision_replace.js",
        ...
    },
    ...
}
```

**HTML**

head.ejs
```ejs
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="Description" content="">
    <link rel="shortcut icon" type="image/png" href="/favicon.ico" />
    <link rel="manifest" href="/manifest.json">
    <link rel="stylesheet" href="/<%= /*revManifest['index-min.css']*/ %>">
    <title><%= title %></title>
</head>

<body>
```

footer.ejs
```ejs
        <script src="https://sdk.scdn.co/spotify-player.js"></script>
        <script src="/<%= /*revManifest['index-min.js'] */%>" type="module"></script>
        <script>
            if ('serviceWorker' in navigator) {
                window.addEventListener('load', function() {
                    navigator.serviceWorker.register('./../service-worker.js')
                        .then(function(registration) {
                            return registration.update();
                        });
                });
            }
        </script>
    </body>
</html>
```
</details>

Om de critical render path een beetje te optimaliseren heb ik ervoor gezorgd dat alle JS geminified wordt en dat alle 
CSS en JS en gecached worden in de SW.

Verder kreeg ik bij de beoordeling van PWA en RTW nog de feedback dat mijn NPM packages soms geïnstalleerd waren als 
gewone dependencies terwijl het eigenlijk dev dependencies zouden moeten zijn. Daar heb ik dus tijdens dit project ook 
zoveel mogelijk op gelet dat dit nu wel juist was. En dit is nu in ieder geval heel veel beter dan bij PWA en RTW.

<details>
    <summary>(Dev) Dependencies</summary>
    
package.json
```json
{
    ...
    "dependencies": {
        "cookie-parser": "^1.4.5",
        "dotenv": "^8.2.0",
        "ejs": "^3.1.3",
        "express": "^4.17.1",
        "express-session": "^1.17.1",
        "fs": "0.0.1-security",
        "heroku": "^7.42.1",
        "mongoose": "^5.9.18",
        "multer": "^1.4.2",
        "node-fetch": "^2.6.0",
        "npm-run-all": "^4.1.5",
        "query-string": "^6.12.1",
        "rimraf": "^3.0.2"
    },
    "devDependencies": {
        "chokidar-cli": "^2.1.0",
        "concurrently": "^5.2.0",
        "eslint": "^7.1.0",
        "gulp": "^4.0.2",
        "gulp-autoprefixer": "^7.0.1",
        "gulp-clean-css": "^4.3.0",
        "gulp-concat": "^2.6.1",
        "gulp-filter": "^6.0.0",
        "gulp-minify": "^3.1.0",
        "gulp-rev": "^9.0.0",
        "gulp-rev-css-url": "^0.1.0",
        "gulp-rev-replace": "^0.4.4",
        "gulp-sass": "^4.1.0",
        "nodemon": "^2.0.4",
        "npx": "^10.2.2",
        "stylelint": "^13.5.0",
        "stylelint-config-sass-guidelines": "^7.0.0",
        "stylelint-config-standard": "^20.0.0"
    }
}
```
</details>

### Browser Technologies (BT)

#### Criteria
3: Feature detection: Wat laat je zien als een browser of gebruiker 'enhancement' niet kan tonen of zien? Hoe doe je Feature Detection en wat doe je als een techniek niet werkt?
- Student laat zien hoe Feature Detection kan worden toegepast in Web Development

#### Reflectie

In het project heb ik zoveel mogelijk rekening gehouden met alle browsers. Dit heb ik gedaan door ipv de functie 
querySelector() de functie getElementById() of getElementsByClassName() te gebruiken. Dit aangezien querySelector() 
niet ondersteund wordt in IE 6 en 7 en maar gedeeltelijk gesupport wordt in IE 8. Dit is niet het geval met 
getElementById() of getElementsByClassName(). \
Ook heb ik nog om errors te voorkomen, checks op alle code gezet om te checken of een bepaald element wel 
aanwezig is in de DOM voordat er code wordt uitevoerd in de client.

Wel vind ik dat ik nog iets beter had kunnen letten op Progressive Enhancements. Zo heb ik namelijk geen fallbacks geschreven,
wat bijvoorbeeld bij het uploaden van afbeeldingen misschien nog wel handig was geweest in het geval dat de functie `createObjectURL`
niet zal werken, zoals bij IE 6 tot en met 9 waar deze functie niet ondersteund wordt.

Daarnaast heb ik ook om de input field (type `file`) van de upload button te kunnen stylen, de gewone input verborgen en 
de functionaliteit aan een andere button gegeven door middel van JavaScript (JS). Alleen heb ik hierbij niet stil gestaan bij
wat er gebeurt wanneer er geen JS is. In dit geval heb ik momenteel de input met type `file` verborgen door het attribuut `hidden="hidden"` erop te zetten.
Maar eigenlijk had ik dit beter niet kunnen doen en bijvoorbeeld gewoon met JS alle elementen hiervoor kunnen toevoegen. 
Dit aangezien wanneer er geen JS is het uploaden ook niet mogelijk is. En dan had ik standaard in de HTML een fallback kunnen 
maken voor het geval er geen JS was en deze weer met JS kunnen verbergen wanneer het er wel was.

### Real Time Web (RTW)

#### Criteria

Client-server interactie

#### Reflectie 
Voor RTW heb ik kort gekeken samen met Simone en Coen hoe je sockets in een aparte module zou kan zetten. Dit was dit 
erg leerzaam om uit te zoeken. Uiteindelijk had ik met de hulp van Laurens een voorbeeld gevonden van Lennart. 
Hij had dit namelijk gedaan bij RTW. Ik heb naar zijn code gekeken en uitgezocht hoe hij dat had gedaan en toen ik het 
zag was het eigenlijk best simpel. Wat wij de hele tijd probeerde te doen was om socketIO ook in de module te 
implementeren en aan te maken, maar uit het voorbeeld van Lennart bleek dat je dat gewoon in de server moest houden 
wat eigenlijk ook best wel logisch was.

Helaas hebben we dit niet meer geïmplementeerd in het project. Maar ik heb hier wel veel van geleerd en zal dit ook 
zeker toepassen in een volgend project waar ik gebruik maak van sockets. 

<details>
    <summary>Voorbeeld van Lennart</summary>
    
[Module](https://github.com/lennartdeknikker/wiki-game/blob/828c6b3524a479c2d1bc98a0caf7d62620dc94d9/server/scripts/socket.js) \
[Server](https://github.com/lennartdeknikker/wiki-game/blob/5d7821d94ea210f6970651d4018498a0b59e5256/bin/www)
</details>

### Web Design (WD)

#### Criteria

Leren hoe je moet testen en de resultaten gebruiken voor het verbeteren van je ontwerp
- De student kan testresultaten toepassen om een ontwerp te verbeteren.

#### Reflectie

Wanneer we een meeting hadden met Marnix, Martijn en Rebacca, en daar weer nieuwe tip en bevindingen uit kwamen, hebben 
wij als team weer nieuwe schetsen gemaakt. Daaruit hebben we de beste ideeën verwerkt in ons design en later weer voorgelegt 
aan Marnix, Martijn en Rebecca. Vaak moesten we dan nog een paas kleine dingetjes veranderen, maar was het wel al veel 
beter dan het eerste design. Dit was soms een best lang proces, voornamelijk met het maken van het definitieve design, aangezien
je dan alles perfect wilt hebben staan om een zo goed mogelijk voorbeeld te hebben voor tijdens het developen van de app.
Zo dacht ik aan het einde wel even snel de definitieve versie van het design te maken, maar toen bleek al snel dat hier heel
veel tijd in ging zitten. Gelukkig hebben toen de anderen mij nog geholpen. Hierdoor ging het veel sneller. 

Ook was het de eerste keer dat ik met Figma heb gewerkt. Alhoevel het soms nog een beetje lastig voor mij is om uit te zoeken 
waar alles staat, vond ik dit wel echt een super goede tool om samen in te kunnen werken. In het verleden had ik ook wel eens
de real-time design collaboration tool gebruikt, maar dit was soms nog een beetje haperig. Figma is wat dat betreft echt stukken
beter! 

Wat ik alleen wel jammer vind is dat je de dingen die je in Adobe XD hebt gemaakt niet kunt kopiëren naar Figma. Ik was
namelijk begonnen met mijn design in Adobe XD en kon toen we weer overstapte naar Figma weer helemaal opnieuw beginnen. 
Dat vond ik wel een beetje jammer.

Helaas hebben we tijden het project niet kunnen testen met echte personen, maar dit zou zeker nog erg leerzaam kunnen zijn
tijdens toekomstige iteraties.

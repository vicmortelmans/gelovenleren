---
title: "Bijbel-API"
date: "2018-06-09"
categories: 
  - "blog"
tags: 
  - "bijbel"
  - "projecten"
  - "publicaties"
coverImage: "yql.jpg"
---

De laatste dagen bestond mijn tijdverdrijf in het _refactoren_ van de Bijbel-API die ik ooit schreef in [YQL (Yahoo Query Language)](https://en.wikipedia.org/wiki/Yahoo!_Query_Language) naar een Google App Engine API.

Toen YQL in 2008 werd gelanceerd, leek het een heel beloftevol concept. Het internet bevat heel wat data, maar die wordt weergegeven in de vorm van websites, dus bedoeld om te lezen in een browser. Als een programmeur die gegevens wil gebruiken in een eigen webservice, moet die data worden geparseerd, dus ontdaan van alle opmaak. YQL gaf de mogelijkheid om het wereldwijde web programmatorisch te benaderen als een gigantische database.

Ik had ook mijn duit in het zakje gedaan en zo kon je op YQL bijbelverzen ophalen met een eenvoudige _query_ zoals bijvoorbeeld **'SELECT FROM bible WHERE bibleref="Joh 1:1-10" AND language="nl"'**. De service was zo geprogrammeerd dat hij in staat was de belangrijkste online bijbels te gebruiken om de verzen op te halen en terug te bezorgen in een gestandaardiseerd formaat (XML of JSON). Je kon specifiëren welke taal je wilde en zelfs welke editie. De gebruiker kon die data vervolgens in zijn toepassing op een generieke wijze verder verwerken. Dat heette dan een "open table" en zo werd de ganse bijbel in verschillende talen en edities gepresenteerd als een gigantische tabel in een online database.

Die tabel heb ik ondermeer gebruikt om de website [missale.net](http://www.missale.net/of/nl) te voorzien van bijbelverzen bij elke zondagslezing en ook in mijn project om een [online lectionarium](/blog/will-google-home-read-the-bible-for-me/) te voorzien op de Google Home personal assistant (nog steeds niet gereleased).

Verschillende features van YQL zijn intussen afgesloten (waaronder de [Developer Console](https://en.wikipedia.org/wiki/Yahoo!_Query_Language)) en ik ben bang dat de service vroeg of laat volledig uit de ether zal gaan, dus nu heb ik de Bible Open Table omgezet naar een eigen service op Google App Engine. Dat is niet helemaal hetzelfde, want het mooie aan YQL was dat je een "open table" maakte, die dus meteen ook beschikbaar was voor andere YQL-gebruikers. _Crowdsourcing_ voor _data miners_!

Een call naar de nieuwe API ziet er nu zo uit:

[http://catecheserooster.appspot.com/yql/bible?bibleref=Joh+1:1-10&language=nl](http://catecheserooster.appspot.com/yql/bible?bibleref=Joh+1:1-10&language=nl)

Het staat iedereen vrij die API te gebruiken, maar ik kan niet beloven dat hij goed onderhouden wordt. Op dit ogenblik is hij beschikbaar voor Nederlands, Engels en Frans, maar dat is gemakkelijk uit te breiden.

Voor mij is het symbolisch belangrijk dat zoiets blijft bestaan. In het Nederlandse taalgebied is er de laatste jaren veel veranderd wat betreft de beschikbaarheid van online bijbels. Tot voor een jaar of vijf kon je de Willibrordvertaling gratis raadplegen op diverse internationale bijbelsites. Ook de Nieuwe Bijbelvertaling werd van bij de publicatie publiek beschikbaar gesteld. Nu is de Willibrordvertaling enkel nog te vinden op de katholieke website [rkbijbel.nl](http://rkbijbel.nl) en de Nieuwe Bijbelvertaling krijg te enkel te zien als je bent aangmeld op de protestantse site [debijbel.nl](http://www.debijbel.nl). Je kan dat toejuichen als een vorm van professionalisering, maar ik vind dat de Kerk best wat meer zou mogen investeren in de publieke beschikbaarheid van haar schatten en zich minder zou moeten bekommeren om auteursrechten op de vertalingen. De canisiusvertaling, die nu vrij is van auteursrechten, heb ik zelf---weliswaar amateuristisch---aan het publiek [vrijgegeven op mijn blog](/blog/bijbelvertaling-petrus-canisius-studiebijbel-gratis-downloaden/).

Canisiusvertaling via de API:

[http://catecheserooster.appspot.com/yql/bible?bibleref=Joh+1:1-10&language=nl&edition=CAN](http://catecheserooster.appspot.com/yql/bible?bibleref=Joh+1:1-10&language=nl&edition=CAN)

Willibrordvertaling via de API:

[http://catecheserooster.appspot.com/yql/bible?bibleref=Joh+1:1-10&language=nl&edition=WV95](http://catecheserooster.appspot.com/yql/bible?bibleref=Joh+1:1-10&language=nl&edition=WV95)

Ik kan dus voorlopig wel verder. Als iemand de API zou willen gebruiken om een specifieke taal van de Bijbel of een specifieke editie te raadplegen, laat het maar horen. De enige voorwaarde is dat er een online publicatie beschikbaar is!

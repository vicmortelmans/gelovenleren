---
title: "Google Play Music stopt ermee, waar kunnen we terecht met onze (al dan niet religieuze) muziek?"
date: "2020-07-16"
categories: 
  - "blog"
tags:
  #- "theologie"
  #- "vrijheid"
  #- "politiek"
  #- "liturgie"
  #- "traditie"
  #- "biecht"
  #- "ethiek"
  #- "aanbidding"
  #- "sacramenten"
  #- "paus"
  #- "vlaanderen"
  #- "opvoeding"
  #- "verbeelding"
  #- "gebed"
  #- "boeken"
  #- "films"
  #- "bijbel"
  #- "woke"
  #- "antwerpen"
  - "publicaties"
  #- "kerstmis"
  #- "pasen"
  #- "kerkleer"
  #- "onderwijs"
  #- "islam"
  #- "leven"
  #- "synode"
coverImage: "Google-Play-Music-is-Set-to-Dead-on-April-30.png"
---

Dit keer een blogje dat niet over het katholieke geloof gaat, maar laat me er toch een draai aan geven met deze ge-eigende bijbelse uitdrukking: "Onderzoek alles, behoud het goede en vermijd elk kwaad, in welke vorm het zich ook voordoet" (1 Tessalonicenzen 5:21). Een leuze die op commercieel vlak al lang wordt toegepast door Google, waarbij 'hoet goede' wordt gelezen als 'het winstgevende'. Deze firma, net als veel andere technologiereuzen, maakt er een gewoonte van om allerhande gratis tools ter beschikking te stellen, daarmee talloze gebruikers te bekoren, en hen na enige tijd te zeggen dat de gratis dienst ermee ophoudt, maar dat ze kunnen overstappen op een andere tool van Google, waar goed geld mee te verdienen valt. Deze keer is [Google Play Music](http://play.google.com/music) aan de beurt, dat eind dit jaar stopt, waarna [Youtube Music](https://music.youtube.com/) als alternatief wordt voorgesteld. 

Ik gebruik Google Play Music als cloud-platform om mijn muziek op te slaan, veelal gedigitaliseerde CD's, LP's en cassettes, muziek die ik aankocht via Bandcamp en ook (mea culpa) downloads van Newpipe. Op Youtube Music blijft dat mogelijk, maar de interface is er helemaal op gericht dat je muziek van Youtube gaat streamen, en daarvoor een maandelijks bedrag neertelt, dit zonder dat je ook eigenaar wordt van de muziek waarnaar je luistert. Niet echt een valabel alternatief voor melomanen, dunkt me. 

Ik heb mijn muziek om te testen al laten overzetten naar Youtube Music en de eerste teleurstelling is dat je nummers niet langer per genre worden geklasseerd. Ik had er nochtans veel moeite in gestoken om dat op Google Play music netjes te organiseren. 

Gelukkig heeft Google ook een zoekmachine en die heeft me twee alternatieven aangebracht die wel aantrekkelijk lijken.

[iBroadcast](https://www.ibroadcast.com/home/) biedt cloudopslag voor muziekbestanden aan en gebruiksvriendelijke apps op computer of gsm om die af te spelen. De app heeft nog maar 50k downloads, dus echt groot zijn ze niet. Om je muziek op te laden, hebben ze een heel gemakkelijk tooltje voor Windows, Mac of Linux, maar de nerds kunnen ook met scripting aan de slag!

[Astiga](https://asti.ga/) werkt anders. Deze dienst biedt zelf geen opslag aan. Je moet je muziek zelf opslaan op Google Drive, OneDrive, Dropbox, Amazon S3 of iets dergelijks. Hun service bestaat er enkel in dat je die muziek heel gebruiksvriendelijk kan afspelen, ook weer via de browser of via mobiele apps. Zij zijn nog minder bekend, met amper 5k downloads… Het is dus afwachten hoe lang dit kan overleven.

Astiga heeft als voordeel dat je de opslag van je muziek zelf beheert, dus die kunnen ze je zeker nooit afpakken. Wat je niet kan, is via de app online het genre van een nummer wijzigen, dat kon je op Google Play Music wel heel gemakkelijk. Als je je muziekbestanden graag netjes organiseert, moet je dat met een andere tool doen! Daaraan heeft iBroadcast wel gedacht, daar kan je de metadata van je nummers online aanpassen, alhoewel het niet erg gebruiksvriendelijk werkt. 

Zo, die twee ga ik nu even uittesten. Ik ben niet van plan me IKEA-gewijs elke keer door de toonzaal van Youtube te worstelen als ik gewoon mijn eigen muziek wil horen. Mijn muziek beheer ik zelf en als ik eens iets anders wil horen dan mijn eigen nummers, luister ik graag naar [Radiooooo](https://radiooooo.com/#) (daar kan je kiezen uit welk jaar en uit welk land je muziek wil), of ik kijk wat rond op [Bandcamp](https://bandcamp.com/) (daar zetten veel kleinere bands hun muziek online, zodat je er enkele keren naar kan luisteren alvorens te beslissen of je ze wil aankopen), of zet ik een alternatieve radio aan, zoals [Radio Centraal](https://onlineradiobox.com/be/centraal/) of [Radio Maria](http://radio.gelovenleren.net/), of zoek ik iets op via Newpipe (wat eigenlijk een interface is naar Youtube, de cirkel is rond :) ). 

Astiga heb ik voor mezelf als volgt opgezet, maar dat kan iedereen op zijn eigen manier aanpakken:

- de 'master' files van mijn muziek staan op een USB-schijf die aangesloten is op een Raspberry Pi in mijn thuisnetwerk. Met Samba wordt dit een fileserver. 
- een kopie van de bestanden staat op Amazon S3.
- Astiga synchroniseert zich met mijn muziekbestanden op S3.
- Als ik bestanden toevoeg of wijzig, doe ik dat vanop mijn PC, waar ik toegang heb tot de USB-schijf via het thuisnetwerk.
- Om mijn muziekbestanden te beheren, gebruik ik Amarok (een Linux-toepassing)
- De Raspberry Pi voert regelmatig een scriptje uit dat die bestanden synchroniseert met S3

In een iets minder geeky omgeving, zou een NAS (een harde schijf die je op je thuisnetwerk aansluit) handig zijn, vanwaaruit je op je PC muziekbestanden met Google Drive of OneDrive synchroniseert, waar Astiga ze kan terugvinden. 

Met iBroadcast kan je ongeveer hetzelfde doen. Je hoeft je bestanden niet noodzakelijk zelf te synchroniseren in de cloud, maar op zich is dat wel handig om een backup bij te houden. Je beheert je bestanden lokaal en met een synchronisatieprogramma voeg je de wijzigingen en toevoegingen door in de kopie die bij iBroadcast wordt opgeslagen. Ook hier kan de techneut zijn gading vinden, door zelf een synchronisatiescript te schrijven met behulp van de API's die beschikbaar zijn voor zowat alle programmeertalen.

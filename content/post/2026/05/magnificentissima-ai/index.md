---
title: "Magnificentissima AI"
date: 2026-05-28T20:58:33+02:00
categories: 
  - "blog"
tags:
  - "theologie"
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
  - "boeken"
  #- "films"
  #- "bijbel"
  #- "woke"
  #- "antwerpen"
  - "publicaties"
  #- "kerstmis"
  #- "pasen"
  - "kerkleer"
  #- "onderwijs"
  #- "islam"
  #- "leven"
  #- "synode"
coverImage: "aquinas-ai.png"
---

In 2019, toen er van AI nog geen sprake was, publiceerde ik op een nieuwe website de Nederlandse vertaling van de Summa Theologiae die in 1933 in het Antwerpse Dominicanenklooster tot stand kwam ([Geloven Leren - Thomas van Aquino in een Euclidisch jasje](https://gelovenleren.net/blog/thomas-van-aquino-in-een-euclidisch-jasje/)). Niet de ganse vertaling echter! Het bronmateriaal waarop ik me baseerde, was niet publiceerbaar, het bestond uit ingescande boeken, waarop wel tekstherkenning was toegepast, maar met nog heel wat herkenningsfouten en bovendien in de oude spelling, dus een manuele correctie was absoluut noodzakelijk. De Nederlandse vertaling (die trouwens op zichzelf al onvolledig was) besloeg 15 boekdelen en in de voorbije 7 jaren is daarvan ongeveer de helft door mezelf en een tiental wisselende vrijwilligers manueel gecorrigeerd en op de website gepubliceerd, vaak met grote tussenpozen en zonder een concrete planning.

Met de opkomst van AI daagde het me dat het dit soort werk is waar AI juist goed in zou moeten zijn. En dat blijkt! De hele Nederlanse vertaling staat intussen online en AI heeft me daarbij op vier vlakken geholpen:

-   Het bedenken en evalueren van ideeën en concepten
-   Het schrijven van scripts om de automatische verwerking van de teksten uit te voeren
-   Het uitvoeren van een nieuwe tekstherkenning op de ingescande boeken
-   Het corrigeren van de oude spelling

De winst die ik maakte met AI is overduidelijk: de verwerking van duizenden bladzijdes ingescande boeken tot een nette publicatie op een website kostte me financieel minder dan enkele drankjes op een zonnig terras en zelfs met het onderzoeks- en programmeerwerk inbegrepen, kreeg ik op enkele weken dezelfde hoeveelheid werk voor mekaar waar ik eerst zeven jaar voor nodig had. Het verlies is dat ik nu eigenlijk niet meer inhoudelijk met de tekst bezig ben geweest. Ik kan hem natuurlijk zelf gaan lezen op de website, maar daar kom ik waarschijnlijk nooit toe. En hetzelfde geldt voor de vrijwilligers die zich de voorbije jaren over grote of kleine stukken van het project gebogen hebben.

Het resultaat is te bewonderen op de website: [Summa Theologiae](https://summa.gelovenleren.net/). Op de pagina 'over deze website' kan je ook reeds de PDF-bestanden downloaden die later ook als drukwerk zullen worden aangeboden.

Voor de liefhebbers ga ik even in op de techniciteit van mijn gebruik van AI voor dit project:


<a id="orgb6a23b5"></a>

# Conceptueel denken

Als ik een idee heb voor een project of een uitwerking, leg ik het voor aan ChatGPT. Die is vooral goed om de juiste hulpmiddelen te zoeken: welke programmeeromgeving gebuik je best, welke libraries zijn er ter beschikking, zijn er referenties op het web naar gelijkaardige projecten,&hellip;? Af en toe komt hij zelfs met een oplossing *out-of-the-box*, waarvan ik moet toegeven dat ik er zometeen zelf niet opgekomen was.


<a id="orgd09ae7a"></a>

# Programmeren

Programmeren doen AI chatbots heel netjes, dat is geen geheim. Ik liet scripts schrijven om de teksten te structureren volgens de geijkte indeling van de Summa. Dat kan een script perfect aan, want heel de Summa is opgebouwd volgens een strikte template van *quaestiones*, daarin *articuli* en daarin telkens een reeks *bedenkingen*, een *sed contra*, een *leerstelling* en *antwoorden* op de bedenkingen. Hoewel zo'n scripts niet erg complex zijn, liet ik AI de `python` scripts maken die de verschillende stappen van het verwerkingsproces automatiseren. Ik werk in `vscode` en gebruik de geïntegreerde chatbots van Gemini en Github in hun niet-betalende versies. Zelfs na ettelijke uren onafgebroken programmeren liep ik zelden tegen de gratis limieten aan, enkel Gemini heeft af en toe een *downgrade* gedaan naar een eenvoudiger model.


<a id="orge9838f5"></a>

# Tekstherkenning (OCR)

Je kan een ingescande PDF opladen in ChatGPT en tekstherkenning laten uitvoeren, maar voor geautomatiseerde verwerking van grotere volumes, is een gespecialiseerd model aangewezen en het gebruik van een API. Die vond ik bij [Mistral Ocr](https://docs.mistral.ai/api/endpoint/ocr). Het is een betalende service en vermits die via een API loopt, komt er wat programmeerwerk bij kijken (dat dan ook weer grotendeels naar AI gedelegeerd wordt), maar dat loont. In de laatste verwerkingsronde liet ik tekstherkenning toepassen op meer dan 2000 bladzijdes en dat kostte me welgeteld €3.46! Het resultaat is zo goed als foutloos en een ware verademing in vergelijking met de algoritmes die ik voordien gebruikte (m.n. *tesseract*), waarvan de resultaten nog boordevol fouten zaten.


<a id="org027b216"></a>

# Spellingscorrectie

Deze stap bleek uiteindelijk met minst vanzelfsprekend, hoe gek het ook klinkt. Iedereen die al eens met AI heeft gewerkt, weet dat die grammatikaal foutloze teksten aflevert. Hoe moeilijk zou het kunnen zijn om een bestaande tekst te laten corrigeren? Ik heb me regelmatig in de haren gekrabt, want er zijn onvatbaar veel parameters om rekening mee te houden&hellip; ik geef enkele puntjes

-   om niet in de fuik van een betalend model te eindigen, wilde ik een model lokaal draaien — die beslissing leidt tot een aantal vraagstukken:
    -   om zelf een AI model te draaien experimenteerde ik eerst met `llama.cpp` (daarmee kan je modellen draaien op een gewone PC, zonder GPU), maar ik ben overgestapt op `vLLM` om gebruik te kunnen maken van performantieverbeteringen zoals *prefix caching*, *continuous batching* en *speculative decoding*, waarbij me we opviel dat 'performantieverbetering' vaak wil zeggen dat je hogere prestaties krijgt, als je ook meer hardware inzet.
    -   er zijn online duizenden AI-modellen gratis te downloaden op de website [Huggingface](https://huggingface.co/); bij de specificaties krijg je meestal een indicatie van de benodigde hardware (lees: geheugen op de GPU), wat een belangrijk keuzecriterium is, maar je hebt er meestal het raden naar of een model bijvoorbeeld goed is in Nederlandse taal of in dit specifiek soort van opdrachten. Kleine modellen zijn dan ook vaak nutteloos om ernstig werk mee te verrichten. Ik koos na enig experimenteren voor het model [unsloth/gemma-3-27b-it-bnb-4bit](https://huggingface.co/unsloth/gemma-3-27b-it-bnb-4bit), dat beslaat 16GB schrijfruimte en kan draaien op een GPU met minstens 45GB RAM. Met de performantieverbeteringen actief, doet zo'n server er slechts luttele minuten over om bijvoorbeeld in een van mijn finale rondes een tekstvolume van meer dan 83000 woorden te corrigeren en daar 2665 fouten uit te halen.
    -   ik heb zelf geen PC met GPU, dus ik moet een server huren. Een vaste server met GPU is veel te duur, dus ik kwam uit bij `vast.ai`, waar eigenaars hun servers te huur aanbieden op uurtarief. Voor pakweg €1 kan je gedurende een uur je model laten lopen in een omgeving die je volledig zelf kan configureren als een docker image. Is de server waarop je gisteren werkte niet meer beschikbaar, of wil je upgraden naar een krachtiger GPU, geen probleem: je docker is overal beschikbaar.
-   als een AI teksten schrijft zonder fouten, kan die dat omdat hij gevoed is met onmetelijk veel voorbeelden, niet omdat hij elk woord grammatikaal analyseert of in een woordeboek opzoekt. De juistheid berust vooral op statistiek. Bij het corrigeren van een tekst in oude spelling (lees: systematische spelfouten), krijgt een AI snel last van *anchoring bias*. Als je hem een tekst geeft waarin tien keer het woord 'mens' voorkomt en je spelt het één keer als 'mensch', haalt hij dat er gemakkelijk uit, maar als je tekst systematisch 'mensch' schrijft en de AI in zijn trainingsdata wellicht ook wat bronnen heeft gekregen waar 'mensch' geschreven wordt, gaat hij snel denken dat het toch geen fout is. Daarom voer ik triviale correcties zoals deze vooraf uit in een zoek-en-vervangscript. Dat kan ik niet in contextgevoelige gevallen, bv. 'door den **Heiligen** Geest' moet zonder 'n', maar 'door de Heiligen van God' is wel correct.
-   een AI stuur je aan met behulp van een *prompt*. In die instructie bepaal je wat de AI moet doen en welk resultaat je verwacht. Net als een programma dat je schrijft. Er is echter een lastig onderscheid. Als een programma niet het verwachte resultaat levert, kan je het *debuggen*: in het programma kijken welke stap precies verkeerd loopt en die corrigeren. Een AI is echter een zwarte doos: je kan er niet inkijken. Het afregelen van een *prompt* is een moeizaam proces van *trial-and-error*. Ik heb gewerkt met een referentietekstfragment dat ik manueel corrigeerde, en mat dan of een bepaalde wijziging van de prompt meer of minder fouten corrigeerde. Het kwam dikwijls voor dat ik een bepaalde instructie toevoegde om een specifiek gebrek te corrigeren en daardoor plots heel wat nieuwe gebreken kreeg die met de wijziging niks te maken hadden. Dat kan best frustrerend zijn! Dit is de prompt waarmee ik uiteindelijk de correctie heb uitgevoerd:
    
    > Je taak is om uitsluitend de spelling te moderniseren.
    > Een spellingwijziging wordt NIET beschouwd als een wijziging van het woord zelf.
    > 
    > Herschrijf de volgende tekst in modern Nederlands met correcte spelling. Antwoord uitsluitend met de gecorrigeerde tekst en stop daarna.
    > 
    > Regels:
    > 
    > -   Behoud exact hetzelfde woordgebruik en dezelfde woordvolgorde.
    > -   Behoud hoofdletters
    > -   Pas ALLEEN de spelling van woorden en oude naamvalsvormen aan.
    > -   Voorbeelden van wat je moet aanpassen: 
    >     -   dubbele klinkers, bv. 'mededeeling' wordt 'mededeling', 'behooren' wordt 'behoren',
    >     -   uitgang '-sch' wordt '-s', bv. 'mensch' wordt 'mens' en 'menschen' wordt 'mensen',
    >     -   naamvalsvormen van lidwoorden, bv. 'den' wordt 'de',
    >     -   naamvalsvormen van bijvoeglijke naamwoorden, bv. 'van iederen engel' wordt 'van iedere engel'.
    > -   Verander geen betekenissen en voeg niets toe.
    > -   Laat Latijnse tekst volledig onveranderd.
    > -   Woorden die al in moderne spelling staan, blijven exact gelijk.
    > -   Twijfelgevallen mag je ongewijzigd laten, maar corrigeer duidelijke historische spellingen.
    > 
    > Tekst:


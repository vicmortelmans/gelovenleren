---
title: "Nederlandstalige kerkvaders"
date: 2026-04-08T20:26:04+02:00
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
coverImage: "kirchenvater.png"
---

Korte stukjes teksten van de kerkvaders kom je vaak tegen op katholieke websites, bijvoorbeeld als bezinning bij de dagelijkse lezingen of als citaten. Ik wilde al lang een wat langere tekst ter beschikking hebben, liefst digitaal, om er zelf een klein boekje van te maken in zakformaat, om bijvoorbeeld mee te nemen naar de mis en wat te lezen in een stil moment. Nederlandse vertalingen zijn schaars, zeker online. Als er al vertalingen worden uitgegeven, is dat in boekvorm en is die vertaling niet publiek beschikbaar. Daar heb ik nu een oplossing voor gevonden!

De Duitse website [Bibliothek der Kirchenväter](https://bkv.unifr.ch/en/works/cpg-4424/versions/kommentar-zum-evangelium-des-hl-matthaus-bkv/divisions/3) heeft een gigantisch aanbod van volledig vertaalde teksten van de kerkvaders, in verschillende talen, behalve Nederlands. We leven echter in de eeuw van AI, dus waarom zou ik die niet gewoon zelf laten vertalen! Een AI-vertaling is niet hetzelfde als een menselijke vertaling, maar ik heb genoeg aan iets wat de boodschap overbrengt, zonder me druk te maken over fijnzinnige theologische nuances (of interpretaties). Per slot van rekening zijn veel van die teksten preken en dus oorspronkelijk bedoeld voor 'gewone' gelovigen, niet voor hooggeleerde theologen.

Chatgpt vroeg ik, me een geschikte tekst aan te raden "om mee te beginnen". Hij legde me de Homilieën over het Evangelie van Mattheus op, van de H. Johannes Chrysostomus. Hij is de kerkvader die ons het grootste volume teksten naliet. Zo gezegd, zo gedaan. Blijkt dat alleen al deze 90 homilieteksten samen goed zijn voor 1000 blz. A4-formaat, niet meteen geschikt om een klein boekje van te maken. Chatgpt zei me te beginnen met de homilieën over Marcus hoofdstuk 5 en 6, de bergrede dus. Dat is samen 128 bladzijdes, wat nog te verhapstukken is om een boekje van te maken.

Nog volgens chatgpt, zou DeepL de beste vertaalservice zijn voor Europese talen. Om die te gebruiken voor documenten (met behoud van opmaak), moet je via de API gaan, dus een minimum aan technische achtergrond is vereist om de nodige POST calls te maken. Die is gratis tot 500.000 tekens per maand, wat volstaat voor de 128 bladzijdes. Als ik de ganse tekst in één keer zou willen vertalen, zou me dat ongeveer €50 kosten, berekende ik. Google Translate is een gebruiksvriendelijker en volledig gratis alternatief, waarmee ik zelfs het ganse document van 1000 blz. in een mum van tijd vertaald terugkreeg. In de vergelijking van het resultaat blijkt er geen erg groot verschil te zijn. Opmerkelijk is dat DeepL de hoofdletters bewaart in de voornaamwoorden die verwijzen naar Goddelijke Personen, wat Google niet doet. De DeepL API kan je instellen dat hij een meer formele taal hanteert, wat ik voor dit soort oude teksten heel toepasselijk vind. Voorlopig ga ik dus daarmee verder.

Met `pandoc` converteer ik de vertaalde `docx` naar PDF, in het formaat van mijn boekje, en met een zelfgemaakt `perl`-script maak ik afdrukbare katernen die ik tot een boekje kan plooien en lijmen.

Via deze link kan je de documenten terugvinden waarover ik hierboven schreef: <https://drive.google.com/drive/folders/18bNySEmdtRswkYxU6K_NxrW1KljG4eTN?usp=sharing>

{{< figure
  src="images/DeepL-vs-Google.png"
  type="full"
  caption="Vergelijking van de vertaling door DeepL vs. Google Translate"
  link="images/DeepL-vs-Google.png"
 >}}
{{< section "end" >}}



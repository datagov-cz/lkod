# Referenční implementace Lokálního katalogu otevřených dat (LKOD)
V tomto repozitáři popisujeme, jak lze již nyní zprovoznit jednoduchý lokální katalog otevřených dat (LKOD), který plně vyhovuje požadavkům [Národního katalogu otevřených dat](https://data.gov.cz) (NKOD), které mají formu [Otevřené formální normy (OFN) Rozhraní katalogů otevřených dat](https://ofn.gov.cz/rozhraní-katalogů-otevřených-dat/).
Dále v tomto repozitáři sbíráme další požadavky na referenční implementaci Lokálního katalogu otevřených dat.
Neslibujeme, že je všechny implementujeme, ale i tak se hodí je mít na jednom místě.
Pokud tedy máte požadavek na to, co by LKOD měl umět, [založte issue](https://github.com/opendata-mvcr/lkod/issues/new).

## CKAN ani DKAN nestačí
Aktuální rozšířené implementace datových katalogů [CKAN](https://ckan.org/) a [DKAN](https://getdkan.org/) nestačí.
Standardem pro metadata v Evropské unii je [DCAT-AP 2.0.1](https://joinup.ec.europa.eu/collection/semantic-interoperability-community-semic/solution/dcat-application-profile-data-portals-europe/release/201-0), který vyžaduje například využívání EU slovníků a číselníků ([EU Vocabularies](https://publications.europa.eu/en/web/eu-vocabularies/about)), umožňuje mít vícejazyčná metadata a je založen na principu [Propojených dat](https://data.gov.cz/otevřené-formální-normy/propojená-data/).
CKAN ani DKAN i přes existenci různých [DCAT rozšíření](https://github.com/ckan/ckanext-dcat) nedosahují potřebné úrovně kompatibility.

## Co je potřeba pro kompatibilitu s NKOD
Nejprve je třeba si ujasnit, co je nezbytné pro dosažení kompatibility s NKOD.
Je to pouze poskytnutí strojového rozhraní odpovídajícího [OFN Rozhraní katalogů otevřených dat](https://ofn.gov.cz/rozhraní-katalogů-otevřených-dat/).
Jedná se tedy buď o rozhraní [DCAT-AP Dokumenty](https://ofn.gov.cz/rozhraní-katalogů-otevřených-dat/2021-01-11/#dcat-ap-dokumenty), nebo [DCAT-AP SPARQL Endpoint](https://ofn.gov.cz/rozhraní-katalogů-otevřených-dat/2021-01-11/#dcat-ap-sparql-endpoint).
Uživatelské rozhraní není potřeba, datové sady budou uživateli nalezitelné přímo v Národním katalogu otevřených dat.
OFN se v čase vyvíjí podle toho, jak se vyvíjejí standardy DCAT a DCAT-AP a jak je podle nich rozšiřován NKOD.
Teď se to děje zhruba jednou za rok.

Způsob, jakým je rozhraní implementováno, OFN nespecifikuje.
Lze ho tedy implementovat libovolně.
Několik vybraných způsobů:
1. Ručně tvořené soubory s obsahem dle OFN, umístěné na web - toto lze použít pro malé, a ne často aktualizované LKODy.
Toto řešení je však velmi levné a snadné.
Jako formulář pro zadávání dat totiž poslouží [formulář NKOD](https://data.gov.cz/formulář/registrace-datové-sady), ve kterém se pouze na konci přepne do režimu "Stáhnout nový záznam pro LKOD", vyplní se IRI datové sady a IRI poskytovatele, a stáhne se hotový katalogizační záznam, který lze vystavit na web.
Pokud je jako úložiště využit GitHub, je toto vystavení zcela zdarma a lze zajistit i automatizované generování souboru s katalogem.
V opačném případě je třeba zajistit existenci souboru s katalogem, který odkazuje na jednotlivé záznamy datových sad.
URL souboru s katalogem je pak zaregistrováno v NKOD.
2. Proprietární systém, který dané soubory (či SPARQL endpoint) zpřístupňuje.
3. Vlastní silou rozšířený katalog CKAN, DKAN či jiný, který poskytne rozhraní pro vkládání dat, a navíc se zajistí transformace těchto dat do formy dle OFN.
Toho lze docílit například nástrojem [LinkedPipes ETL (open-source)](https://etl.linkedpipes.com).
4. Použití jedné z variant referenční implementace LKOD popsané v tomto repozitáři, nebo jejích částí.

## Referenční implementace LKOD - DCAT-AP Dokumenty
Tato varianta používá pouze formulář NKOD a GitHub.

### Požadavky referenční implementace DCAT-AP Dokumenty
- Repozitář na GitHubu. Nejjednodušší je udělat fork [minimalistického LKOD](https://github.com/opendata-mvcr/lkod-min).
Sem bude poskytovatel ukládat katalogizační záznamy.
GitHub zde řeší i přihlašování uživatelů a řízení přístupu uživatelů k repozitáři.
- Uživatel plnící LKOD musí umět pracovat s GitHubem.

### Workflow referenční implementace DCAT-AP Dokumenty
S touto implementací se pak pracuje následovně:
1. Pro tvorbu katalogizačního záznamu se využije [formulář pro vkládání záznamu do NKOD](https://data.gov.cz/formulář/registrace-datové-sady).
2. V posledním kroce se tlačítkem se symbolem ozubeného kolečka přepne do režimu "Stáhnout nový záznam pro LKOD", vyplní se IRI datové sady a IRI poskytovatele.
3. **Vyplněné IRI datové sady musí odpovídat URL souboru výsledného katalogizačního záznamu pro přístup k jeho nezměněné podobě po jeho uložení do GitHub repozitáře**.
Např. `https://raw.githubusercontent.com/opendata-mvcr/lkod-mvcr/master/rpp/dokumenty-převodu-agend.jsonld` pro přístup k https://github.com/opendata-mvcr/lkod-mvcr/blob/master/rpp/dokumenty-převodu-agend.jsonld.
Toto URL pak slouží i pro opětovné načtení katalogizačního záznamu do formuláře NKOD, například pro jeho editaci.
4. Výsledný JSON-LD soubor se nahraje na GitHub do repozitáře poskytovatele ([Příklad MV ČR](https://github.com/opendata-mvcr/lkod-mvcr)).
5. GitHub po této akci automaticky spustí [GitHub Actions skript](https://github.com/opendata-mvcr/lkod-github-actions/tree/master/create-catalog-file) generující soubor katalogu, který odkazuje na jednotlivé záznamy datových sad.
Spuštění tohoto skriptu musí být nakonfigurováno v daném repozitáři.
6. URL souboru katalogu, opět ve formě pro přístup k jeho nezměněné podobě po jeho uložení do GitHub repozitáře, je pak [zaregistrováno do NKOD](https://opendata.gov.cz/cinnost:registrace-vlastniho-katalogu-v-nkod).

## Referenční implementace LKOD - SPARQL Endpoint
Referenční implementace je v tuto chvíli tvořena pomocí komponent v aktuální implementaci [Národního katalogu otevřených dat](https://github.com/opendata-mvcr/nkod).
Tato varianta používá výhradně open-source software a GitHub, je pro ni potřeba mít k dispozici (virtuální) server.
Aktuálně na ní běží například [LKOD MV ČR](https://data.mvcr.gov.cz).
Jednotlivé kroky či komponenty lze libovolně vyměňovat za jiné, za předpokladu dodržení rozhraní pro NKOD.

### Požadavky referenční implementace SPARQL endpoint
- Server, kde LKOD poběží, nejlépe s OS Linux (Ubuntu, Debian, ...)
- [LinkedPipes ETL](https://github.com/linkedpipes/etl) - Open-source software pro transformace propojených dat
- Nějaká implementace SPARQL Endpointu. Například [Openlink Virtuoso Open-Source 7](https://github.com/openlink/virtuoso-opensource/), [Apache Jena Fuseki](https://jena.apache.org/documentation/fuseki2/), [Eclipse RDF4J](https://rdf4j.org/), [Blazegraph](https://blazegraph.com/) apod.
- Repozitář na GitHubu. Sem bude poskytovatel ukládat katalogizační záznamy.
- Volitelně [LinkedPipes DCAT-AP Viewer](https://github.com/linkedpipes/dcat-ap-viewer), pokud je vyžadováno i uživatelské rozhraní.
- Web server pro implementaci zabezpečení přístupu k jednotlivým komponentám a příjem Webhooks, např. [nginx](http://nginx.org/)
- Uživatel plnící LKOD musí umět pracovat s GitHubem.

### Workflow referenční implementace SPARQL Endpoint
S touto implementací se pak pracuje následovně:
1. Pro tvorbu katalogizačního záznamu se využije [formulář pro vkládání záznamu do NKOD](https://data.gov.cz/formulář/registrace-datové-sady).
2. V posledním kroce se tlačítkem se symbolem ozubeného kolečka přepne do režimu "Stáhnout nový záznam pro LKOD", vyplní se IRI datové sady a IRI poskytovatele.
3. Výsledný JSON-LD soubor se nahraje na GitHub do repozitáře poskytovatele ([Příklad MV ČR](https://github.com/opendata-mvcr/lkod-mvcr)).
GitHub zde řeší oprávnění uživatelů k editaci jednotlivých záznamů a automatizaci následného aktualizačního procesu.
4. GitHub po této akci automaticky zavolá tzv. Webhook, který spustí transformační proces v nástroji [LinkedPipes ETL](https://github.com/linkedpipes/etl).
5. Proces v LinkedPipes ETL stáhne záznamy z GitHub a nahraje je do SPARQL endpointu (rozhraní pro NKOD).
6. Volitelně, pokud je vyžadováno i uživatelské rozrhaní LKOD, LinkedPipes ETL ve stejném procesu připraví i data pro frontendovou část LKOD, která je realizována pomocí [LinkedPipes DCAT-AP Viewer](https://github.com/linkedpipes/dcat-ap-viewer), stejně jako v NKOD.
7. SPARQL endpoint je pak [zaregistrován do NKOD](https://opendata.gov.cz/cinnost:registrace-vlastniho-katalogu-v-nkod)

## LKOD, NKOD a vizualizační aplikace
LKOD je vzhledem k NKOD tedy pouze rozhraní, které splňuje [Otevřenou formální normu (OFN) Rozhraní katalogů otevřených dat](https://ofn.gov.cz/rozhraní-katalogů-otevřených-dat/). Měl by poskytovateli umožnit pohodlně zadávat katalogizační záznamy, tak jako to teď dělá [formulář pro vkládání záznamu do NKOD](https://data.gov.cz/formulář/registrace-datové-sady). Další funkcionalita, jako uživatelské prohlížení záznamů či vizualizace dat jsou volitelnou, nikoliv nutnou součástí.

![Architektura LKOD](architektura.svg)

Tento repozitář je udržován v rámci projektu OPZ č. CZ.03.4.74/0.0/0.0/15_025/0013983.
![Evropská unie - Evropský sociální fond - Operační program Zaměstnanost](https://data.gov.cz/images/ozp_logo_cz.jpg)

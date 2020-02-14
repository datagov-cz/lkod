# Referenční implementace Lokálního katalogu otevřených dat (LKOD) - repozitář pro sběr požadavků
V tomto repozitáři sbíráme požadavky na referenční implementaci Lokálního katalogu otevřených dat.
Ten bude založen na aktuální implementaci [Národního katalogu otevřených dat](https://github.com/opendata-mvcr/nkod), který již řadu potřebných částí obsahuje.

Pokud tedy máte požadavek na to, co by LKOD měl umět, [založte issue](https://github.com/opendata-mvcr/lkod/issues/new).
Budeme se snažit požadavek rozpracovat tak, aby byl jasně definovaný a aby se dalo říci, co obnáší za pracnost a jakou by měl mít prioritu.
Cílem je vytvořit jednu open-source referenční implementaci, kterou bude moci kdokoliv rozvíjet a implemenovat na svém webovém portálu.
Snažíme se vyhnout tomu, aby si každý úřad musel vyvíjet stejnou věc znova a znova.
Zároveň chceme oddělit data (metadata) od samotných aplikací, které je vytvářejí či zobrazují tak, aby mohly být jednotlivé části volně vyměnitelné.

Účelem tohoto repozitáře je tedy nejprve sběr požadavků na takové řešení, a poté i hostování jeho vývoje.

## CKAN ani DKAN nestačí
Aktuální rozšířené implementace datových katalogů [CKAN](https://ckan.org/) a [DKAN](https://getdkan.org/) nestačí.
Standardem pro metadata v Evropské unii je [DCAT-AP v1.2](https://joinup.ec.europa.eu/release/dcat-ap/12), který vyžaduje například využívání EU slovníků ([EU Vocabularies](https://publications.europa.eu/en/web/eu-vocabularies/about)), umožňuje mít vícejazyčná metadata a je založen na principu [Propojených dat](https://data.gov.cz/otevřené-formální-normy/propojená-data/).
CKAN ani DKAN i přes existenci různých [DCAT rozšíření](https://github.com/ckan/ckanext-dcat) nedosahují potřebné úrovně kompatibility.
Podobné řešení, které by ale vyhovovalo stadardům, zatím bohužel chybí.


Tento repozitář je udržován v rámci projektu OPZ č. CZ.03.4.74/0.0/0.0/15_025/0004172.
![Evropská unie - Evropský sociální fond - Operační program Zaměstnanost](https://data.gov.cz/images/ozp_logo_cz.jpg)

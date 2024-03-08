Představujeme Django
====================

Co je Django?
-------------
| Django je *vysokoúrovňový webový framework*, který umožňuje rychlý vývoj bezpečných a udržitelných webových aplikací. 
| Je napsán v Pythonu a navržen s cílem usnadnit tvorbu komplexních, databázově orientovaných webových aplikací. 
| Django se řídí principem **"DRY"** (*Don't Repeat Yourself*), který podporuje opětovné použití kódu a udržitelnost projektů.


Historie a vývoj
----------------
`Framework Django <https://cs.wikipedia.org/wiki/Django_(framework)>`_ byl původně vyvinut v roce 2003 v novinovém vydavatelství Lawrence Journal-World v Kansasu (USA), aby uspokojil potřeby rychle se vyvíjejících redakčních systémů. 

Django je open-source projekt a je dostupný pod licencí BSD, což znamená, že je zdarma k použití a modifikaci. Od svého vzniku se Django stalo jedním z nejpoužívanějších webových frameworků a získalo velkou a aktivní komunitu vývojářů.


.. admonition:: Zajímavost 
     
    Název Django je odvozen od jména jazzového kytaristy **Django Reinhardta**. K tomuto názvu došlo náhodou, když jeden z vývojářů poslouchal jeho hudbu během vývoje frameworku. 

     .. image:: https://upload.wikimedia.org/wikipedia/commons/thumb/f/f5/Django_Reinhardt_%28Gottlieb_07301%29.jpg/220px-Django_Reinhardt_%28Gottlieb_07301%29.jpg
        :alt: Django Reinhardt
        :align: center
        :width: 300
        :height: 300
        :target: https://cs.wikipedia.org/wiki/Django_Reinhardt
        
    `Django Reinhardt <https://cs.wikipedia.org/wiki/Django_Reinhardt>`_  byl belgický jazzový kytarista a skladatel, který byl jedním z prvních a nejvýznamnějších jazzových kytaristů.


Proč používat Django?
---------------------
- **Škálovatelnost, výkon, flexibilita.** Django bylo navrženo s ohledem na schopnost zvládnout velmi náročné aplikace a nabízí různé nástroje a techniky pro optimalizaci výkonu a škálovatelnosti aplikací. Je vhodné pro projekty všech velikostí, od malých osobních blogů po velké komerční weby. Díky své flexibilitě a rozšiřitelnosti může Django uspokojit potřeby široké škály webových projektů.
- **Bezpečnost.** Django klade velký důraz na bezpečnost webových aplikací. Automaticky zajišťuje ochranu proti mnoha běžným útokům, jako je SQL injection, cross-site scripting, cross-site request forgery a další.
- **Rychlost vývoje.** Django umožňuje rychlý vývoj webových aplikací. Django je dodáváno s širokou škálou knihoven, které pokrývají běžné webové vývojové úkoly, jako je autentizace uživatelů, formuláře, upload souborů, a další. 
- **Dokumentace, komunita.** Django má velmi kvalitní a podrobnou dokumentaci. Stojí za ním také velká a aktivní komunita vývojářů.


Klíčové vlastnosti
------------------
- **MVT architektura.** Django používá MVT (Model-View-Template) architekturu, což je varianta tradičního modelu MVC (Model-View-Controller). Tato architektura pomáhá organizovat kód a oddělit logiku aplikace od jejího uživatelského rozhraní.
- **Automatická správa databáze.** Django podporuje mnoho různých databázových systémů. Modely v Django jsou nezávislé na konkrétním databázovém systému. Poskytují výkonný a intuitivní systém pro definici databázových schémat a manipulaci s databázovými záznamy. Díky ORM (Object-Relational Mapping, objektově-relační mapování) můžete pracovat s databázemi použitím Python kódu místo SQL. 
- **URL mapování.**  Django obsahuje vlastní URL mapování, které umožňuje definovat URL adresy pro jednotlivé části aplikace.
- **Šablony.** Django obsahuje vlastní šablonovací systém, který umožňuje oddělit prezentaci od logiky aplikace. Vychází z konceptu DRY a umožňuje opětovné použití šablon, použití bloků, dědičnost šablon a další.
- **Formuláře.** Součástí Django je vlastní formulářový framework, který umožňuje snadno vytvářet a zpracovávat formuláře, včetně validace dat a zpracování vstupů od uživatelů.
- **Autentizace a autorizace.** Django obsahuje zabudovaný systém pro autentizaci a autorizaci uživatelů. Nabízí také možnost připojení k externím systémům pro autentizaci, jako jsou OAuth, OpenID a další.
- **Administrační rozhraní.** Velkou výhodou tohoto frameworku je vestavěné administrační rozhraní, které umožňuje snadno spravovat data v aplikaci. Toto rozhraní je automaticky generováno na základě definice modelů a umožňuje provádět CRUD operace nad daty. Rozhranní je také možné rozšiřovat a upravovat podle potřeb aplikace.
- **Podpora jazykových verzí.** Django obsahuje zabudovanou podporu pro mezinárodní aplikace a lokalizaci. Využívá standardních nástrojů pro lokalizaci a umožňuje snadno překládat aplikaci do různých jazyků.
- **Testování.** Django obsahuje zabudovaný testovací framework, který umožňuje snadno psát a spouštět testy aplikace.
- **REST framework.** Django nabízí vlastní REST framework, který umožňuje snadno vytvářet RESTful API pro aplikace. 
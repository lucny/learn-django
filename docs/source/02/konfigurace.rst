Konfigurace projektu v Django
=============================

Konfigurace Django projektu se provádí v souboru `settings.py`. V tomto souboru se nastavují všechny parametry, které se týkají chování aplikace. Nastavení se provádí pomocí proměnných, které se nacházejí v souboru `settings.py`. Některé z těchto proměnných jsou povinné, jiné jsou volitelné. Některé z proměnných se mohou nacházet v souboru `settings.py` přímo, jiné mohou být v souborech, které se importují v `settings.py`.

Soubor settings.py je rozdělěn do řady sekcí, které se týkají různých částí konfigurace.

BASE_DIR
--------

BASE_DIR je proměnná, která uchovává cestu k adresáři, kde je uložen soubor settings.py. To je zpravidla kořenový adresář vašeho Django projektu. Tato proměnná se používá jako výchozí bod pro odvození cest k různým zdrojovým souborům a složkám, jako jsou statické soubory, šablony, databázové soubory atd.

V Django 3.1 a novějších verzích je BASE_DIR nastavena s použitím třídy Path z modulu pathlib, který je součástí standardní knihovny Pythonu. Třída Path umožňuje snadnou a bezpečnou manipulaci s cestami k souborům.


.. code-block:: python

  from pathlib import Path

  # Build paths inside the project like this: BASE_DIR / 'subdir'.
  BASE_DIR = Path(__file__).resolve().parent.parent

* ``Path(__file__)`` vytváří Path objekt, který reprezentuje cestu k souboru, ve kterém je tento kód zapsán (tj. settings.py).
* ``resolve()`` vyřeší jakékoliv symbolické odkazy v cestě, což znamená, že dostanete absolutní cestu k souboru bez zkratů jako jsou . nebo ...
* ``parent`` je vlastnost třídy Path, která vrací rodičovský adresář dané cesty. Protože settings.py je typicky v adresáři, který je podadresářem kořenového adresáře projektu (např. ``school_library/school_library/settings.py``), použití ``.parent`` dvakrát (``parent.parent``) nám dá cestu k tomu kořenovému adresáři projektu.

Příklady použití:
'''''''''''''''''

.. code-block:: python

  # Konfigurace pro ukládání statických souborů
  STATIC_URL = '/static/'
  STATICFILES_DIRS = [
      BASE_DIR / 'static',  # cesta k adresáři static ve vašem projektu
  ]

  # Konfigurace pro ukládání mediálních souborů
  MEDIA_URL = '/media/'
  MEDIA_ROOT = BASE_DIR / 'media'  # cesta k adresáři media ve vašem projektu

  # Konfigurace pro ukládání šablon
  TEMPLATES = [
      {
          # ... nějaké další konfigurace ...
          'DIRS': [BASE_DIR / 'templates'],  # cesta k adresáři šablon
          # ... nějaké další konfigurace ...
      },
  ]

  # Konfigurace databáze - například pro SQLite
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.sqlite3',
          'NAME': BASE_DIR / 'db.sqlite3',  # cesta k souboru databáze
      }
  }


Tento způsob konfigurace umožňuje snadno a přehledně určit, kde má Django hledat vaše soubory, a zároveň držet vaše cesty relativní k BASE_DIR, což je užitečné, pokud vaše aplikace bude nasazena na různých serverech nebo prostředích.


SECRET_KEY
----------	

SECRET_KEY je dlouhý náhodný řetězec, který se používá pro zabezpečení webu. Django jej používá k zajištění, že data uložená v cookies nebo uložená v jiné podobě na straně klienta nejsou snadno čitelná nebo upravitelná. Příklady jeho použití zahrnují:

Zabezpečení cookies, včetně session cookies a CSRF cookies.
Generování kryptografických podpisů, které ověřují, že informace v URL nebo cookies nebyly upraveny.

.. code-block:: python

  # SECURITY WARNING: keep the secret key used in production secret!
  SECRET_KEY = 'django-insecure-%uq*p_3nun)e4uzo***uh=p80cz#u)as42ydd1_-#bj@0wff1e'


* ``SECRET_KEY`` by měl zůstat tajný, protože jakákoli únik může vést k bezpečnostním rizikům.
* Hodnota ``'django-insecure-%uq*p_3nun)e4uzo***uh=p80cz#u)as42ydd1_-#bj@0wff1e'`` je výchozí nastavení generované Django při vytvoření projektu a měla by být nahrazena skutečným bezpečnostním klíčem před nasazením do produkčního prostředí.

.. caution:: 
    
	**Doporučení pro produkční prostředí:**

 	* Nikdy nevkládejte ``SECRET_KEY`` do verzovacích systémů jako je Git, pokud je tento repozitář veřejně přístupný.
	* V produkčním prostředí by ``SECRET_KEY`` měl být nastaven na jedinečnou, náhodnou hodnotu a měl by být chráněný.
  	* Jeden způsob, jak spravovat ``SECRET_KEY`` bezpečně, je pomocí environmentálních proměnných nebo speciálních nástrojů pro správu tajných klíčů.
  

Příklad nastavení pro produkční prostředí:
''''''''''''''''''''''''''''''''''''''''''

.. code-block:: python
   
   import os

   SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'volitelný-výchozí-klíč-pro-vývoj')

Tento přístup umožňuje, aby v produkčním prostředí byla ``SECRET_KEY`` načítána z environmentální proměnné ``DJANGO_SECRET_KEY``. Pokud tato proměnná není k dispozici (např. vývojové prostředí), použije se výchozí hodnota ``'volitelný-výchozí-klíč-pro-vývoj'``. V produkčním prostředí byste ale nikdy neměli mít nastavenou výchozí hodnotu.


DEBUG
-----	

``DEBUG`` je logická (boolean) proměnná, která by měla být nastavena na True pouze během vývoje.

Když je ``DEBUG`` nastavena na ``True``:

* Django zobrazí podrobné chybové stránky s trasováním volání (stack trace) a dalšími diagnostickými informacemi, pokud dojde k chybě ve vaší aplikaci. To usnadňuje vývojářům ladění jejich kódu.
* Statické soubory jsou obsluhovány Django, což zjednodušuje vývoj, protože nemusíte nastavovat webový server pro obsluhu statických souborů.
* Výkonnostní optimalizace, které by měly být aktivní v produkčním prostředí, jsou deaktivovány.

.. code-block:: python

    # SECURITY WARNING: don't run with debug turned on in production!
    DEBUG = True

.. caution:: 
    
	**Doporučení pro produkční prostředí:**

    Když nasazujete aplikaci do produkčního prostředí, je nezbytné nastavit DEBUG na False:

    * S ``DEBUG`` nastaveným na False, Django nezobrazí podrobné chybové stránky, které by mohly odhalit citlivé informace. Místo toho uživatelům zobrazí generické chybové stránky, obvykle ``500`` interní chyba serveru.
    * Statické soubory by měly být obsluhovány samostatným webovým serverem nebo službou pro doručování statických souborů.
    * Všechny výkonnostní optimalizace by měly být aktivní.
    * Je důležité si uvědomit, že zapnutí ``DEBUG`` v produkčním prostředí představuje značné bezpečnostní riziko a mělo by se tomu vždy vyhnout.

Příklad nastavení pro produkční prostředí:
''''''''''''''''''''''''''''''''''''''''''

Pro zajištění, že DEBUG je v produkčním prostředí vypnuto, můžete použít:

.. code-block:: python
   
    DEBUG = os.environ.get('DJANGO_DEBUG', 'False') == 'True'

V tomto případě aplikace načte hodnotu proměnné prostředí ``DJANGO_DEBUG`` a převede ji na Python ``boolean``. Pokud proměnná prostředí není nastavena, ``DEBUG`` bude defaultně ``False``.


ALLOWED_HOSTS
-------------	

``ALLOWED_HOSTS`` je seznam řetězců reprezentujících doménová jména a/nebo IP adresy, které může váš server přijímat. Když Django zpracovává žádost, kontroluje hodnotu HTTP Host hlavičky (kterou může klient libovolně nastavit) a ověří ji proti tomuto seznamu.

Pokud není hostitel, ze kterého přichází žádost, uveden v ``ALLOWED_HOSTS``, Django vrátí chybu ``Bad Request (400)``.

.. code-block:: python

    ALLOWED_HOSTS = []

Ve výchozím nastavení je seznam prázdný, což je v pořádku během vývoje, protože vývojový server není přístupný z Internetu a nečelí tedy útokům založeným na ``HTTP Host`` hlavičce.

.. caution:: 
    
	**Doporučení pro produkční prostředí:**

    Před nasazením vašeho projektu do produkčního prostředí byste měli specifikovat, které hostitele může váš server obsloužit. To se dělá vložením doménových jmen a IP adres do seznamu ALLOWED_HOSTS.


Příklad nastavení pro produkční prostředí:
''''''''''''''''''''''''''''''''''''''''''

Povolení konkrétního hostitele:

.. code-block:: python
   
    ALLOWED_HOSTS = ['www.yourwebsite.com']


Povolení více hostitelů:

.. code-block:: python
   
    ALLOWED_HOSTS = ['www.yourwebsite.com', 'subdomain.yourwebsite.com', 'localhost', '127.0.0.1']


Povolení všech hostitelů (nedoporučuje se v produkčním prostředí):

.. code-block:: python
   
    ALLOWED_HOSTS = ['*']


V tomto případě je hodnota ``DJANGO_ALLOWED_HOSTS`` načtena z prostředí a rozdělena podle čárek, což umožňuje snadné nastavení více hostitelů. Pokud proměnná prostředí není nastavena, bude použita defaultní hodnota 'www.yourwebsite.com'.


INSTALLED_APPS
--------------	

``INSTALLED_APPS`` je seznam obsahující řetězce, které identifikují Django aplikace, které jsou aktivní pro váš projekt. Každá aplikace je zde zastoupena svým plným konfiguračním názvem třídy nebo cestou k modulu aplikace.

Django využívá tento seznam k řadě interních procesů, jako jsou:

- **Database Migrations:** Django vyhledá v každé aplikaci soubory s migracemi, aby věděl, jak nastavit nebo změnit databázové schéma.
- **Model Registration:** Aby byly modely dostupné v Django ORM, musí být aplikace, která je obsahuje, zahrnuta do ``INSTALLED_APPS``.
- **Admin Interface:** Django automaticky vyhledá modely v aplikacích pro registraci v administrativním rozhraní.
- **Template Tags and Filters:** Vlastní šablonové tagy a filtry jsou registrovány, když jsou jejich aplikace uvedeny v ``INSTALLED_APPS``.
- **Test Discovery:** Django hledá testy k běhu v aplikacích uvedených v ``INSTALLED_APPS``.

.. code-block:: python
   
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'library.apps.LibraryConfig',  # příklad vlastní aplikace
    ]

Každý řetězec v seznamu reprezentuje aplikaci. Standardní aplikace začínající ``django.contrib`` jsou součástí frameworku Django a poskytují následující funkce:

- ``django.contrib.admin`` Administrativní stránka, která usnadňuje správu obsahu stránek.
- ``django.contrib.auth`` Autentizační systém pro správu uživatelů, skupin, oprávnění a cookies.
- ``django.contrib.contenttypes`` Framework pro sledování typů objektů pro modely.
- ``django.contrib.sessions`` Systém pro správu session (sezení) přes webové požadavky.
- ``django.contrib.messages`` Framework pro dočasné zprávy mezi požadavky.
- ``django.contrib.staticfiles`` Správa a sloužení statických souborů.

Dále je zde uvedena aplikace ``library.apps.LibraryConfig``, což je uživatelská aplikace definovaná v projektu. Zde je ``LibraryConfig`` název konfigurační třídy aplikace, která je definovaná v souboru ``apps.py`` uvnitř aplikace ``library``.


MIDDLEWARE
----------

``MIDDLEWARE`` je série háčků (hooks) a globálních funkcí v žádosti/response cyklu. 

Zde je seznam standardních middleware tříd, které Django obsahuje:

- ``django.middleware.security.SecurityMiddleware``
  Zajišťuje různé bezpečnostní funkce jako nastavení hlaviček HTTP pro ochranu proti určitým útokům, jako je XSS.

- ``django.contrib.sessions.middleware.SessionMiddleware``
  Spravuje data session, umožňuje uchování dat mezi různými požadavky stejného uživatele.

- ``django.middleware.common.CommonMiddleware``
  Poskytuje obecné funkce jako správu trailing slash a přesměrování URL.

- ``django.middleware.csrf.CsrfViewMiddleware``
  Chrání proti Cross-Site Request Forgery tím, že zajistí přítomnost CSRF tokenu v POST požadavcích.

- ``django.contrib.auth.middleware.AuthenticationMiddleware``
  Přidává `request.user`, který je buď instance `AnonymousUser`, nebo skutečný objekt uživatele pro přihlášeného uživatele.

- ``django.contrib.messages.middleware.MessageMiddleware``
  Umožňuje zobrazování jednorázových zpráv (flash messages) v aplikaci.

- ``django.middleware.clickjacking.XFrameOptionsMiddleware``
  Ochrana proti clickjacking útokům nastavením hlavičky `X-Frame-Options`.

Pro plnou funkcionalitu těchto middleware je nezbytné je mít správně nakonfigurované a poradí v seznamu `MIDDLEWARE` má také význam, protože určuje pořadí, ve kterém jsou middleware aplikovány na každý požadavek/odpověď.


ROOT_URLCONF
------------

``ROOT_URLCONF`` je řetězec, který určuje, který soubor bude použit jako kořenový soubor pro URL konfiguraci. Tento soubor obsahuje definice URL adres, které mají být směrovány na jednotlivé pohledy (views) v aplikaci.

.. code:: python

    ROOT_URLCONF = 'school_library.urls'

Výchozí hodnota je ``'school_library.urls'``, což znamená, že Django bude hledat soubor ``urls.py`` v aplikaci ``school_library``. Tento soubor bude obsahovat definice URL adres pro celou aplikaci.


TEMPLATES
---------

``TEMPLATES`` je seznam konfigurací šablonovacího systému, které Django používá k vykreslování HTML šablon. Každá konfigurace je slovník, který obsahuje různé nastavení pro šablonovací systém. 

Každý slovník může obsahovat několik klíčů, ale některé z nich jsou povinné. Některé z povinných klíčů jsou:

- ``BACKEND``: Název šablonovacího backendu, který se má použít. Většinou se používá ``'django.template.backends.django.DjangoTemplates'``.
- ``DIRS``: Seznam adresářů, ve kterých má Django hledat šablony. Toto je povinné, pokud chcete používat vlastní šablony.
- ``APP_DIRS``: Logická hodnota, která určuje, zda má Django hledat šablony v aplikacích. Většinou je nastavena na ``True``.
- ``OPTIONS``: Další konfigurace pro šablonovací systém.
- ``'context_processors'``: Seznam funkcí, které mají být použity pro vytváření kontextu pro každou šablonu.

.. code:: python

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [BASE_DIR / 'templates'],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]

V tomto příkladu je konfigurace šablonovacího systému nastavena tak, aby Django hledal šablony v adresáři ``templates`` ve vašem projektu. Tento adresář je určen pomocí proměnné ``BASE_DIR``. 
Tento adresář je také nastaven jako adresář pro šablony v konfiguraci šablonovacího systému.


WSGI_APPLICATION
----------------

``WSGI_APPLICATION`` je řetězec, který určuje, který soubor bude použit jako WSGI aplikace. WSGI (Web Server Gateway Interface) je standardní rozhraní mezi webovým serverem a webovou aplikací napsanou v Pythonu.

.. code:: python

    WSGI_APPLICATION = 'school_library.wsgi.application'

Výchozí hodnota je ``'school_library.wsgi.application'``, což znamená, že Django bude hledat soubor ``wsgi.py`` v aplikaci ``school_library``. 
Tento soubor bude obsahovat WSGI aplikaci, která bude použita pro nasazení aplikace na webový server.


DATABASES
---------

``DATABASES`` je slovník, který obsahuje konfigurace pro všechny databáze, které vaše aplikace používá. Většinou se používá jedna databáze, ale Django umožňuje používat více databází. 
Každá konfigurace je slovník, který obsahuje různé nastavení pro připojení k databázi. 

Některé z klíčů, které můžete použít, jsou:

- ``ENGINE``: Název backendu databáze, který se má použít. Například ``'django.db.backends.sqlite3'`` pro SQLite, ``'django.db.backends.postgresql'`` pro PostgreSQL, atd.
- ``NAME``: Název databáze, ke které se má připojit.
- ``USER``: Uživatelské jméno pro připojení k databázi.
- ``PASSWORD``: Heslo pro připojení k databázi.
- ``HOST``: Adresa hostitele, na kterém běží databáze.
- ``PORT``: Číslo portu, na kterém běží databáze.
- ``CONN_MAX_AGE``: Maximální věk spojení v sekundách, než bude uzavřeno a znovu otevřeno.
- ``OPTIONS``: Další konfigurace pro připojení k databázi.

.. code:: python

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': BASE_DIR / 'db.sqlite3',
        }
    }


V tomto příkladu je konfigurace databáze nastavena tak, aby Django používal SQLite databázi, která je uložena v souboru ``db.sqlite3`` ve vašem projektu. Tento soubor je určen pomocí proměnné ``BASE_DIR``.


AUTH_PASSWORD_VALIDATORS
------------------------

``AUTH_PASSWORD_VALIDATORS`` je seznam konfigurací, které určují, jakým způsobem bude ověřováno heslo uživatele. Každá konfigurace je slovník, který obsahuje různé nastavení pro ověřování hesla.

Některé z klíčů, které můžete použít, jsou:

- ``NAME``: Název ověřovacího pravidla.
- ``OPTIONS``: Další konfigurace pro ověřovací pravidlo.

Možnosti OPTIONS mohou zahrnovat:

- ``'min_length'``: Minimální délka hesla.
- ``'min_digits'``: Minimální počet číslic v hesle.
- ``'min_special_characters'``: Minimální počet speciálních znaků v hesle.
- ``'min_lowercase'``: Minimální počet malých písmen v hesle.
- ``'min_uppercase'``: Minimální počet velkých písmen v hesle.
- ``'max_similarity'``: Maximální podobnost hesla s uživatelskými údaji.
- ``'user_attributes'``: Uživatelské atributy, které se mají použít pro ověřování hesla.
- ``'user_model'``: Název modelu uživatele, který se má použít pro ověřování hesla.
- ``'validate_password_change'``: Logická hodnota, která určuje, zda se má heslo ověřovat při změně.
- ``'validate_password_creation'``: Logická hodnota, která určuje, zda se má heslo ověřovat při vytvoření.
- ``'validate_user_attributes'``: Logická hodnota, která určuje, zda se mají ověřovat uživatelské atributy.
- ``'validate_user_password'``: Logická hodnota, která určuje, zda se má ověřovat uživatelské heslo.
- ``'validate_user_password_change'``: Logická hodnota, která určuje, zda se má ověřovat změna uživatelského hesla.
- ``'validate_user_password_creation'``: Logická hodnota, která určuje, zda se má ověřovat vytvoření uživatelského hesla.
- ``'validate_user_password_reset'``: Logická hodnota, která určuje, zda se má ověřovat resetování uživatelského hesla.
  
.. code:: python

    AUTH_PASSWORD_VALIDATORS = [
        {
            'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
            'OPTIONS': {
                'min_length': 9,
            }
        },
        {
            'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
        },
        {
            'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
        },
    ]


V tomto příkladu je konfigurace ověřování hesla nastavena tak, aby Django ověřoval heslo uživatele pomocí tří pravidel:

- ``MinimumLengthValidator``: Ověřuje, zda je heslo delší než 9 znaků.
- ``CommonPasswordValidator``: Ověřuje, zda heslo není běžné heslo.
- ``NumericPasswordValidator``: Ověřuje, zda heslo obsahuje číslice.
    

LANGUAGE_CODE
-------------

``LANGUAGE_CODE`` je řetězec, který určuje výchozí jazyk, který má být použit pro všechny překlady v aplikaci. Tento řetězec by měl být ve formátu jazyk_kód, například ``'en-us'`` pro angličtinu nebo ``'cs-cz'`` pro češtinu.

.. code:: python

    LANGUAGE_CODE = 'cs-cz'

V tomto příkladu je výchozí jazyk nastaven na češtinu.


TIME_ZONE
---------

``TIME_ZONE`` je řetězec, který určuje výchozí časové pásmo, které má být použito pro všechny časové údaje v aplikaci. Tento řetězec by měl být ve formátu kontinent/město, například ``'Europe/Prague'``.

.. code:: python

    TIME_ZONE = 'Europe/Prague'

V tomto příkladu je výchozí časové pásmo nastaveno na středoevropský čas.


USE_I18N
--------

``USE_I18N`` je logická (boolean) proměnná, která určuje, zda má být internacionalizace (i18n) povolena. Když je nastavena na ``True``, Django bude hledat překlady pro všechny textové řetězce v aplikaci.

.. code:: python

    USE_I18N = True

V tomto příkladu je internacionalizace povolena.


USE_L10N
--------

``USE_L10N`` je logická (boolean) proměnná, která určuje, zda má být lokalizace (l10n) povolena. Když je nastavena na ``True``, Django bude používat lokalizované formáty pro datumy a čísla.

.. code:: python

    USE_L10N = True

V tomto příkladu je lokalizace povolena.


USE_TZ
------

``USE_TZ`` je logická (boolean) proměnná, která určuje, zda má být časová zóna povolena. Když je nastavena na ``True``, Django bude ukládat časové údaje v UTC a převádět je na místní časové pásmo při vykreslování.

.. code:: python

    USE_TZ = True

V tomto příkladu je časová zóna povolena.


STATIC_URL
----------

``STATIC_URL`` je řetězec, který určuje URL adresu, pod kterou budou statické soubory dostupné. Tato URL adresa bude použita pro všechny statické soubory, které jsou součástí aplikace. 
Statické soubory mohou zahrnovat obrázky, CSS soubory, JavaScript soubory, atd. 

.. code:: python

    STATIC_URL = '/static/'

V tomto příkladu je URL adresa pro statické soubory nastavena na ``'/static/'``. 


DEFAULT_AUTO_FIELD
------------------

``DEFAULT_AUTO_FIELD`` je řetězec, který určuje, jaký typ pole bude použit jako primární klíč pro nové modely. Tento řetězec by měl být ve formátu ``'app_label.ModelName.field_name'``, kde ``app_label`` je název aplikace, ``ModelName`` je název modelu a ``field_name`` je název pole.

.. code:: python

    DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

V tomto příkladu je primární klíč pro nové modely nastaven na ``BigAutoField``. Tento typ pole je vhodný pro databáze, které podporují velká čísla.
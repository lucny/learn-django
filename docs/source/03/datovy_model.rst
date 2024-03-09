Datový model projektu
=====================

Poté, co jsme se seznámili se základy práce s modely v Django, je na čase se podívat na datový model našeho projektu. 
Konečným cílem je vytvořit funkční webovou aplikaci pro školní knihovnu, ale zatím si vystačíme s jednodušším modelem, 
který bude obsahovat pouze informace o knihách, včetně s nimi provázaných informacích o autorech, žánrech a vydavatelstvích.

ER diagram
----------
Budeme vycházet z následujícího ER diagramu:

.. figure:: media/er-diagram-school-library-01.png
   :alt: ER diagram
   :align: center

ER diagram školní knihovny - základní datový model

V tomto případě jsme pro identifikaci jednotlivých entit i atributů použili pseudočeské názvy bez diakritiky.

- Entita `autor` obsahuje informace o autorech knih. Každý autor má své jméno, příjmení, datum narození, případně i datum úmrtí, biografii (životopis) a fotografii.
- Entita `zanr` obsahuje informace o žánrech knih. Každý žánr má svůj název.
- Entita `vydavatelstvi` obsahuje informace o vydavatelstvích. Každé vydavatelství má svůj název a adresu.
- Entita `kniha` obsahuje informace o knihách, které mohou být v knihovně k dispozici. Každá kniha má svůj titul, obsah, počet stran a rok vydání. 
  Tato entita je provázána s entitami `autor`, `zanr` a `vydavatelstvi` pomocí různých typů vztahů. 
  Vztahy mezi knihou a autorem i knihou a žánrem jsou typu M:N, vztah mezi knihou a vydavatelstvím je typu 1:N.
  
Vytvoření modelu
----------------
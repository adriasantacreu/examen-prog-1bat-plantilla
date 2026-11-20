# Trimestral: fulls de càlcul

**1BAT Programació i robòtica · 20/11/2026 · Durada: 1 h 30**
**Total: 10 punts** (Dades 3,5 p · Regex 1,5 p · Dashboard 3 p · Teoria 1 p · Test 1 p)

## Instruccions

- **Grava la pantalla** durant tot l'examen i puja la gravació a Classroom.
- **Internet sí, IA no** (ni ChatGPT, Gemini o Copilot, ni missatgeria). Cerca només amb [DuckDuckGo](https://duckduckgo.com), mai des de la barra de Chrome.
- Fes les parts en ordre; cada pas indica on va el resultat i la funció amb la seva documentació.
- Encallat? Copia les columnes `M:Q` del full *Taula resolta* (enllaç a Classroom) com a valors (`Ctrl+Maj+V`). Aquell pas no puntua.
- Si no entens un enunciat, fes-ho amb la teva millor interpretació i escriu-la en un comentari de la cel·la (clic dret → `Insereix un comentari`). Es tindrà en compte en corregir.
- Lliura a Classroom l'enllaç del teu full *i* l'enllaç del teu repositori (amb `test.md` i `teoria.md` dins).

**Context.** La Lliga de robots acaba la temporada i cal un resum per repartir medalles. El fitxer [`dades_trimestral_1bat-prog.csv`](dades_trimestral_1bat-prog.csv) (aquí al repositori, amb el botó de descàrrega, i també a Classroom) té **24 recorreguts**, un per fila (files 2 a 25; la fila 1 són les capçaleres). Què hi ha a cada columna:

- `A` Equip · `B` Robot · `C` Categoria (Velocitat, Laberint o Sumo) · `D` Ronda · `E` Temps (durada del recorregut) · `F` Infraccions (quantes en ha fet) · `G` Contacte (correu).
- `I2:J4`: taula de *longitud de la pista* (en metres) de cada categoria: a `I` la categoria, a `J` els metres.
- `L2`: *penalització*, els segons que se sumen per cada infracció.

**Com treballar:** les columnes noves (`M` a `Q`) les crees tu, *una per pas*, escrivint el títol a la fila 1 i la fórmula a la fila 2, i omplint-la cap avall fins a la fila 25.

---

## Part A · Preparar i calcular (3,5 punts)

**A1.** **Prepara el full.** Fes aquests 4 passos: (1) importa el CSV a un *full de càlcul nou* (`Fitxer` → `Importa`); (2) posa la configuració regional a *Estats Units* (`Fitxer` → `Configuració`), perquè els decimals portin punt; (3) congela la fila 1 (`Visualitza` → `Inclou`); (4) selecciona la columna `E` (Temps) i aplica-hi el format de durada `[h]:mm:ss` (`Format` → `Nombre` → `Format personalitzat`). Canvia el nom del full pel teu nom i la pestanya de dades per `Dades`. **[0,5 punts]**

**A2.** **Segons finals** (columna `M`; escriu `Segons finals` a `M1` i la fórmula a `M2`). Els segons finals d'un recorregut són *el seu temps expressat en segons* més *els segons de penalització de totes les seves infraccions* (una penalització per cada infracció). Fes-ho amb una sola fórmula a `M2` i omple fins a `M25`. *Pista: el temps de la columna `E` és una fracció de dia (un dia té 24 h).* Exemple: un recorregut d'1 minut amb 2 infraccions i una penalització de 5 s val 60 + 2·5 = 70 s.

> *Segons finals* = (temps × 86400) + (infraccions × penalització)

**[1 punt]**

**A3.** **Medalla** (columna `N`; títol `Medalla` a `N1`, fórmula a `N2` i omple fins a `N25`). La fórmula mira els segons finals de la mateixa fila (columna `M`) i escriu *com a text* la medalla: *Or* si són 60 o menys; *Plata* si són més de 60 i fins a 90; *Bronze* si són més de 90 i fins a 120; un guió `-` si passen de 120. *Funció: [`IFS`](https://support.google.com/docs/answer/7014145?hl=en).*

> *Medalla* = si els segons finals són ≤ 60 → Or | si no, si són ≤ 90 → Plata | si no, si són ≤ 120 → Bronze | en la resta → `-`

**[1 punt]**

**A4.** **Velocitat** (columna `O`; títol `Velocitat` a `O1`, fórmula a `O2` i omple fins a `O25`), en metres per segon. Per a cada fila: (1) mira la categoria de la columna `C`; (2) cerca-la a la taula `I2:J4` i agafa la longitud de la pista corresponent (columna `J`); (3) divideix aquesta longitud pels segons finals de la fila (`M`). *Funció per al pas 2: [`XLOOKUP`](https://support.google.com/docs/answer/12405947?hl=en).*

> *Velocitat* = (longitud de la pista *que toca a aquesta categoria*) ÷ segons finals

**[1 punt]**

---

## Part B · Regex amb els correus (1,5 punts)

La columna `G` (`Contacte`) té un correu per fila, com `voltors.zenit@robotica.escola.cat`. Un correu **correcte** té exactament aquesta forma: *lletres minúscules* (equip), un *punt*, *lletres minúscules* (robot), i acaba en `@robotica.escola.cat`. No pot portar espais, majúscules ni números, ni res abans o després. Per exemple, `quark.nova@robotica.escola.com` *no* és correcte (domini diferent). Hi ha correus incorrectes a les dades.

**B1.** **Usuari** (columna `P`; títol `Usuari` a `P1`, fórmula a `P2`, omple fins a `P25`). Ha de mostrar només el que hi ha *abans* de l'`@` del correu de la columna `G`, sense l'`@` ni el domini (a la fila 2: `voltors.zenit`). *Funció: [`REGEXEXTRACT`](https://support.google.com/docs/answer/3098244?hl=en).*

> *Usuari* = el tros del correu que hi ha abans de l'`@`

**[0,5 punts]**

**B2.** **Correu vàlid** (columna `Q`; títol `Correu vàlid` a `Q1`, fórmula a `Q2`, omple fins a `Q25`). Per a cada fila, comprova el correu de `G`: `TRUE` si té *exactament* la forma correcta descrita a dalt, `FALSE` si no. El patró de regex l'has d'escriure tu. *Funció: [`REGEXMATCH`](https://support.google.com/docs/answer/3098292?hl=en).*

> *Correu vàlid* = el correu *encaixa del tot* amb: lletres minúscules, un punt, lletres minúscules i el domini fix

**[0,75 punts]**

**B3.** A la cel·la `S1`, una fórmula que doni *quants correus no són vàlids*, mirant els resultats de la columna `Q` (files 2 a 25). *Funció: [`COUNTIF`](https://support.google.com/docs/answer/3093480?hl=en).*

> *Invàlids* = comptar quantes files de la columna de correu vàlid diuen `FALSE`

**[0,25 punts]**

---

## Part C · Dashboard (3 punts)

Crea una pestanya nova anomenada `Resum`. Les fórmules d'aquesta pestanya *llegeixen les dades de la pestanya `Dades`* (columnes `A` equip, `C` categoria, `F` infraccions, `M` segons finals, `N` medalla, files 2 a 25). Tot ha d'estar fet amb fórmules, no a mà.

**C1.** **Equip.** A `A1` escriu `Equip`. A `B1`, un desplegable (`Dades` → `Validació de dades` → llista) amb els 6 equips de la lliga (els noms de la columna `A` de `Dades`). Tria'n un: les cel·les de sota es calculen amb l'equip que hi hagi a `B1` i han de canviar quan en triïs un altre. **[0,5 punts]**

**C2.** **Resultats de l'equip triat.** A la columna `A` escriu les etiquetes `Recorreguts`, `Infraccions`, `Millor temps` i `Ors` (files 2 a 5), i a `B2:B5` la fórmula de cada una: **[0,75 punts]**

- `B2`: nombre de recorreguts. *Funció: [`COUNTIFS`](https://support.google.com/docs/answer/3256550?hl=en).*
  > *Recorreguts* = comptar les files on l'equip és l'equip triat
- `B3`: infraccions totals. *Funció: [`SUMIFS`](https://support.google.com/docs/answer/3238496?hl=en).*
  > *Infraccions* = sumar les infraccions de les files on l'equip és el triat
- `B4`: millor temps final (en segons). *Funció: [`MINIFS`](https://support.google.com/docs/answer/7014063?hl=en).*
  > *Millor temps* = el mínim dels segons finals de les files on l'equip és el triat
- `B5`: medalles d'or (equip i medalla *Or*). *Funció: [`COUNTIFS`](https://support.google.com/docs/answer/3256550?hl=en)* (amb dues condicions).
  > *Ors* = comptar les files on l'equip és el triat *i* la medalla és Or

**C3.** **Millor temps per categoria.** A `A8:A10` escriu les tres categories (`Velocitat`, `Laberint`, `Sumo`) i a `B8:B10` el millor temps final (en segons) de cada una, amb una fórmula que agafi la categoria de la cel·la de la seva fila (`A8`, `A9`, `A10`). *Funció: [`MINIFS`](https://support.google.com/docs/answer/7014063?hl=en).*

> *Millor temps de la categoria* = el mínim dels segons finals de les files d'aquella categoria

**[0,75 punts]**

**C4.** **Gràfic de barres** de la taula `A8:B10` (`Insereix` → `Gràfic`), amb un títol i amb els eixos etiquetats de manera que s'entengui què mostra (categories i segons). **[0,5 punts]**

**C5.** **Format condicional** a la pestanya `Dades`, columna `N` (`N2:N25`): fes que cada cel·la agafi un color de fons segons la medalla (un color per a *Or*, un altre per a *Plata* i un altre per a *Bronze*; el `-` sense color). Menú `Format` → `Format condicional`, regla «El text és exactament». **[0,5 punts]**

---

## Part D · Teoria (1 punt, al fitxer `teoria.md`)

Respon aquestes preguntes amb frases completes al fitxer `teoria.md` del teu repositori. A la [plantilla](https://github.com/adriasantacreu/examen-prog-1bat-plantilla) (`teoria.md`) tens el títol i els apartats de cada resposta ja posats: copia'l al teu repositori i escriu-hi sota cada títol.

**D1.** Un company escriu a `M2` la fórmula `=E2*86400+F2*L2` i l'omple cap avall fins a `M3`. Escriu quina fórmula queda a `M3`, quin és el problema que causa i com s'ha de modificar la fórmula de `M2` perquè en omplir-la bé tot vagi correcte. **[0,5 punts]**

**D2.** Per validar un codi postal (cinc xifres) escrivim el patró `^\d{5}$` i no només `\d{5}`. Escriu un text concret on els dos patrons donarien resultats diferents i digues quin resultat (`TRUE`/`FALSE`) dona cada un. **[0,5 punts]**

---

## Part E · Test (1 punt: correcta +0,25, incorrecta −0,08, en blanc 0)

Respon el test al fitxer `test.md` del teu repositori. A la [plantilla](https://github.com/adriasantacreu/examen-prog-1bat-plantilla) tens l'esquelet de la taula (`Pregunta | Resposta`, una fila per pregunta): copia'l al teu repositori i escriu a la columna `Resposta` la lletra de cada pregunta. Si deixes la cel·la buida, la pregunta val 0. Com es fan les taules en Markdown: [guia de taules de Markdown](https://www.markdownguide.org/extended-syntax/#tables).

**1.** A `=SUMIFS(F2:F25,A2:A25,K1)`, el primer argument és:

- a) El criteri que es compara
- b) El rang on es busca el criteri
- c) El rang que se suma
- d) La cel·la on queda el resultat

**2.** `=IFS(M2<=60,"Or",M2<=90,"Plata")` amb `M2` igual a 100 dóna:

- a) `#N/A`
- b) `Plata`
- c) Una cel·la buida
- d) `0`

**3.** Per comptar els recorreguts amb alguna infracció, la fórmula correcta és:

- a) `=COUNT(F2:F25>0)`
- b) `=SUMIF(F2:F25,"0")`
- c) `=COUNTIF(F2:F25,>0)`
- d) `=COUNTIF(F2:F25,">0")`

**4.** Una durada de 26 h 30 min només es veu bé amb el format `[h]:mm:ss` perquè:

- a) Mostra els segons com a text
- b) Els claudàtors permeten superar les 24 hores
- c) Passa la durada a minuts
- d) Converteix el nombre en data

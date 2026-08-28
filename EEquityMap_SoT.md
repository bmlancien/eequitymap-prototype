# EEquityMap — Source of Truth · Update (Session 2026-08-17)

**Status:** Patch zum Einpflegen in das bestehende Source-of-Truth-Dokument.
Vorschlag Versionssprung **v2.2 → v2.3** (bei Bedarf v3.0 — der Wegfall des
Schwellenmodells und die Terminologie-Überarbeitung sind substanziell).

**Hinweis zur Herkunft:** Dieses Update wurde ohne das aktuelle
Source-of-Truth-Dokument in der Sitzung erstellt (nur Screenshots + letzter
Design-Prompt lagen vor). Vor dem Merge mit dem echten Dokument abgleichen,
insbesondere bei Punkten, die frühere Sessions betreffen.

**Void-Markierung:** Das Zwischenartefakt `EEquityMap_Rebuild_Prompt.md` (diese
Session) enthielt einen 0–100-%-Schwellen-Slider. Das widerspricht dem bereits
in v2.x vollzogenen Rückzug des Prozent-/Schwellenmodells. Der Prototyp hat den
Slider korrekt nicht übernommen. Der Rebuild-Prompt ist damit **nicht
autoritativ**; maßgeblich ist der Fix-Prompt `EEquityMap_Fixes.md`.

---

## A. Kernentscheidungen — bestätigt / präzisiert

- **Konsens-Score = Minimum-Regel.** `Zubaukonsens(Zelle) = min(MW über alle
  gewählten Prinzipien)`. Bestätigt (min statt mean; mean verworfen, s. H).
- **Kein Schwellen-Slider.** Konsens wird als kontinuierliche GW-Größe
  ausgedrückt, nicht als Prozent-Score mit Schwelle. Konsistent mit dem Rückzug
  des dimensionslosen %-Modells aus v2.x.
- **Konflikt ohne Schwelle definiert.** Konsensfläche = jedes gewählte Prinzip
  baut hier noch (Zubaukonsens > kleiner Floor, z. B. > 5 % des Zellmaximums);
  Konfliktfläche = mindestens ein Prinzip lässt die Zelle fallen (Zubaukonsens
  ≈ 0, obwohl ein anderes Prinzip sie empfiehlt). „Konfliktflächen ausblenden"
  blendet die ≈0-Zellen aus.
- **Mindestens ein Prinzip** ist immer ausgewählt; das letzte Häkchen lässt sich
  nicht abwählen. Der 0-Prinzipien-Fall existiert nicht mehr.
- **Ausbauziel-Mechanik (Zeitstrahl).** Zellen werden in absteigender
  Konsens-Reihenfolge rekrutiert, bis das Ziel erreicht ist. Der Zähler „Zellen
  im Konsens" bleibt vom Ausbauziel unberührt; die Ziel-Abhängigkeit wird über
  den **Konsensanteil des Zubaus** ausgedrückt (fällt mit steigendem Ziel).
- **Blocker ist ein echtes Feature.** Blocker = Prinzip mit dem niedrigsten Wert
  (= Bestimmer des Minimums). Speist Minimumkarte und „Was begrenzt den
  Konsens?". Nicht mehr „illustrativ".
- **Gewichtung berührt den Konsens nicht.** Sie wirkt nur auf die
  Leistungs-/Zubauverteilung, nicht auf Konsens-Score oder Konflikt.
- **„davon Neuzubau"** leitet sich aus der gewählten Konfiguration (gewichtete
  Kombination) ab, NICHT aus dem Wert eines einzelnen Prinzips. Es gilt:
  Zubaukonsens ≤ Neuzubau ≤ Ausbau gesamt. Diese Teilmengen-Beziehung muss im
  Zelldetail auch **sichtbar** sein (verschachtelter Balken, s. E), nicht nur
  rechnerisch stimmen.
- **Kein Stale-State.** Alle angezeigten Zahlen werden bei jedem Zellklick und
  jedem Tabwechsel neu berechnet; nichts aus einer vorherigen Auswahl
  zwischengespeichert. Prüfbare Invarianten in JEDEM Zustand: „Zubaukonsens dieser
  Zelle" == begrenzend-Wert == Minimum über die gewählten Prinzipien; Zubaukonsens
  ≤ Neuzubau ≤ Ausbau gesamt; gefüllter Balkenanteil ≤ voller Balken. (Ein
  Stale-State-Bug wurde in der heruntergeladenen HTML-Version beobachtet, nicht
  zuverlässig reproduzierbar — besonders die Abfolge Zelle → Tab → andere Zelle.)

---

## B. Terminologie — geändert, muss gelockt werden

Der Prototyp verwendet eine neue Benennung. Bitte als verbindlich festschreiben
oder korrigieren:

| bisher (v2.2) | jetzt im Prototyp |
|---|---|
| Lastnähe | **Verbrauchsnah** |
| Bevölkerung | **Bevölkerungsnah** (+ neues Gegenstück **Bevölkerungsfern**) |
| Wertschöpfung (primär) | **Bruttowertschöpfungssensitiv** (in Experimentell verschoben) |
| Ökonomisch | **Ökonomisch optimiert** |
| Expert | **Erweitert** |
| Tab „Minimumkarte" | **Was begrenzt?** |
| Tab „Schnittmengen" | **Übereinstimmungen** |
| Tab „Positivplanung" | **Konsens vs. Planung** |
| Tab „Differenz" | **Konsens vs. Referenz** |
| Tab „Verteilungen" | **Vergleich** |

Neue experimentelle Prinzipien im Prototyp: Infrastrukturnah, Einkommenssensitiv,
Vorrang für kommunale Flächen, Gleicher Anteil an Gesamtfläche, Gleicher Anteil an
Potenzialfläche (mehrere als „Metrik in Vorbereitung" ausgegraut).

**Offene Inkonsistenz:** „Konsens" und „Zubaukonsens" werden im Prototyp
synonym verwendet. Einen der beiden Begriffe als Leitbegriff festlegen und
durchziehen.

---

## C. Ansichten / Tabs — finaler Stand

Tab-Reihe (Zubaukonsens als Default). Namen final:

1. **Zubaukonsens** — Karte des Minimums je Zelle
2. **Was begrenzt?** (früher „Minimumkarte") — je Zelle das blockierende Prinzip
   (kategorial nach Prinzipfarbe). Erfüllt die frühere Sticky-Note „ein Prinzip
   pro Hexagon".
3. **Vergleich** (früher „Verteilungen") — Einzelverteilungen je Prinzip
   nebeneinander (frühere small multiples; identisch mit der Einstiegs-Galerie,
   s. G). Zeigt nur Prinzip-Karten, keine Referenzen.
4. **Übereinstimmungen** (früher „Schnittmengen") — **kategoriale Flächenfarbe**
   (keine Heatmap): Zelle eingefärbt danach, welche Prinzipien sie empfehlen.
   „Empfiehlt" = Zelle über dem eigenen Median des Prinzips (fester,
   nicht bedienbarer Schwellenwert). **Legende: nur Einzelfarben je Prinzip,
   keine Kombinations-Kästchen** (bei vielen Prinzipien sonst zu lang; die
   Karteneinfärbung bleibt kombinatorisch). Darunter im **Erweitert**-Modus ein
   einklappbares **UpSet-Diagramm** als detaillierte Aufschlüsselung (Karte bleibt
   Übersicht, s. H — Reversal). **Dichte/Layout:** nach Zellzahl absteigend, nur
   Top ~8 Kombinationen, 0-Zellen-Kombinationen weglassen, Rest als Sammelspalte
   „+ N weitere"; Diagramm füllt die volle Breite, Punktmatrix direkt darunter und
   ausgerichtet; bei genau 2 Prinzipien UpSet nicht zeigen (nur 3 Zustände), ab 3
   einblenden.
5. **Konsens vs. Planung** (früher „Positivplanung") — **eine** Karte mit **einer
   divergierenden Skala** eines abgeleiteten Deckungswerts je Zelle (aus
   Zubaukonsens und Anteil ausgewiesen): Mitte = „deckt sich", Pole = „Planung
   über Konsens" bzw. „Konsens über Planung". KEINE Überlagerung zweier
   Intensitäten mehr (die frühere Blau+Violett-Überlagerung war unlesbar, s. F).
   Kennzahl als ZWEI Richtungen in der **rechten Spalte** („Deutschland gesamt",
   keine schwebende Karten-Box): „Von der Ausweisung trifft X % den Konsens" +
   „Vom Konsens sind erst Y % ausgewiesen" (beide nötig, s. F).
6. **Konsens vs. Referenz** (früher „Differenz") — s. F

Beide Vergleichs-Tabs beginnen mit „Konsens vs. …": der Konsens ist der feste
Bezugspunkt, Planung/Referenz das Verglichene; die gleiche Form macht sie als
Paar erkennbar.

**Kein UpSet-Diagramm** (verworfen, s. H).

**Dauerhafte Caption je Tab** (unter der Tab-Reihe, kein Tooltip, ≥ 14 px):
- Zubaukonsens: „Dunkel = viel Zubau, den alle gewählten Prinzipien mittragen."
- Was begrenzt?: „Farbe = das Prinzip, das hier den Konsens begrenzt."
- Übereinstimmungen: „Farbe = welche Prinzipien dieselbe Fläche empfehlen."
- Vergleich: „Je eine Karte pro Prinzip — so unterschiedlich verteilt jedes
  den Ausbau. Dunkel = viel."
- Konsens vs. Planung: „Vergleich: der Konsens gegenüber den offiziell
  ausgewiesenen Windgebieten. Kennzahl: wie stark sie sich decken." (einzige
  Stelle, die „ausgewiesene Windgebiete" am Ort der Nutzung erklärt — muss
  bleiben.)
- Konsens vs. Referenz: „Vergleich: der Konsens gegenüber der gewählten
  Referenzverteilung. Grün = mehr Konsens, Braun = mehr Referenz." Leerzustand
  (keine Referenz gewählt): „Wählen Sie unten eine Referenzverteilung, um den
  Vergleich zu sehen."

---

## D. Basic / Erweitert — finaler Split

Leitregel unverändert: **Basic beantwortet genau eine Frage — „Wo ist der Ausbau
über die Prinzipien robust?"**

**Basic:** Technologie (Wind/PV); Ausbauziel (Presets **und** Slider, gekoppelt);
4 Primärprinzipien; Tabs Zubaukonsens / Was begrenzt? / Vergleich;
Konfliktflächen-Toggle (nur bei 2+ Prinzipien); Zelldetail mit Zubaukonsens,
begrenzendem Prinzip, Empfehlung je Prinzip, Ausbau gesamt / Neuzubau.

**Erweitert zusätzlich:** experimentelle Prinzipien; Gewichtung; Granularität
(Basisauflösung **5 × 5 km**, s. K; Gemeinde/Landkreis „folgt");
Bestandsanlagen anrechnen + Segmentierung Bestand/Zubau/Gesamt; Konsens vs.
Planung; Übereinstimmungen; Konsens vs. Referenz; Referenzverteilungen;
Downloads / Methodik; Formeln in
den Steckbriefen; Rang / Anteil an Zielerreichung / Priorität / regionale
Zielleistung im Zelldetail.

**Geändert ggü. v2.2:** Der **Ausbauziel-Slider gehört in Basic** (nicht nur
Presets) — er ist die erzählerisch stärkste Interaktion.

**Trennlinie zwischen den Modi (verbindlich):** Basic = die Prinzipien und wo sie
übereinstimmen. Erweitert = alles, was diese Übereinstimmung an etwas anderem
misst (Vergleich). Damit ist **jeder Vergleich Erweitert-only**: externe
Referenzverteilungen, der Tab „Konsens vs. Referenz" und das Element „Mit einer
anderen Verteilung vergleichen". In Basic erscheint keine Vergleichs-Auswahl.

Regeln gegen Leaks: Bottom-Toolbar (Vergleichen / Download / Methodik) nur in
Erweitert; Bestand/Zubau/Gesamt nur bei aktivem Bestands-Toggle; Konflikt-Toggle
und die Tabs „Was begrenzt?"/„Übereinstimmungen" nur bei 2+ Prinzipien; keine
ausgegrauten Elemente in Basic.

---

## E. Rechte Spalte — Hybrid-Verhalten (neu)

**Skelett konstant, Inhalt tab-abhängig.** Immer: große Kennzahl oben,
Analyseblock darunter. Nur der Block wechselt mit dem Tab:

| Tab | Kennzahl oben | Analyseblock |
|---|---|---|
| Zubaukonsens / Was begrenzt? | Zubaukonsens gesamt (verschachtelter Balken: Anteil am Ausbauziel) | Was begrenzt den Konsens? |
| Verteilungen | Zubaukonsens gesamt | Summe je Prinzip |
| Konsens vs. Planung | „Deutschland gesamt" — zwei Zahlen: „Von der Ausweisung trifft X % den Konsens" + „Vom Konsens sind erst Y % ausgewiesen" (in der rechten Spalte, KEINE schwebende Karten-Box) | Deckung ausgewiesen vs. nicht |
| Übereinstimmungen | Zubaukonsens gesamt | Zusammenfassung (von allen / genau einem / keinem) + Top 5–6 Kombinationen, Rest hinter „Weitere anzeigen", keine 0-Zellen-Kombinationen |
| Konsens vs. Referenz | Differenz gesamt | größte Abweichungen (mit Richtung) |

**Zelldetail:** vollständig **tab-unabhängig** — Inhalt, Reihenfolge und
Darstellung bleiben beim Tabwechsel gleich, es gibt **keine** tab-abhängige
Hervorhebung eines Blocks (die frühere Hervorhebung wirkte wie ein Fehler und
wurde verworfen, s. H). Einzige tab-bezogene Ergänzung: Auf dem Differenz-Tab
zeigt das Zelldetail zusätzlich die Differenz **dieser** Zelle — das ist ein
fehlender Fakt, keine Hervorhebung.

**Neuzubau & Zubaukonsens verschachtelt darstellen:** „Neuzubau" und
„Zubaukonsens dieser Zelle" stehen direkt untereinander (nicht durch die
Prinzip-Aufschlüsselung getrennt) und teilen sich **einen** Balken: der volle
Balken = Neuzubau, der gefüllte/kräftige Abschnitt = Zubaukonsens, der blasse
Rest = nicht von allen Prinzipien mitgetragener Anteil. Caption: „Zubaukonsens =
der Anteil des Neuzubaus, den alle gewählten Prinzipien mittragen." Der blasse
Rest darf NICHT wie Konflikt aussehen (keine Rosa-Codierung). Bei einem einzelnen
Prinzip sind beide Werte identisch — dann keine Verschachtelung, nur ein Wert.
Grund: die zwei Zahlen (1,60 vs. 1,70 im Beispiel) sind korrekt und
unterschiedlich, wirkten aber getrennt wie ein Widerspruch — die
Teilmengen-Beziehung muss sichtbar sein, nicht nur rechnerisch stimmen.

**Derselbe verschachtelte Balken auf Deutschland-Ebene:** Die große Kennzahl
„Zubaukonsens gesamt" in der Deutschland-Übersicht bekommt die identische
Darstellung — Balken = Ausbauziel (z. B. 215 GW), gefüllter Abschnitt =
Zubaukonsens gesamt (z. B. 169,29 GW), blasser Rest = Zubau außerhalb des
Konsens. Caption: „Vom Ausbauziel liegen X GW im Konsens aller gewählten
Prinzipien." Der vorhandene Prozentwert („78,7 % des Ausbauziels") bleibt.
Gleiche Füll-/Blass-Logik und Caption-Struktur wie auf Zellebene, damit
erkennbar ist: dieselbe Idee in zwei Maßstäben (Konsens = Ausschnitt einer
größeren Zubaugröße).

**„Region auswerten" bei Zellauswahl:** Im Zelldetail ein Button „Region
auswerten" — setzt den Scope auf diese Zelle/Region (Karte zoomt/zentriert auf
die Zelle, übrige Hexagone abgedimmt, Scope-Label wechselt), zurück über den
bestehenden „Zurück zu Deutschland"-Button. Reine Mock-Interaktion.

**„Was begrenzt den Konsens?"-Lesbarkeit:** längerer Balken = stärkere
Begrenzung (Erklärzeile ergänzen); Balken codiert nur den GW-Verlust, Zellzahl
sekundär.

---

## F. Referenzen & „Konsens vs. Referenz" (neu / korrigiert) — nur Erweitert

Vergleich ist die Frage, die Basic bewusst nicht stellt. Der gesamte Abschnitt F
gilt daher **nur im Erweitert-Modus**. Der Tab „Vergleich" zeigt in beiden
Modi ausschließlich Prinzip-Karten; Referenzen erscheinen dort NICHT als eigene
Karten, sondern nur als Vergleichsziel im Differenz-Tab (s. u.).

**Positivflächen ≠ Potentialflächen** — verbindliche Unterscheidung (Begriffe
bleiben als Fachbegriffe bestehen; nur der Tab heißt jetzt „Konsens vs.
Planung"):

- **Positivplanung / Positivflächen** = offiziell ausgewiesene Windgebiete →
  Tab „Konsens vs. Planung". **Flächenanteil statt Ja/Nein:** Eine Zelle ist fast
  nie ganz oder gar nicht ausgewiesen — meist nur teilweise. Daher pro Zelle ein
  Feld „Anteil ausgewiesen" (0–100 %). Zell-Fakten: „Anteil ausgewiesen: X %" +
  „Zubaukonsens dieser Zelle: X,XX GW" (als GW-Zahl, NICHT „Im Konsens: ja/nein" —
  Konsens ist durchgängig eine kontinuierliche GW-Größe, kein Binär-Flag) + die
  Wort-Einordnung „Planung über Konsens" / „deckt sich" / „Konsens über Planung". Kennzahl als **zwei Richtungen** desselben Verhältnisses,
  jede mit ausgeschriebener Richtung und sichtbar parallel: „Von der Ausweisung
  trifft X % den Konsens" (ist das Geplante gut platziert?) + „Vom Konsens sind
  erst Y % ausgewiesen" (wird die faire Fläche geplant?). Beide nötig — die erste
  allein beschönigt, die zweite zeigt die Lücke; „erst" signalisiert die offene
  Lücke. Nie zwei nackte Prozentzahlen ohne Richtungstext. **Ort:** in der
  rechten Spalte („Deutschland gesamt", nur ohne Zellauswahl), NICHT als
  schwebende Box auf der Karte — die wirkte wie Zell-Rückmeldung und ließ
  fälschlich erwarten, dass sich die bundesweiten Zahlen beim Zellklick ändern.
  **Kartendarstellung (Entscheidung):** NICHT zwei Intensitäten überlagern (blauer
  Konsens + violetter Anteil ausgewiesen war unlesbar — zwei Skalen auf denselben
  Hexagonen ergeben Matsch). Stattdessen **eine** Karte eines abgeleiteten
  Deckungswerts (normierter Anteil ausgewiesen − normierter Zubaukonsens),
  divergierende Skala: Mitte „deckt sich", Pole „Planung über Konsens" /
  „Konsens über Planung". Skala von Prinzipfarben unterscheidbar, kein Rosa in
  Konflikt-Bedeutung. Legende = eine Achse mit drei Ankern. Zelldetail zeigt
  zusätzlich die Wort-Einordnung.
- **Potentialflächen** = technisch/rechtlich verfügbare Flächen → als externe
  Referenzverteilung.

**Externe Referenzverteilungen** (ökonomisch optimiert, historisch,
Potentialflächen) bleiben im Tool, aber:

- **nur in Erweitert.**
- **KEINE eigene Gruppe** in der linken Spalte. Der frühere Vorschlag einer
  eigenen Liste „Referenzverteilungen (extern)" unter den Prinzipien ist
  verworfen (s. H).
- Referenzen sind **ausschließlich Vergleichsziel**, erreichbar allein über die
  Auswahl „Wo unterscheiden sie sich?" / „Mit einer anderen Verteilung
  vergleichen". Sie werden nie als eigenständige, wählbare Verteilungskarte
  gezeigt — nur als Differenz gegen den Konsens (grün/braun, s. u.).
- Vorteil: Eine Referenz kann nicht mehr mit einem wählbaren Prinzip verwechselt
  werden — sie ist eindeutig „etwas, wogegen man vergleicht", nie „etwas, das man
  auswählt".

**Differenz-Regeln (Korrektur eines echten Fehlers):** Die frühere Rechnung
`Konsens − gewähltes Prinzip` ist strukturell immer ≤ 0 (das Minimum kann keinen
seiner Bausteine übertreffen) und damit informationslos. Daher:

- Verglichen wird nur gegen ein **nicht gewähltes Prinzip** oder eine **externe
  Referenz**. Vergleich gegen ein gewähltes Prinzip wird nicht angeboten (das ist
  bereits „Was begrenzt den Konsens?").
- **Richtung statt Vorzeichen anzeigen:** „2,97 GW mehr bei X" statt „−2,97 GW",
  in der Sprache der Kartenlegende. Die Differenz-Karte (grün/braun) ist bereits
  korrekt; nur die Sidebar-Liste war falsch.

---

## G. Verständlichkeit / Erststart (geändert)

**Kein Onboarding-Modal mehr** (entfernt, s. H). Die Konzepterklärung, die früher
das Modal trug, liegt jetzt auf dem Erststart-Zustand der Hauptansicht und im
Info-Panel „Was ist das?". Der Nutzer landet also kalt — der Erststart muss das
Werkzeug daher nicht nur bestätigen, sondern erstmalig erklären.

**Erststart = Vergleichs-Galerie (difference-first):**

- Beim ersten Laden zeigt die Kartenfläche **alle Prinzip-Verteilungen als kleine
  Karten nebeneinander** (Galerie / small multiples), nicht eine leere Karte und
  nicht die Aufforderung, ein einzelnes Prinzip zu wählen. Der Nutzer sieht
  zuerst, DASS die Prinzipien unterschiedlich verteilen — das ist das Argument
  für den Konsens, visuell, bevor der Konsens gezeigt wird.
- Rahmenzeile über der Galerie: **eine** Headline „So unterschiedlich verteilen
  die Prinzipien den Ausbau — und wo sie sich einig sind." + kleine Legende „Je
  eine Karte pro Prinzip · dunkel = viel empfohlener Zubau". Frühere doppelte
  Zeilen (separate erste Headline bzw. „Jedes Gerechtigkeitsprinzip verteilt den
  Ausbau anders …") sind gestrichen. Je Karte eine kurze Erklärzeile (s. u.).
- **Diese Galerie IST der Vergleich-Tab** — eine Ansicht, nicht zwei. Beim
  Erststart zeigt sie alle Prinzipien; später (mit Auswahl) die gewählten. Die
  Vergleichsansicht ist damit jederzeit über den Tab wieder erreichbar.
- **Primäraktion „Wo sind sich die Prinzipien einig? →"** (Klartext, nicht der
  Fachbegriff „Zubaukonsens"): **direkt unter der Kartenreihe** mit kleinem festen
  Abstand (~24–32 px), als Teil desselben zentrierten Blocks (Framing-Zeile +
  Legende + Galerie + CTA gehören zusammen) — NICHT am Seitenende, NICHT mittig in
  der leeren Restfläche. **Blendet ca. 2–3 s nach Betreten sanft ein** (erst
  Karten scannen, dann der nächste Schritt), wählt das Trio Verbrauchsnah +
  Bevölkerungsnah + Ökonomisch optimiert und wechselt in den Zubaukonsens-Tab.
- **Klick auf eine Galerie-Karte → Vollbild-Einzelverteilung dieses Prinzips
  (Drill-in), und die Checkbox des Prinzips wird gesetzt** (Drill-in = Auswahl).
  Die Karten müssen als **klickbar erkennbar** sein (Hover-Zustand, Cursor
  pointer, dezenter „Einzeln ansehen →"-Hinweis) — sie sind der leise, verfügbare
  Nebenweg (einzelnes Prinzip erkunden), NICHT so laut wie der CTA und kein
  zweiter prominenter Button. Der CTA steht darum bewusst UNTER der Galerie
  (verzögert eingeblendet, s. o.): erkunden oben, weitergehen unten — das
  entflechtet die zwei Pfade und hält die Reihenfolge Unterschied → Konsens.
  So läuft der bestehende Pfad „zweites Prinzip hinzufügen → Konsens" unverändert
  weiter. Zurück über einen deutlichen Link „← Alle Prinzipien vergleichen" (kein
  Hintergrund-Klick).
- **Mindestens-ein-Prinzip** greift erst nach der ersten Wahl; null Prinzipien
  nur beim Erststart. Zurück in die Vergleichsansicht läuft über den
  Vergleich-Tab, nicht über das Abwählen aller Prinzipien.

- **Einmaliger Prinzip-Hinweis nach dem CTA-Einstieg:** Landet der Nutzer über den
  CTA auf der Zubaukonsens-Karte (drei vorausgewählte Prinzipien, die er nicht
  selbst gewählt hat), erscheint einmalig **inline direkt unter der Überschrift
  „GERECHTIGKEITSPRINZIPIEN"** (kein schwebendes Overlay): „Dieser Konsens beruht
  auf drei Prinzipien. Ändern Sie die Auswahl, um zu sehen, wie er sich
  verschiebt." Die drei aktiven Checkboxen bekommen eine **ruhige, dauerhafte
  Hervorhebung** (Tint/farbige Kante, KEIN Puls — ein kurzer Puls wird übersehen,
  weil der Blick auf der Karte liegt). **Ausblenden nicht per Timer/Cursor:**
  Hinweis und Hervorhebung bleiben, bis der Nutzer ein Prinzip ändert (verstanden,
  handelt) ODER anderswo interagiert (Zelle/Tab — gesehen, weitergezogen), dann
  gemeinsam ausblenden. Kein Schließen-Button. Einmal pro Sitzung (im Speicher,
  keine Persistenzlogik). Zweck: erklärt am Live-Moment die Kausalität Prinzipien →
  Konsens und führt den Flow weiter.

Damit lehrt das Werkzeug die Konzepte in natürlicher Reihenfolge: Unterschied
(Galerie) → einzelnes Prinzip (Drill-in) → Konsens. Difference-first statt
answer-first.

**Prämissen-Overlay beim Start (Inhalt festgelegt):** Kurzes, schließbares
Ein-Schirm-Modal, das die Idee vermittelt, BEVOR das Wort „Gerechtigkeitsprinzip"
fällt (konkret vor abstrakt). Aufbau: (1) Titel „Wo wäre der Ausbau fair?";
(2) Prämisse „…die eine richtige Verteilung gibt es nicht"; (3) **Bild aus drei
echten, sichtbar unterschiedlichen Prinzip-Karten** unter „Dieselbe Aufgabe,
verschiedene Antworten.", beschriftet über die FRAGE statt den Begriff („Wo der
Strom verbraucht wird" / „Wo mehr Menschen wohnen" / „Wo es am günstigsten ist");
(4) erst danach der Begriff: „Jede solche Vorstellung ist ein
Gerechtigkeitsprinzip. EEquityMap … zeigt, wo sie sich einig sind."; (5) CTA
„Verteilungen ansehen →", Link „Mehr dazu" → „Was ist das?". Keine Stockfotos —
nur die drei Karten. Lässt nichts auswählen, erzwingt keinen Pfad. Jederzeit über
„Was ist das?" wieder aufrufbar. **Abgrenzung zum verworfenen Menü-Modal:** kein
Pfad, keine Auswahl, nur Prämisse + Beispiel.

**Trio des Startpunkts:** Die Galerie-Primäraktion („Wo sind sich die Prinzipien
einig? →", s. o.) wählt **Verbrauchsnah + Bevölkerungsnah + Ökonomisch
optimiert** — drei unterschiedliche Logiken, KEINE Gegensatzpaare. (Das frühere
Trio enthielt Bevölkerungsnah und Bevölkerungsfern als bewusste Gegensätze und
ließ den ersten gezeigten Konsens künstlich niedrig ausfallen.) Voraussetzung:
**Ökonomisch optimiert gehört ins Basic-Primärset** (Verbrauchsnah,
Bevölkerungsnah, Bevölkerungsfern, Ökonomisch optimiert).

**Landing verstärkt das Konzept:** Modal und Einstiegs-Galerie tragen die Kernidee
gemeinsam — das Modal nennt und zeigt sie (Prämisse + drei Beispielkarten), die
Galerie zeigt sie erneut über alle Prinzipien. Die Galerie-Headline/Erklärzeile
erklärt weiterhin das Werkzeug („wo sich die Prinzipien einig sind"), nicht nur
die Karte („dunkel = viel").

**Erklärzeile je Prinzip (Vergleich / Einzelverteilung):** Prinzipname fett,
darunter eine kurze Erklärzeile, **außerhalb/unterhalb der Kartengrafik** (nicht
darübergelegt) und in lesbarer Größe (Mindestgröße s. K). Name + Erklärzeile sind
primär, die Kartengrafik ist die Illustration. Inhalt z. B. „Verbrauchsnah
verteilt den Ausbau dorthin, wo viel Strom verbraucht wird — dunkle Zellen
bekommen mehr." Nicht den Steckbrief wiederholen (der bleibt im Info-Icon). In
den Small-Multiples je Karte maximal eine Zeile; bei breiten Karten die Zeile
nicht über die volle Breite laufen lassen.

**Weitere bestätigte Verständnishilfen:**

- Info-Panel „Was ist EEquityMap?" mit den modularen Erklärtexten (Warum das
  Tool / Was ist ein Prinzip / Wie entsteht Konsens / Was das Tool nicht
  leistet). Textbausteine liegen vor. **Zusätzlich ein durchgerechnetes
  Beispiel:** „In einer verbrauchsstarken Region empfiehlt Verbrauchsnah viel
  Ausbau, Bevölkerungsnah wenig — hier sind sich die Prinzipien uneinig; wo beide
  hoch bewerten, entsteht Konsens." Optional dieselbe Aussage als Ein-Zeiler live
  beim ersten Klick auf eine Konfliktzelle (wirkt am stärksten, wenn sie gerade
  zutrifft).
- **Dauerhafte Erklärzeile je Tab** direkt unter der Tab-Reihe (kein Tooltip),
  die sagt, was die Farbe bedeutet.
- Headline über der Karte ändert sich mit jedem Tab.
- „So-what"-Zeile in der Deutschland-Übersicht: „Je höher dieser Anteil, desto
  robuster lässt sich das Ziel über die Prinzipien hinweg erreichen."
- „Zubaukonsens" wird bei erstem Auftreten in situ definiert (Caption, nicht
  vorausgesetzt).
- Unterzeile der Prinzipienliste: „Kein Prinzip ist objektiv richtig …".
- Dauerhinweis „Konsens = robust, nicht automatisch genehmigungsfähig." bleibt
  immer sichtbar.
- Einzelansichten sind dauerhaft als solche markiert („Einzelverteilung eines
  Prinzips — nicht der Konsens"), damit kein Einzelprinzip- oder Einzelzell-Blick
  mit der Aussage des Werkzeugs verwechselt wird.

---

## H. Verworfene Ideen (mit Begründung) — für den Reject-Log

- **Konsens vs. Planung als Doppel-Intensität (blauer Konsens + violetter
  Ausweisungsanteil überlagert)** — gebaut, dann verworfen. (Grund: zwei
  Intensitätsskalen auf denselben Hexagonen sind nicht trennbar — jede Zelle wird
  ein Blau-Violett-Matsch, keine der beiden Größen ist ablesbar, und die
  Relation, um die es geht, erst recht nicht. Ersetzt durch eine Karte eines
  abgeleiteten Deckungswerts, divergierende Skala, s. C/F.)
- **„Im Konsens: ja/nein" als Zell-Fakt (Konsens vs. Planung)** — verworfen
  zugunsten der GW-Zahl. (Grund: binärer Rückfall — Konsens ist im ganzen Werkzeug
  eine kontinuierliche GW-Größe, seit das Schwellenmodell abgelöst wurde. Ein
  Ja/Nein-Flag verbirgt die Zahl, die anderswo ohnehin gezeigt wird, und
  widerspricht der Kernentscheidung. Die Wort-Einordnung „deckt sich / Planung
  über Konsens / Konsens über Planung" trägt die Relation weiterhin.)
- **UpSet-Diagramm** — zunächst verworfen (Schnittmengen als kategoriale
  Flächenfarbe genügten als Übersicht), dann **wieder aufgenommen als
  Erweitert-Detail UNTER der Übereinstimmungen-Karte** (Karte bleibt Übersicht,
  UpSet ist die einklappbare Aufschlüsselung). Kein Ersatz der Karte, kein
  eigenes mentales Modell im Basic-Modus. Reversal bewusst.
- **Prozent-/Schwellenmodell mit Slider** — endgültig verworfen (bereits in v2.x
  begonnen). Konsens ist eine GW-Größe ohne Schwelle. Der zwischenzeitliche
  Rebuild-Prompt mit %-Schwelle ist void.
- **Mittelwert-Score** — verworfen zugunsten Minimum. (Grund: Mittelwert kann
  Übereinstimmung nicht von Niveau unterscheiden — je ein Prinzip bei 100 und 0
  ergäbe denselben Wert wie beide bei 50. Das Minimum misst echte Robustheit.)
- **Referenzen in der Prinzipienliste** — verworfen. (Grund: ließ Referenzen wie
  wählbare Prinzipien aussehen.)
- **Eigene Gruppe „Referenzverteilungen (extern)" in der linken Spalte** —
  ebenfalls verworfen. (Grund: zwischenzeitlich beschlossen, dann verworfen —
  auch eine separate, gleich dargestellte Gruppe konkurriert visuell mit den
  Prinzipien. Referenzen sind jetzt reines Vergleichsziel und nur über „Wo
  unterscheiden sie sich?" erreichbar, nie als eigenständige wählbare Karte.)
- **Voller Tausch der rechten Spalte je Tab** — verworfen zugunsten Hybrid.
  (Grund: Karte + ganze Spalte gleichzeitig neu = zu viel Bewegung.)
- **Vergleich Konsens gegen ein gewähltes Prinzip** — nicht angeboten (immer
  negativ, dupliziert „Was begrenzt den Konsens?").
- **Tab-abhängige Hervorhebung eines Blocks im Zelldetail** — verworfen. (Grund:
  ohne benannte Kennzeichnung las sich die Hervorhebung als Darstellungsfehler.
  Der eigentliche Wert liegt in der stabilen, tab-unabhängigen Detailansicht; eine
  verwirrende Hervorhebung ist schlechter als keine. Das Zelldetail bleibt in
  jedem Tab identisch.)
- **Onboarding-Modal als Vier-Karten-Menü („Was möchten Sie untersuchen?")** —
  entfernt. (Grund: es war ein Navigations-Menü, das sich als Intro ausgab, und
  zwang den Nutzer, einen Pfad zu wählen, bevor er irgendetwas verstand. NICHT
  verwechseln mit dem später ergänzten Prämissen-Overlay, s. G: das nennt nur die
  Prämisse, lässt nichts auswählen und erzwingt keinen Pfad. Verworfen ist das
  Pfad-Menü, nicht jede Form von Intro.)
- **Erststart „leere Karte, ein Prinzip wählen" (pick-one / build-up)** —
  gebaut, dann abgelöst. (Grund: verzögerte die Kernidee. „Ein Prinzip macht eine
  Verteilung" wird gezeigt, aber der eigentliche Punkt — die Prinzipien
  widersprechen sich — kam erst nach mehreren Klicks. Ersetzt durch die
  difference-first-Galerie: alle Verteilungen zuerst nebeneinander, dann Drill-in,
  dann Konsens. Unterschied ist sichtbar, Übereinstimmung ist abstrakt — deshalb
  Unterschied zuerst.)

---

## I. Offene Fragen — für den Open-Questions-Log

1. **Farbsystem: Grün/Teal vs. Steel Blue.** Der Prototyp nutzt durchgängig
   Grün; das Briefing sah Steel Blue vor. Bewusster Rebrand oder Drift?
   Unabhängig davon: die Dreifachbelegung von Grün (Marke + Konsensskala +
   Prinzipfarbe Bevölkerungsfern) auflösen.
2. **Schnittmengen-Schwelle „empfiehlt".** Aktuell „über dem eigenen Median".
   Fester Wert bestätigen oder anpassen.
3. **Vergleich gegen ein gewähltes Prinzip über die gewichtete
   Kombination** statt über das Minimum — möglich, aber „meine Auswahl" bedeutet
   dann Zubauplan statt Konsens-Floor. Anbieten oder nicht?
4. **Sechs Tabs in Erweitert.** „Was begrenzt?" / Vergleich / Übereinstimmungen
   beantworten alle „wie unterscheiden sich die Prinzipien?". Ggf. unter einer
   Ansicht „Unterschiede" bündeln, falls Verständlichkeit weiter schwierig. (Das
   wäre auch der Moment, die grammatisch gemischten Tab-Namen zu vereinheitlichen.)
5. **Verteilungen-Sidebar-Label:** „Summe je Prinzip" in beiden Modi (Referenzen
   erscheinen dort nicht mehr als Karten). *Entschieden — siehe D/F.*
6. **Bestandsanlagen: Basic oder Erweitert?** Aktuell Erweitert. Falls die
   Bestands-Logik politisch zentral ist, nach Basic — mit Zusatz-Control als
   Preis.
7. **„Konsens vs. Planung": Basic oder Erweitert?** Aktuell Erweitert. Die
   71-%-Kennzahl ist der konkreteste Politik-Payoff — ggf. nach Basic hochziehen.
8. **Primärset der Prinzipien driftet zwischen Builds.** Basic zeigte mal
   {Verbrauchsnah, Bevölkerungsnah, Bevölkerungsfern, Ökonomisch optimiert}, mal
   {… Gleicher Anteil an Gesamtfläche, Gleicher Anteil an Potenzialfläche} ohne
   Ökonomisch optimiert. Das Basic-Primärset verbindlich festschreiben (das
   Beispiel-Trio setzt Ökonomisch optimiert voraus). Ebenso festschreiben, welche
   Prinzipien primär, welche experimentell und welche „Metrik in Vorbereitung"
   sind — analog zur Terminologie-Sperre (B).

---

## J. Parameter-/Konsistenz-Audit (Ergänzung)

Beim nächsten Review gegen das Backend-Modell prüfen, dass die neue
Terminologie (B) und die neuen experimentellen Prinzipien mit vorhandenen
Backend-Metriken hinterlegt sind bzw. korrekt als „Metrik in Vorbereitung"
gekennzeichnet bleiben. Der bekannte Grundsatz gilt weiter: eine Datenquelle
für Score, Häkchen/Begrenzer, Kartenfarbe und Blocker-Auswertung.

---

## K. Typografie-Untergrenzen (Design-System-Regel)

Verbindliche Mindestgrößen für die gesamte Anwendung:

- **Kein Fließ-/Info-Text unter 14 px.**
- **Keine Zahl unter 13 px.**

Gilt auch für neue Elemente. Beim Einführen der Regel besonders zu prüfen:
Karten-Erklärzeilen und -Wertebereiche, Slider-Beschriftungen (Jahres-Marken und
GW-Enden), Microcopy unter Toggles/Reglern, Hinweistexte der rechten Spalte,
Tab-Erklärzeilen und Legenden.

---

## L. Rasterauflösung / Ebenen (geändert)

Zwei Ebenen mit zwei Einheiten (s. M):
- **National (DE-Ansicht):** Einheit = **Landkreis**. Scope-Label „Deutschland ·
  Landkreise". Keine 5-×-5-km-Zellen auf dieser Ebene.
- **Regional (Regionsansicht):** Einheit = **5 × 5 km** Zelle (früher 10 × 10 km),
  nur innerhalb einer Region sichtbar. Scope-Label „[Landkreis] · 5 × 5 km
  Raster".

Der frühere Granularitäts-Schalter geht in der DE-/Region-Aufteilung auf (s. M).
Die schematische Mock-Zellzahl kann bestehen bleiben; nur Beschriftung/Geometrie
je Ebene wie oben (bei echten Daten entspräche 5 × 5 km einer feineren Zellzahl
innerhalb jedes Landkreises).

---

## M. Regionale Auswertung & Zwei-Ebenen-Kriterien (neu, Research-Anforderung)

**Mentales Modell in einem Satz:** Die nationalen Prinzipien entscheiden, **wie
viel jede Region bekommt**; die regionalen Prinzipien entscheiden, **wo innerhalb
der Region** es hinkommt. Zwei geschachtelte Verteilungsschritte; das Ergebnis
von Schritt 1 ist das feste Budget von Schritt 2.

**Verfügbarkeit:** in Basic UND Erweitert.

**Zwei Modi (zwei Geometrien, nicht zwei Zooms desselben Rasters):**
- **DE-Ansicht** — ganz Deutschland, Einheit ist der **Landkreis**. Das Land wird
  als ~400 Landkreis-Formen gezeigt (schematische Hexagone, ein Hexagon = ein
  Landkreis, eingefärbt nach dem Aggregatwert). **Keine 5-×-5-km-Zellen auf dieser
  Ebene.** Scope-Label: „Deutschland · Landkreise". Verteilt das nationale Ziel
  über die Landkreise nach den **nationalen Prinzipien**. Schnell/reaktiv.
- **Regionsansicht** — ein Landkreis, dargestellt als seine **5-×-5-km-Zellen**,
  bewusst als **schematische Rechtecke** (nicht Hexagone), damit die andere Ebene
  visuell sofort erkennbar ist. 5 × 5 km existiert als sichtbare Einheit **nur
  hier**. Scope-Label: „[Landkreis] · 5 × 5 km Raster". Verteilt das für diesen
  Landkreis berechnete Budget über seine Zellen nach den **regionalen Prinzipien**.

**Einstieg:** Klick auf eine Zelle (DE-Ansicht) → **Popover an der Zelle
verankert** mit der Aktion **„[Landkreis] im Detail auswerten →"** (Regionsname
konkret genannt). **Gestaltung:** an der Zelle verankert mit sichtbarem Zeiger,
sitzt NEBEN der Zelle (überdeckt weder Zelle noch Nachbarn), kompakt (Chip-Größe,
keine Banderole), Grün gedämpft (überstrahlt die Karte nicht). Enthält nur die
Aktion (kein zweites Detail-Kärtchen — das Zelldetail bleibt in der rechten
Spalte); nicht flüchtig, bleibt bei ausgewählter Zelle, verschwindet bei Klick auf
den Kartenhintergrund. (Der frühere unscheinbare „Region auswerten"-Button oben in
der rechten Spalte war zu leicht zu übersehen; die erste Popover-Version war ein
großer schwebender Block ohne Zellbezug — daher die Verankerungs-/Kompaktheits-
Vorgaben.) Rechte Spalte bleibt inhaltlich zunächst gleich, nur der Titel wechselt
(Landkreis X → Zelle Y).

**Mode-Transition sichtbar machen** (der Wechsel DE → Region war zu leise):
- **Einmaliger Banner** beim Betreten der Regionsansicht: „Sie werten jetzt
  [Landkreis] im Detail aus — die nationale Verteilung bleibt oben als Kontext."
- **Erst-Hinweis** an der Liste „Verteilung innerhalb der Region": „Startet mit
  denselben Prinzipien wie national. Ändern Sie sie, um innerhalb der Region
  anders zu verteilen." (macht die Opt-in-Natur sichtbar).
- **Breadcrumb** „Auf Regionen verteilt nach: …" bekommt echtes Gewicht (nicht
  blasse Microcopy) — es ist die einzige sichtbare Verbindung der zwei Ebenen.

**Kriterien auf zwei Ebenen (Kern der Anforderung):**
- Die zwei Prinzipiensätze werden **strukturell parallel** benannt — beide
  „[Skala] verteilt nach:", nur das Skalen-Wort unterscheidet sie, damit sie als
  zwei Pole EINER Achse lesbar sind: **„Bundesweit verteilt nach: …"** (national) /
  **„Innerhalb der Region verteilt nach: …"** (regional). NICHT beide
  „Gerechtigkeitsprinzipien", und NICHT „Auf Regionen verteilt" (das kontrastiert
  nicht sauber mit „innerhalb").
- In DE-Ansicht ist die linke Liste der nationale Satz („Bundesweit verteilt
  nach: …").
- In Regionsansicht **wird die linke Liste zum regionalen Satz** (gleiche
  Position, „Innerhalb der Region verteilt nach: …"). Der nationale Satz steht
  darüber **eingeklappt/read-only als Kontext-Breadcrumb** („Bundesweit verteilt
  nach: … ▾"). So gibt es immer nur EINE editierbare Liste, die andere Ebene
  bleibt als Breadcrumb sichtbar — verhindert die „welche Prinzipien stelle ich
  gerade ein?"-Verwirrung. Parallelität = derselbe Trick wie bei „Konsens vs. …"
  und den zwei Planung-Kennzahlen: zwei Versionen einer Beziehung strukturell
  gleich halten, damit nur der Unterschied variiert.

**Default (verbindlich, gegen Komplexität):** Beim Betreten der Regionsansicht
sind die **regionalen Prinzipien identisch mit der nationalen Auswahl**
vorbelegt. So liest sich die Regionsansicht zuerst als „dieselbe Logik, nur
gezoomt". Die Zwei-Ebenen-Komplexität ist damit **opt-in** — sie erscheint erst,
wenn der Nutzer die regionalen Prinzipien bewusst ändert. (Die Slide zeigte
unterschiedliche Sätze nur, um die Fähigkeit zu illustrieren, nicht als Default.)

**Konsens auf beiden Ebenen:** Minimum-Regel gilt auf beiden Ebenen. Die
Regionsansicht rechnet **einen eigenen Konsens** unter den regionalen Prinzipien
(verteilt das feste Regionsbudget). „Zubaukonsens" bezeichnet damit zwei Zahlen
auf zwei Skalen — immer **scope-beschriftet** halten („… dieser Region" vs. „…
dieser Zelle in der Region"), damit kein Verwechslungs-/Binär-Rückfall entsteht.

**Rechenzeit-Asymmetrie (wichtig):**
- **DE-Ansicht ist schnell/reaktiv** (Toggle → sofort), wie der Rest des
  Werkzeugs.
- **Nur die Regionsansicht** hat eine **echte Backend-Rechenzeit (~30 s)**. Daher
  eigener Button **„Berechnung starten"** mit **festem Platz unten an der Liste
  „Innerhalb der Region verteilt nach: …"** (nicht in der Kartenmitte, nicht
  verschwindend).
- **Auto-Berechnung beim ersten Betreten, manuell danach:** Beim ersten Betreten
  einer Region läuft die Berechnung **automatisch einmal** (mit Spinner), sodass
  sofort ein Ergebnis erscheint (der Popover-Einstieg soll belohnend sein, kein
  leerer Screen). Nach einer Änderung eines regionalen Prinzips **kein
  Auto-Recompute** — Anzeige geht auf „nicht aktuell", Button erscheint als
  **„Neu berechnen"**, erst Klick rechnet neu.
- **Headline zeigt das Regionsbudget, nie „0,00 GW":** Vor/während der Rechnung
  „Regionsbudget [X] GW — wird verteilt …"; danach „Von [Budget] GW Regionsbudget
  liegen [Y] GW im Konsens der regionalen Prinzipien." Das nationale Ausbauziel
  gehört NICHT in die Regions-Headline. (Der frühere „0,00 GW"-Zustand war ein
  Bug — die Region hat immer ein national berechnetes Budget.)
- Nach einer Änderung der regionalen Prinzipien muss der Zustand **sichtbar
  „nicht aktuell"** sein (Karte abgedimmt / Hinweis), bis „Neu berechnen" gedrückt
  wird — sonst wirkt es kaputt. Fortschritts-/Spinner-Zustand während der
  Rechnung. Im Mock die Verzögerung ehrlich nachbilden (~20–30 s), damit Tests den
  echten Rhythmus erleben (configure → compute → read statt toggle → instant).
- **Banner in der Regionsansicht:** nur der einmalige Mode-Transition-Banner; der
  DE-Ansicht-Hinweis „Zwei Prinzipien gewählt …" gehört NICHT hierher. „Noch nicht
  berechnet" nur an einer Stelle (erscheint durch die Auto-Berechnung ohnehin nur
  kurz als „wird berechnet …").

**Karten-Abstraktion:** weiterhin schematisch, KEINE echten Geodaten
(vg250_krs.gpkg wird für den Prototyp NICHT gerendert — widerspräche der
Gründungsvorgabe „keine Live-Karte"). Deutschland = Hexagone (Landkreis-Ebene
schematisch), Region = Rechtecke. Reale Geodaten sind ein separates, späteres,
größeres Arbeitspaket, erst nach Konzept-Validierung.

**Granularität aufgelöst:** 5 × 5 km ist NICHT die Auflösung des nationalen
Kartenbilds — national ist der Landkreis die Einheit; 5 × 5 km existiert nur
INNERHALB einer Region. Damit ist der Modus die Granularität: Der frühere
Granularitäts-Schalter („5 × 5 km / Gemeinde / Landkreis") ist durch die
DE-/Region-Aufteilung weitgehend abgelöst und sollte darin aufgehen, nicht als
separater Regler bestehen bleiben.
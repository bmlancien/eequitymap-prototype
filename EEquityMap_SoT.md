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
  ≈ 0, obwohl ein anderes Prinzip sie empfiehlt). ~~„Konfliktflächen ausblenden"
  blendet die ≈0-Zellen aus.~~ → Switch entfernt (s. O).
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

> **Teilweise abgelöst durch O (2026-10-09):** Reiter-Reihenfolge, Wegfall von
> „Konsens vs. Planung" / „Konsens vs. Referenz" als eigene Ansichten.

Tab-Reihe (Zubaukonsens als Default). Namen final:

1. **Zubaukonsens** — Karte des Minimums je Zelle.
   Legendentitel nennt die eingefärbte Einheit (Session 2026-10-12):
   „Zubaukonsens je Landkreis" (Deutschland), „… je Gemeinde" (Region, Ansicht
   „Gemeinden" bzw. Basic), „… je 5 × 5-km-Zelle" (Region, Ansichten mit Raster).
   Gleiches gilt für die Legende bei nur einem gewählten Prinzip: „Empfohlener
   Zubau je Landkreis / je Gemeinde / je 5 × 5-km-Zelle".
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

→ 5. und 6. entfallen als eigene Ansichten; Vergleiche nur noch im Hub
„Szenarien vergleichen" (s. O).

Beide Vergleichs-Tabs beginnen mit „Konsens vs. …": der Konsens ist der feste
Bezugspunkt, Planung/Referenz das Verglichene; die gleiche Form macht sie als
Paar erkennbar.

**Kein UpSet-Diagramm** (verworfen, s. H).

> **Neu gefasst (Session 2026-10-12):** Die Caption unter der Tab-Reihe sagt in
> ganzen Sätzen, was der Reiter zeigt — **keine Farblegende in Worten** („Dunkel
> = …", „Farbe = …"); die Farblegende steht unter der Karte. Aktuelle Texte:
> - Konsens: „Die Karte zeigt, wo alle gewählten Prinzipien denselben Zubau
>   mittragen."
> - Was begrenzt?: „Der Konsens ist überall nur so groß wie das Prinzip, das
>   dort am wenigsten vorsieht. Ohne dieses Prinzip wäre an der Stelle mehr
>   Konsens möglich."
> - Konfliktflächen · „Wie stark?" (früher „Dissens"): „Die Karte zeigt, wie
>   stark die gewählten Prinzipien vom Konsens abweichen: Je dunkler, desto
>   weiter liegt mindestens eines über dem Konsens."
> - Konfliktflächen · „Welche Prinzipien?" (früher „Wer denkt anders?"): „Die
>   Karte zeigt, welches Prinzip wo um mehr als 20 % vom Konsens abweicht. Dort
>   würde eine andere Auswahl mehr Konsens ermöglichen."
>   Zu diesem Reiter (Session 2026-10-12, dritte Runde): Die beiden Umschalter
>   heißen als Paar „Wie stark?" / „Welche Prinzipien?" — ältere Stellen in
>   diesem Dokument, die „Dissens" / „Wer denkt anders?" nennen, meinen dieselben
>   Unteransichten. Im Fließtext durchgängig „abweichen / Abweichung";
>   „Konfliktflächen" bleibt nur der Reitername. Headline je Unteransicht: „Wo
>   weichen die Prinzipien am stärksten vom Konsens ab?" bzw. „Welche Prinzipien
>   weichen wo vom Konsens ab?". Legende: „Größte Abweichung vom Konsens je
>   Landkreis" (Einheit wie bei den anderen Legenden). Offen: „Wie stark?" misst
>   in GW, „Welche Prinzipien?" mit der relativen 20-%-Schwelle.
>
>   **Rechte Spalte bei „Wie stark?" — „Größte Abweichungen" (Session
>   2026-10-12, rückbaubar).**
>   - *Vorher:* nur die Box „Zubaukonsens gesamt" (Zahl, Anteil am Ausbauziel,
>     Balken) und der Klick-Hinweis — nichts zur Abweichung selbst.
>   - *Jetzt:* Die Zubaukonsens-Box **bleibt unverändert oben**. Darunter neu der
>     Abschnitt „Größte Abweichungen": (1) ein Satz zur Konzentration, „Die
>     Hälfte der gesamten Abweichung liegt in [k] von [N] Landkreisen."; (2) die
>     fünf Landkreise mit der größten Abweichung, je mit Wert und dem Prinzip
>     (Farbfeld + Name), das dort am weitesten vom Konsens abweicht.
>   - *Berechnung:* Abweichung je Landkreis = derselbe Wert wie auf der Karte
>     (größter Abstand eines gewählten Prinzips zum Konsens, in GW bzw. MW).
>     „Gesamte Abweichung" = Summe dieser Werte über alle Landkreise; k = Anzahl
>     der größten Werte, die zusammen die Hälfte dieser Summe erreichen.
>   - *Interaktion:* Hover auf eine Zeile lässt nur diesen Landkreis farbig, alle
>     anderen werden hellgrau; Klick wählt den Landkreis aus (Zelldetail). In der
>     Region heißen die Einheiten „Rasterzelle"; Klick nur in der Raster-Ansicht.
>   - *Zurück zum vorherigen Stand:* in `EEquityMap.dc.html` die Konstante
>     `KONFLIKT_TOPLIST` auf `false` setzen — dann zeigt die rechte Spalte hier
>     wieder nur die Zubaukonsens-Box und den Klick-Hinweis; Satz, Liste und
>     Hover-Hervorhebung entfallen. Sonst ist nichts zurückzubauen.
>   - *Bekannte Schwäche:* Rangfolge in GW — große Landkreise führen die Liste
>     eher an (gleicher Größeneffekt wie auf der Karte).
> - Einzelverteilungen: **keine Caption** (Session 2026-10-12, zweite Runde) —
>   Headline und Kartentexte sagen es bereits. Weitere Vereinfachungen dieses
>   Reiters: (1) rechte Spalte ohne Konsenszahl; statt „Summe je Prinzip" mit
>   gleich langen Balken ein Satz „Jedes Prinzip verteilt das ganze Ausbauziel
>   ([X]), nur räumlich anders." (Balken nur, falls sich die Summen tatsächlich
>   unterscheiden); kein Hinweis „Klicken Sie auf eine Zelle …". (2) Kartentext
>   ohne wiederholten Prinzipnamen („Dorthin, wo viel Strom verbraucht wird.").
>   (3) Wertebereich mit Bezug: „0,00 – 0,59 GW je Landkreis" bzw. „… je
>   5 × 5-km-Zelle". (4) Karten fluchten je Zeile (Karte + Wertebereich am
>   unteren Kartenrand). (5) Kein Konsens-Hinweis unter den Karten. Offen: Jede
>   Karte hat ihre eigene Skala, Dunkelheit ist zwischen den Karten nicht
>   vergleichbar.
>   Die Kartengalerie hat keinen eigenen Scrollbereich (Session 2026-10-12):
>   Alle Karten stehen in voller Höhe, gescrollt wird nur die Hauptspalte.
> - Einzelansicht eines Prinzips: dessen Erklärsatz ohne Legenden-Nachsatz, z. B.
>   „Verbrauchsnah verteilt den Ausbau dorthin, wo viel Strom verbraucht wird."
> - Szenarien vergleichen: „Wählen Sie zwei Szenarien oder Referenzen als A und
>   B. Die Karte zeigt, wo A und wo B mehr Zubau vorsieht." (Der Tabellen-Satz
>   steht in der Tabellen-Box.)
>   Steht als einzige Caption **nicht** unter der Tab-Reihe, sondern im Hub
>   direkt über den A/B-Boxen.
>
> Die folgende Liste ist der frühere Stand (Legenden-Formulierungen) und gilt
> nicht mehr.

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
→ **Abgelöst durch O:** Der Hub „Szenarien vergleichen" (inkl. der zwei
Referenzen) ist in Basic und Erweitert sichtbar; „Mit einer anderen Verteilung
vergleichen" ist entfernt.

Regeln gegen Leaks: Bottom-Toolbar (Vergleichen / Download / Methodik) nur in
Erweitert; Bestand/Zubau/Gesamt nur bei aktivem Bestands-Toggle; Konflikt-Toggle
und die Tabs „Was begrenzt?"/„Übereinstimmungen" nur bei 2+ Prinzipien; keine
ausgegrauten Elemente in Basic.

---

## E. Rechte Spalte — Hybrid-Verhalten (neu)

> **Abgelöst durch O:** Zeilen „Konsens vs. Planung" und „Konsens vs. Referenz" sowie
> die Differenz-Zeile im Zelldetail entfallen mit diesen Ansichten.

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
der Anteil des Neuzubaus, den alle gewählten Prinzipien mittragen." (Seit Session
2026-10-12 hinter einem „i" neben dem Label „Zubaukonsens" im Zelldetail — zu,
bis angeklickt.) Der blasse
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
Prinzipien." Seit Session 2026-10-12 steht diese Caption hinter einem „i" neben
dem Label „Zubaukonsens gesamt" (zu, bis angeklickt; gleiches Muster wie die
Prinzip-Infos links) — ebenso die Regions-Variante „Vom Regionsbudget …". Der
vorhandene Prozentwert („78,7 % des Ausbauziels") bleibt.
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

> **Neu gefasst (Session 2026-10-12):** Die Liste (Reiter Konsens und „Was
> begrenzt?") zeigt je Prinzip den Betrag **ohne Vorzeichen**; die Erklärung
> steht einmal darüber: „So viel Zubaukonsens käme hinzu, wenn das jeweilige
> Prinzip nicht gewählt wäre." (bei zwei Prinzipien mit dem Zusatz „… für beide
> gleich viel"). Unterzeile in der Einheit der Karte: „begrenzt in 89
> Landkreisen" (Region: „Rasterzellen", Singular beachtet). Ein Prinzip, das
> nirgends begrenzt und nichts kostet, zeigt nur „begrenzt nirgends" — keine
> Zahl, kein Balken. Headline des Reiters: „In jedem Landkreis sehen Sie,
> welches Prinzip den Konsens begrenzt." (Region: „Die Karte zeigt, welches
> Prinzip den Konsens wo begrenzt."). Kartenlegende „Begrenzendes Prinzip" in
> der Reihenfolge der Liste, ohne Prinzipien, die nirgends begrenzen.
> Offen: Fläche auf der Karte ≠ Rang in der Liste (große Landkreise wirken
> dominanter, als ihr entgangener Konsens ist) — bisher nicht erklärt.

---

## F. Referenzen & „Konsens vs. Referenz" (neu / korrigiert) — nur Erweitert

> **Abgelöst durch O:** Referenzen sind jetzt „Historischer Ausbau" und
> „Planungsflächen" als Vergleichsobjekte im Hub. Begriffe (Positivflächen ≠
> Potentialflächen) und die zwei Deckungs-Kennzahlen gelten weiter.

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
(2) Prämisse „…die eine richtige Verteilung gibt es nicht"; (3) **schematische
Blasen-Illustration** (drei neutrale, gleichwertige Figuren, je eine Gedankenblase
mit Icon: Industrie/Verbrauch, Menschen/Bevölkerung, Münze/günstig) — macht die
Prämisse menschlich (verschiedene Menschen priorisieren Verschiedenes) —
**gefolgt von** dem **Bild aus drei echten, sichtbar unterschiedlichen
Prinzip-Karten** unter „Dieselbe Aufgabe, verschiedene Antworten.", beschriftet
über die FRAGE statt den Begriff („Wo der Strom verbraucht wird" / „Wo mehr
Menschen wohnen" / „Wo es am günstigsten ist"). Kausale Reihenfolge: Blasen (warum)
→ Karten (Wirkung); bei Platzmangel führen die Blasen, die Karten werden zum
Begleitstreifen; beide bleiben. Illustration = Dekoration, nicht Instruktion.
(4) erst danach der Begriff: „Jede solche Vorstellung ist ein
Gerechtigkeitsprinzip. EEquityMap … zeigt, wo sie sich einig sind."; (5) CTA
„Verteilungen ansehen →", Link „Mehr dazu" → „Was ist das?". Keine Stockfotos
(keine Windräder o. Ä.) — nur die schematische Illustration und die drei Karten.
Lässt nichts auswählen, erzwingt keinen Pfad. Jederzeit über „Was ist das?" wieder
aufrufbar. **Abgrenzung zum verworfenen Menü-Modal:** kein Pfad, keine Auswahl,
nur Prämisse + Beispiel.

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
  die sagt, was der Reiter zeigt (seit Session 2026-10-12 nicht mehr, was die
  Farbe bedeutet — s. C).
- Headline über der Karte ändert sich mit jedem Tab.
- „So-what"-Zeile in der Deutschland-Übersicht: „Je höher dieser Anteil, desto
  robuster lässt sich das Ziel über die Prinzipien hinweg erreichen."
- „Zubaukonsens" wird bei erstem Auftreten in situ definiert (Caption, nicht
  vorausgesetzt).
- Unterzeile der Prinzipienliste: „Kein Prinzip ist objektiv richtig …".
- Dauerhinweis „Konsens = robust, nicht automatisch genehmigungsfähig." bleibt
  immer sichtbar. Ort seit Session 2026-10-12: unter der Karte, direkt **unter**
  der Farblegende (zuvor unter der Tab-Reihe, wo er wie die Tab-Caption wirkte).
- **Reihenfolge unter der Karte** (alle Reiter, Session 2026-10-12): Farblegende
  → Konsens-Hinweis → weitere Auswertungen (Paar-Matrix bzw.
  Kombinations-Aufschlüsselung) → Buttons. Die Legende gehört zur Karte und
  steht deshalb immer direkt unter ihr.
- **Konfliktflächen · „Wer denkt anders?"** (Session 2026-10-12, aufgeräumt):
  Legende in einer Zeile („Farbe = die Verteilung, die hier mehr als 20 % vom
  Konsens abweicht. Gestreift = mehrere Verteilungen weichen hier ab.") + die
  Farbfelder, ohne „eingefärbt von:". Die Kombinations-Aufschlüsselung
  (Erweitert, ab 3 Prinzipien) ist ein eigener Abschnitt **nach** Legende und
  Hinweis, mit Titel „Welche Prinzipien weichen gemeinsam ab?" und Erklärzeile;
  die Punktzeilen sind links mit den Prinzipnamen beschriftet, die zweite
  Punkt-Legende entfällt. Der Konsens-Hinweis („Konsens heißt: …") wird in
  dieser Ansicht nicht gezeigt.
  Rechte Spalte: Titel „Zellen je abweichender Verteilung (> 20 %)"; die
  frühere Zusammenfassungsbox („… genau eine Verteilung weicht … ab · … mehrere ·
  … keine", s. Tabelle in E) entfällt. Je Kombination eine Reihe Farbfelder der
  beteiligten Prinzipien + Zellzahl, darunter die Namen — kein gestreiftes
  Kästchen mehr. Top 6, Rest hinter „Weitere anzeigen". **Hover** auf eine
  Zeile hebt auf der Karte die Zellen dieser Kombination hervor; alle anderen
  werden hellgrau, bis die Maus die Zeile verlässt. Bei „keine Abweichung"
  werden die betroffenen Zellen dunkler grau gezeigt, damit sie sich abheben.
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
- **Chip-Shelf für gespeicherte Szenarien** — gebaut, dann verworfen. (Grund:
  skaliert nicht — ein Chip ist Label UND Control zugleich; bei vielen Szenarien
  eine Wand, die die Karte verdrängt. Ersetzt durch: kleiner Zähler „(N) ·
  Vergleichen" + die Vergleichstabelle als einzige Verwaltung, jede Spalte
  wiederherstellbar/löschbar, horizontal scrollbar. Zwei UI-Flächen zu einer
  kollabiert.)
- **Tab pro gespeichertem Szenario** — verworfen. (Grund: zeigt nur eins zugleich
  — das Gegenteil von Vergleichen; und Tab-Reihe skaliert nicht. Ein
  Vergleichs-Einstieg, eine Tabelle mit Spalten.)
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
9. **Differenzkarte Szenario vs. Referenz mischt zwei Effekte** (Session
   2026-10-12, s. O „Referenz-Hinweis"). Die Referenz verteilt das ganze
   Ausbauziel, das Szenario nur den Zubaukonsens; jede Zelle zeigt A − B und
   damit zugleich „hier wird anders verteilt" und „die Referenz hat insgesamt
   mehr zu verteilen". Der Hinweis nennt die Lücke in GW, auf der Karte ist sie
   aber nicht ablesbar — beim Blick auf die Karte bleibt er unverständlich.
   Zwei Wege, nichts davon umgesetzt:
   - *Gleiche Mengen vergleichen:* Referenz auf die Summe des Szenarios
     skalieren. Die Karte zeigt dann nur die räumliche Abweichung, der Hinweis
     schrumpft auf eine Zeile („Zum Vergleich ist [Referenz] auf die [Y] des
     Szenarios skaliert."). Ändert die Berechnung (bisher „Logik unverändert");
     „Differenz gesamt" in der rechten Spalte wird 0.
   - *Berechnung lassen, Hinweis an das Kartenbild binden:* z. B. „Deshalb ist
     die Karte überwiegend braun (mehr in B)." Erklärt die sichtbare Schlagseite,
     die Karte bleibt eine Mischung.

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

**Meldungs-Format (Design-System-Regel):** Nicht alles, was kommuniziert werden
muss, braucht ein dauerhaftes vollbreites Band über der Karte — sonst stapeln sich
die Bänder und drücken die Karte nach unten. Drei Werkzeuge, drei Fälle:
- **Dauerhafter Zustand** (z. B. „Ergebnis veraltet") → linke Spalte / inline, EIN
  Ort, nicht dupliziert.
- **Momentane Bestätigung** (z. B. „Gespeichert …") → **Toast** (erscheint, fadet
  nach einigen Sekunden), kein dauerhaftes Band.
- **Einmalige Lehre** (z. B. Mode-Transition) →
  schließbares Band, sparsam, verschwindet nach erstem Lesen und kommt nicht
  zurück.
Zwei Meldungen, die dasselbe sagen, werden zusammengeführt (nie das gleiche
„Eingaben geändert" als Band UND als Panel-Box).

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

**Einheit nach Scope (geändert — „nur GW"-Regel wird skaliert):** Die frühere
Stakeholder-Regel „durchgehend GW" gilt weiter **im nationalen Scope** (160 GW,
114 GW Konsens — GW-große Größen; MW wäre absurd). **Im regionalen Scope: MW**
(1 GW = 1000 MW), fest nach Modus (nicht adaptiv pro Wert). Grund: eine 5-×-5-km-
Zelle hält ≈ 0,00–0,01 GW — in GW rundet alles auf „0,00", die Region wirkt tot;
in MW („6 MW", Legende „0 — 12 MW") sind die Werte lesbar und variieren sichtbar.
MW ist für Standort-/Zell-Leistung ohnehin die natürlichere Einheit. Der
Einheitenwechsel begleitet den Moduswechsel und verstärkt die Ebenen-Trennung.
**Rundung:** Die GW-Zwei-Nachkommastellen-Regel ist eine GW-Regel; MW auf dieser
Skala als ganze Zahl / eine Nachkommastelle, kein erzwungenes „,00".

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
- **Klick-Hinweis der rechten Spalte** nennt die klickbare Einheit (Session
  2026-10-12): „Klicken Sie auf einen Landkreis / eine Gemeinde / eine
  Rasterzelle, um die Zahlen für diesen Ort zu sehen." — nicht mehr pauschal
  „eine Zelle".
- **Scope-Label nur in der Kartenleiste** (Session 2026-10-12): In der rechten
  Spalte entfallen die Dachzeilen „Deutschland · Landkreise" (Landkreis gewählt),
  „[Landkreis] · Gemeinden" (Gemeinde gewählt) und „Region" (Regionsübersicht) —
  sie wiederholten nur die Kartenleiste. Ebenso entfällt im Hub die Dachzeile
  „Szenarien vergleichen" über „A und B im Überblick"; dort steht stattdessen
  auf Deutschland-Ebene die Dachzeile „Deutschland" (bei einem
  Regionsvergleich keine). Es bleiben „Übersicht" (Deutschland
  gesamt) und „[Landkreis] · 5 × 5 km Raster" (Rasterzelle gewählt).

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
(Landkreis X → Zelle Y). **Alle Regions-Headlines/Captions nennen den
Regionsnamen** (aus dem aktiven Scope), nie „Deutschland" oder das Bundesziel; und
Werte in **MW** statt GW (s. L).

**Mode-Transition (als Ladezustand-Label):** Der Wechsel DE → Region wird nicht
mehr über ein dauerhaftes Band angekündigt (spart vertikalen Platz), sondern als
**Label des Ladezustands** gezeigt: während der Regionsberechnung „Landkreis X
wird ausgewertet — die nationale Verteilung bleibt oben als Kontext." Damit an den
Lade-Moment gebunden (Auto-Berechnung beim Betreten, „Neu berechnen"), nicht als
garantierte Einmal-Ankündigung — ok, weil die Scope-Leiste den Landkreis dauerhaft
nennt. Zusätzlich weiter sichtbar machen:** an der Liste „Verteilung innerhalb der Region": „Startet mit
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
- **Farb-Rhyme + Hierarchie:** BEIDE Sätze tragen die Prinzip-Farben (nationaler
  Kontext als farbige Chips, regionale Liste wie gehabt), damit das Auge
  „dieselben Prinzipien, zwei Ebenen" ohne Lesen verbindet. Der nationale
  Kontext-Kasten ist zurückgenommen (leiser/recessed = Kontext), die regionale
  Liste prominenter (= aktives Steuerelement). Ziel: unterscheidbar (Hierarchie)
  UND zusammengehörig (Farbe). „Einfach" ist nicht das Ziel — zwei Ebenen sind
  inhärent komplexer als eine Liste; das ist der Preis des Features.

**Zurück von der Zelle zur Übersicht:** primär das **× im Zelldetail-Panel**
(rechte Spalte) + **Klick auf den Kartenhintergrund** (vergebendes Muster).
Erneutes Anklicken der Zelle bleibt Bonus, ist aber nicht der primäre (weil
unauffindbare) Weg. „← Deutschland" verlässt die Region ganz (unverändert).

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
  wird — sonst wirkt es kaputt. Während der Rechnung liegt ein **abdimmender
  Scrim** über Karte + Bedienelementen, **nur „Abbrechen" klickbar** (verhindert
  Eingaben mitten in der Rechnung; der Scrim ist selbst „es arbeitet"-Signal).
  **Fortschritt als Trickle-Balken:** bewegt sich links→rechts, verlangsamt sich
  (asymptotisch), erreicht das Ende nicht von selbst, springt erst bei
  Fertigstellung auf 100 % — zeigt Vorwärtsbewegung („hängt nicht") OHNE %/Dauer zu
  behaupten. Muss in den ersten 1–2 s sichtbar kriechen (sonst liest man ihn als
  hängend). KEINE Prozentzahl/Countdown, kein konstanter Sweep. Status-Text zeigt,
  dass Zeit vergeht (Backend-Stufen wechselnd, sonst nach kurzer Zeit „Das kann
  einen Moment dauern…"). Text „[Landkreis] wird ausgewertet …", plus **„Abbrechen"**. Im Mock die Verzögerung nur als Delay nachbilden, ohne
  bekannte Dauer zu behaupten.
- **Banner in der Regionsansicht:** nur der einmalige Mode-Transition-Banner. (Der
  frühere DE-Ansicht-Hinweis „Zwei Prinzipien gewählt …" ist seit Session
  2026-10-12 ganz entfernt.) „Noch nicht
  berechnet" nur an einer Stelle (erscheint durch die Auto-Berechnung ohnehin nur
  kurz als „wird berechnet …").

**Karten-Abstraktion (geändert — Reversal mit Einschränkung):** Für das Testing
werden **echte Landkreis-Formen** eingeführt (aus vg250_krs.gpkg → **TopoJSON**,
maßvoll vereinfacht ~150–300 KB → D3-geo/SVG), weil die Hypothese ist, dass die Hexagon-Abstraktion
Verständnis und Vertrauen beeinflusst (der eigene Landkreis sichtbar). **Wichtige
Einschränkung: weiterhin KEINE Live-Karte** (kein Leaflet, keine Tiles, kein
Pan/Zoom, kein Backend) — statisches, vorab vereinfachtes GeoJSON als SVG. Es
werden nur erfundene Formen durch echte ersetzt; der „schematisch, eigenständig,
kein Backend"-Charakter bleibt. Sequenzierung: **Deutschland-Ebene (Landkreise)
zuerst** (billig, hohe Wirkung), **Region-Ebene (5-×-5-Raster im Landkreis, an die
Grenze geclippt, grob) als Fast-Follow** — bis dahin bleiben die schematischen
Rechtecke Platzhalter. Region-Zellen bleiben Rechtecke (Kontrast zu den
Landkreis-Formen). Details s. Build-Task „Real Map".

**Granularität aufgelöst:** 5 × 5 km ist NICHT die Auflösung des nationalen
Kartenbilds — national ist der Landkreis die Einheit; 5 × 5 km existiert nur
INNERHALB einer Region. Damit ist der Modus die Granularität: Der frühere
Granularitäts-Schalter („5 × 5 km / Gemeinde / Landkreis") ist durch die
DE-/Region-Aufteilung weitgehend abgelöst und sollte darin aufgehen, nicht als
separater Regler bestehen bleiben.

**Gemeinde-Ebene (nur Erweitert, zum Testen — erweitert das Modell):** In der
Regionsansicht ein Ansichts-Umschalter UNTER der Karte, nur im Erweitert-Modus,
mit drei Optionen: (1) **Raster (5 × 5 km)** — Zellen klickbar (Bestand);
(2) **Gemeinden** — echte Gemeinde-Formen (aus vg250_gem_simpl250m.gpkg →
TopoJSON, nur die Gemeinden des aktuellen Landkreises), Gemeinden klickbar;
(3) **Gemeinden + Raster** — Gemeinde-Grenzen als gedämpfte Kontextlinien über dem
Raster, Rasterzellen bleiben das Klickbare. **Das klickbare Element wechselt mit
der Ansicht** — muss eindeutig signalisiert sein (Hover/Cursor), sonst klickt man
die falsche Einheit.
- **Gemeinde-Klick = aggregierte Zell-Ergebnisse.** Gerechnet wird weiter auf dem
  Raster; Gemeinden decken sich nicht mit Zellen. Rahmung, damit das Modell ehrlich
  bleibt: **Grid = Berechnung, Gemeinde = administrative Ablesung derselben
  Ergebnisse.** Gemeinde-Detail rechts analog zum Zell-Detail, aggregiert, in MW.
- Konzeptioneller Hinweis: Das ergänzt zur „zwei Ebenen, zwei Geometrien"-Story
  eine dritte Ablese-Ebene (Gemeinde). Bewusst als Testfeature (3 Optionen), um zu
  lernen, ob es das Verständnis verbessert oder die Karte übersichtlich hält —
  besonders „Gemeinden + Raster" (zwei Grenzsysteme übereinander) ist ein
  offener Punkt fürs Testing.

> **Position geändert (Session 2026-10-12):** Der Ansichts-Umschalter steht nicht
> mehr unter der Karte, sondern **in der Karte, Ecke oben rechts** (kompakte
> weiße Leiste mit den drei Optionen, ohne das Label „Ansicht"). Schrift im
> Umschalter 13 px — **bewusste Ausnahme** von der 14-px-Untergrenze in K, auf
> ausdrücklichen Wunsch, damit die Leiste in der Kartenecke klein bleibt. Die Kartenfläche bekommt dafür oben einen
> Streifen Platz, damit der Umschalter die Karte nicht verdeckt. Die zugehörigen
> Hinweise („Gemeinden anklicken für Details …", Lade-/Fehlermeldung der
> Gemeindegrenzen) stehen direkt unter der Karte, über der Farblegende. In der
> Galerie „Einzelverteilungen" erscheint der Umschalter nicht mehr (er hatte
> dort keine Wirkung).
>
> **Regionsansicht vereinfacht (Session 2026-10-12):**
> - **Standardansicht in Erweitert = „Gemeinden"** (wie Basic), nicht mehr
>   „Gemeinden + Raster": Dort lagen Raster, Gemeindegrenzen und Kreisumriss
>   übereinander, und das abgedunkelte Raster — das eigentliche Ergebnis — war
>   kaum lesbar. Reihenfolge im Umschalter: Gemeinden · Gemeinden + Raster ·
>   Raster (5 × 5 km).
> - **Headline erklärt das Regionsbudget** statt die rechte Spalte zu
>   wiederholen: „Regionsbudget für [Landkreis]: [X] MW — sein Anteil am
>   bundesweiten Konsens." (bei nur einem bundesweiten Prinzip: „… an der
>   bundesweiten Verteilung."). Vorher: „Von [X] MW Regionsbudget liegen [Y] MW
>   im Konsens der regionalen Prinzipien." — diese Aussage steht weiter in der
>   rechten Spalte (Zahl, Anteil, Balken, „i").
> - **Linke Spalte:** Die Unterzeile „Kein Prinzip ist objektiv richtig …"
>   entfällt in der Regionsansicht (bleibt auf Bundesebene); dort steht bereits
>   der Zwei-Ebenen-Hinweis. Der Kontext-Kasten der bundesweiten Prinzipien ist
>   zugeklappt nur **eine Zeile**: „Bundesweit:" + Farbpunkte der Prinzipien +
>   Anzahl („3 Prinzipien") + Pfeil. Die Namen stehen erst aufgeklappt als Chips
>   darunter — vorher standen sie zugeklappt und aufgeklappt doppelt. Der
>   Hinweis „Startet mit denselben Prinzipien wie national. …" bleibt
>   unverändert sichtbar (wichtig für das Zwei-Ebenen-Verständnis).
> - **Zähleinheit = klickbare Einheit:** In „Was begrenzt den Konsens?" zählt
>   die Unterzeile in der Region **Gemeinden** („begrenzt in 64 Gemeinden"),
>   solange Gemeinden gezeigt werden (Basic; Erweitert in „Gemeinden" und
>   „Gemeinden + Raster"). Von **Rasterzellen** ist nur in der Ansicht „Raster
>   (5 × 5 km)" die Rede. Begrenzendes Prinzip einer Gemeinde = das Prinzip mit
>   dem kleinsten flächengewichteten Wert in der Gemeinde — dieselbe Regel wie
>   im Gemeinde-Detail. Die GW/MW-Beträge der Liste sind davon unberührt.
>   **Noch zellbasiert** (offen): „Größte Abweichungen" in der Region
>   („Rasterzelle …"), „Zellen je abweichender Verteilung" samt
>   Kombinations-Diagramm, sowie die Legende „Begrenzendes Prinzip".

---

## N. Szenarien speichern & vergleichen (neu)

> **Erweitert durch O:** „Szenarien vergleichen" ist jetzt der einzige
> Vergleichs-Hub und immer sichtbar (nicht erst ab ≥ 2 Szenarien).

**Konzept:** Ein Szenario = Momentaufnahme der aktuellen Konfiguration (Scope,
Prinzipien, Technologie, Ausbauziel/-jahr) + berechnete Kernergebnisse. Verglichen
werden **Zahlen**, nicht Karten nebeneinander (zwei Karten nebeneinander sind
unlesbar — dieselbe Lehre wie bei der Doppel-Intensität, s. H).

**Session-only:** kein Login, keine Persistenz; Snapshots leben im Speicher,
Neuladen verwirft sie. Klar kommuniziert („nur für diese Sitzung").

**Drei Teile:**
- **„Zum Vergleich speichern"** (nicht bloß „speichern" — Zweck statt Mechanik)
  in der **Tab-Reihe, rechtsbündig** (nahe Karte/Ergebnis), als Aktion (Icon +
  Label), editierbarer Auto-Name. Beim ersten Speichern einmalige Zweck-
  Bestätigung („Gespeichert. Speichern Sie weitere Szenarien und vergleichen Sie
  sie.").
- **Kein Chip-Shelf** (skaliert nicht — bei 15 Szenarien eine Wand). Stattdessen
  nur ein kleiner Indikator „Gespeicherte Szenarien (N) · Vergleichen".
- **Die Vergleichstabelle ist die einzige Verwaltung:** jedes Szenario = eine
  Spalte; im Spaltenkopf kurzer Name + „Wiederherstellen" (lädt Config + Karte) +
  × (löschen). Ansehen/wiederherstellen/löschen passiert dort. Kein
  In-place-Editieren (wiederherstellen → ändern → neu speichern). „Vergleichen"
  ab ≥ 2 Szenarien. EIN „Vergleichen"-Einstieg (KEIN Tab pro Szenario).

**Kennzahlen (Zeilen), Default-Set:** Scope · Prinzipien · Ausbauziel ·
Zubaukonsens · % im Konsens · begrenzendes Prinzip.

**Unbegrenzt viele Szenarien, horizontal scrollbar.** Damit das funktioniert:
- **Erste Spalte (Kennzahl-Labels) sticky** — bleibt sichtbar beim Scrollen (sonst
  Zahlen ohne Zeilenbezug).
- Je Spalte Kopf mit Name + × (die Tabelle verwaltet auch die Ablage).
- **DE- (GW) und Region-Szenarien (MW) klar unterscheidbar** (Gruppierung/Tag), da
  160 GW und 350 MW nicht direkt vergleichbar sind; jede Zelle mit eigener Einheit
  (s. L).

**Bewusst nicht:** keine Karten nebeneinander, kein Tab pro Szenario, kein
In-place-Editieren, keine Persistenz. Parallelität (Zeilen fest, Spalten variieren)
ist derselbe Trick wie bei „Konsens vs. …" — nur der Unterschied variiert.

---

## O. Vergleichs-Hub „Szenarien vergleichen" (Reversal, Session 2026-10-09)

**Reversal (bewusst):** Der Reiter „Vergleich" (Konsens vs. Referenz / Konsens vs.
Planung) ist entfernt. Referenzen sind auswählbare Vergleichsobjekte im Reiter
„Szenarien vergleichen", der damit der **einzige Vergleichs-Hub** ist.

**Reiter:** Konsens | Was begrenzt? | Konfliktflächen | Einzelverteilungen |
Szenarien vergleichen. „Szenarien vergleichen" ist **immer sichtbar** (auch bei
0/1 Szenarien, Basic und Erweitert). Entfernt: „Mit einer anderen Verteilung
vergleichen", Switch „Konfliktflächen ausblenden". Weitere Prinzipien nur links
wählbar. „Zum Vergleich speichern" ist auch im Hub sichtbar (Session
2026-10-12; zuvor dort ausgeblendet).

**Hub-Aufbau:**
- **Kopf:** Wind/PV-Umschalter — eigener Filter des Hubs (startet mit der
  Technologie links, ändert die Berechnung nicht). Hub-Rahmen/-Hintergrund blau
  (Wind) bzw. gelb (PV). Nur Szenarien der gewählten Technologie.
- **Vergleichsobjekte** (im Dropdown der Slots, s. u.), gruppiert: Referenzen ·
  Deutschland · je Landkreis.
- **Referenzen (immer vorhanden):** „Historischer Ausbau" und „Planungsflächen"
  (Zubau nach ausgewiesenem Flächenanteil je Zelle), je auf dasselbe Ausbauziel
  skaliert. Visuell abgesetzt (gestrichelter Rahmen, eigener Grundton), je ein
  (i) mit Kurzinfo.
- **A = Ausgangspunkt, B = Vergleichsobjekt** — je ein Szenario oder eine
  Referenz. Die Slots sind selbst Auswahl-Picker (s. Slot-Picker unten); × im Slot
  leert ihn (ohne zu löschen). Default: zwei Szenarien; bei einem
  Szenario dieses vs. Planungsflächen; ohne Szenario die zwei Referenzen.
- **Differenzkarte A − B** nur auf gemeinsamer Ebene: zwei Szenarien derselben
  Ebene; eine Referenz übernimmt Ebene + Ziel des Szenarios (Region → 5 × 5 km,
  MW); zwei Referenzen = Deutschland.
- **Zelle auf der Differenzkarte anklicken** (Session 2026-10-12): Landkreis
  (Deutschland) bzw. Rasterzelle (Region) ist klickbar. Die Auswahl ist auf der
  Karte als dunkle Kontur sichtbar; rechts erscheint **anstelle** von „A und B
  im Überblick" (nie beides zugleich, wie Übersicht/Zelldetail in den anderen
  Reitern) dieselbe Gliederung wie im Überblick, nur für diese Zelle: Dachzeile
  „Auf der Karte ausgewählt" + Name als Titel, Box „Differenz A − B" mit
  „mehr in A" / „mehr in B", darunter je eine Karte für A und B mit einem Wert
  (Szenario: „Zubaukonsens", Referenz: „Verteilt").  Erneuter Klick oder × hebt die Auswahl auf; sie
  bleibt beim Wechsel von A/B auf derselben Ebene stehen. Bewusst **kein**
  Popover „… im Detail auswerten →" wie in den anderen Reitern: Der Wechsel in
  die Regionsauswertung würde die linke Konfiguration ändern, nicht A und B.
  Gemeinden sind hier nicht wählbar (die Karte zeigt das Raster).
  Die Karte trägt keine Unterzeile mehr („Deutschland · je Landkreis · …
  anklicken …" entfernt); Klickbarkeit zeigt nur der Zeiger-Cursor.
- **Differenzen ohne Vorzeichen** (Session 2026-10-12): Im Hub steht keine
  Differenz mit „+" oder „−". Die Richtung ist ausgeschrieben: Betrag + „mehr in
  A" / „mehr in B" / „gleich viel in A und B" — in der Karte zur gewählten
  Zelle und bei „Differenz A − B gesamt". Die Legende der Differenzkarte nennt
  an beiden Enden „bis [Maximum]" statt „+[Maximum]".
- **Konsens vs. Planung:** Steht ein Szenario gegen „Planungsflächen", erscheinen
  unter der Differenzkarte die zwei Kennzahlen „Von der Ausweisung trifft X % den
  Konsens" / „Vom Konsens sind erst Y % ausgewiesen" (Logik wie bisher, aus
  gespeichertem Zubaukonsens + Konfliktflächen des Szenarios). Die frühere
  Deckungskarte entfällt.
- **Konsens vs. Historischer Ausbau** (Session 2026-10-12): Steht ein Szenario
  gegen „Historischer Ausbau", erscheint an derselben Stelle eine Kennzahl: „Vom
  historischen Ausbau liegen X % im Konsens". „Im Konsens" wie bei
  Planungsflächen (konfliktfreie Zelle über 35 % des Zellmaximums); X = Anteil
  der Referenzverteilung in Konsens-Zellen. Keine zweite Richtung („Vom Konsens
  … gedeckt"): Die Referenz ist auf das Ausbauziel skaliert, nicht der reale
  Bestand — die Aussage wäre nur hypothetisch (verworfen).
- **Tabellen je Ebene** (Deutschland · GW, je Landkreis · MW); Spaltenkopf mit
  Wiederherstellen + × (löschen). Region-Szenarien: zwei Zeilen „Prinzipien DE" +
  „Prinzipien regional"; DE-Szenarien: eine Zeile „Prinzipien".

**Greenfield-Hinweis** zu den Jahres-Marken (2030/2035/2045) — seit Session
2026-10-12 hinter einem „i" neben dem Label „Ausbauziel" in der linken Spalte
(zu, bis angeklickt), nicht mehr dauerhaft unter dem Regler: „Die Marken 2030,
2035 und 2045 sind drei getrennte Berechnungen, jede für sich von Grund auf neu
verteilt. Sie zeigen keinen Ausbaupfad: Das Ergebnis für 2035 baut nicht auf dem
für 2030 auf."

Entfernt (Session 2026-10-12): der gelbe Hinweis „Bei PV unterscheiden sich die
Gerechtigkeitsprinzipien nur geringfügig." unter dem Technologie-Schalter.

**Vereinfachung (Session 2026-10-09, nachgezogen):**
- **Held = A/B-Slots + Differenzkarte** (volle Breite des Hubs).
- ~~Objektliste mit Klick-Regel, „Nächster Klick"-Hinweis, „ersetzt: A/B"-Leiste~~
  → abgelöst durch den **Slot-Picker** (s. u.).
- **Rechte Spalte im Hub:** A/B-Zusammenfassung (Kernzahlen je Seite + Differenz
  gesamt), nie die Links-Konfiguration. Ohne Auswahl: „Noch nicht gewählt".
- **Konsens-Disclaimer** im Hub ausgeblendet; seit Session 2026-10-12 auch in
  Konfliktflächen · „Wer denkt anders?" und in Einzelverteilungen (dort wird
  kein Konsens gezeigt); übrige Reiter unverändert.
- **Löschen:** kein Dialog; Undo-Toast „Szenario gelöscht · Rückgängig" (≈ 6 s),
  stellt Szenario an seiner Position inkl. A/B-Zuordnung wieder her.
- **Wind/PV:** Der Hub-Umschalter und der Umschalter links sind gekoppelt
  (Session 2026-10-12): Beide setzen dieselbe Technologie, der Hub kann nicht
  von links abweichen. Umschalten im Hub rechnet also auch die linke
  Konfiguration für die andere Technologie; Prinzipien, Ausbauziel,
  Bestandsanrechnung und Region bleiben erhalten. Entfallen: Kennzeichnung
  „Abweichend von links (…)" und „Wie links".
- **Technologie überall erkennbar** (Session 2026-10-12): Die Farben der Hub-Box
  (blau = Wind, gelb = Freiflächen-PV) gelten in kleiner Dosis auf allen
  Reitern: ein Technologie-Chip rechts in der Tab-Reihe (Farbpunkt + Name, also
  nicht nur über Farbe) und die aktive Hälfte beider Wind/PV-Schalter in der
  Technologiefarbe statt neutral weiß. Keine getönte Hauptfläche außerhalb des
  Hubs — die Karten-Farbskalen sollen unverfälscht bleiben.
- **Referenz-Hinweis:** Steht ein Szenario gegen eine Referenz, mit den
  tatsächlichen Summen der beiden Seiten: „Die beiden Seiten verteilen
  unterschiedlich viel: [Referenz] [X] (das ganze Ausbauziel), das Szenario [Y]
  (nur den Zubaukonsens). Die [X − Y] Unterschied stecken mit in der Karte. Sie
  zeigt also nicht nur, wo anders verteilt wird, sondern auch, dass [Referenz]
  mehr verteilt." Entfällt, wenn beide Summen gleich angezeigt würden (Konsens =
  Ziel). (Logik unverändert.)
- **Einstieg:** Haupteinstieg ist der Reiter; die Zeile „Gespeicherte Szenarien (N)
  · Vergleichen →" ist nur Shortcut und im Hub ausgeblendet. Leere GW-/MW-Tabellen
  werden nicht gerendert.

## P. Paarweise Prinzipien-Matrix & „Vergleich teilen" (Session 2026-10-09)

**Paarweise Matrix** (Reiter „Konsens", direkt unter Karte + Legende; nur
Erweitert; DE- und Regionsmodus; erst ab 2 Prinzipien):
- Zeilen/Spalten = aktuell gewählte Prinzipien, Dreieck unterhalb der Diagonale,
  jedes Paar genau einmal. Die stets leere erste Zeile und letzte Spalte entfallen
  (n gewählt → (n−1) × (n−1): Zeilen = Prinzip 2…n, Spalten = Prinzip 1…n−1).
- Spaltenköpfe 45° gedreht, mit Farbpunkt; Kopfhöhe nach dem längsten Namen,
  damit nichts überlappt oder abgeschnitten wird. Feste Spaltenbreiten (kompakt).
  Seit Session 2026-10-12: lange Namen brechen im Kopf auf zwei Zeilen um (max.
  150 px), damit ein einzelner langer Name nicht viel Leerraum über der Tabelle
  erzeugt; Zeilenkopf 180 px, Wertspalten 88 px (vorher 200/112), sodass die
  Matrix auch mit sechs Prinzipien ohne Abschneiden in die Spalte passt; etwas
  mehr Abstand zu den Buttons darunter.
- Zellwert = Zubaukonsens des Paars: Minimum-Regel über genau diese zwei
  Prinzipien, Summe über alle Zellen — GW (DE) bzw. MW (Region).
- Eckzelle: „Zubaukonsens in GW (Ziel: …)" bzw. „Zubaukonsens in MW
  (Regionsbudget: …)".
- Farbe: Konsens-Blauskala der Konsenskarte, über den Wertebereich der Matrix
  gespannt (dunkel = hoch); keine neue Farbsprache.

**„Vergleich teilen"** (Hub „Szenarien vergleichen", an der Stelle von „Zum
Vergleich speichern"): Popup mit kopierbarem Link + „Link kopieren" und kurzem
Hinweis, welchen Vergleich der Link teilt (A, B, Technologie). Prototyp:
Platzhalter-Link aus A/B + Technologie, keine Persistenz.

**Slot-Picker (Session 2026-10-09, ersetzt Klick-Regel):**
- A und B sind selbst Dropdowns: Klick auf den Slot öffnet die Liste aller
  wählbaren Objekte, gruppiert „Referenzen (immer vorhanden)" · „Deutschland" · je
  „Landkreis …"; Referenzen mit gestricheltem Kennzeichen + (i). Auswahl füllt
  genau diesen Slot. **Wechseln = Slot erneut anklicken**, kein ×-zuerst.
- Ein Objekt im anderen Slot ist deaktiviert und markiert („bereits in A/B"), das
  aktuelle als „gewählt" — A und B können nicht identisch werden.
- × leert den Slot („— bitte wählen"); Klick außerhalb schließt das Dropdown.
- Wind/PV-Filter gilt für den Dropdown-Inhalt; ohne Szenarien der Technologie
  zeigt das Dropdown einen Hinweis statt einer leeren Gruppe.
- Die Tabellen je Ebene sind die Gesamtübersicht aller gespeicherten Szenarien
  (ersetzt die weggefallene Objektliste).

**Button-Positionen (Session 2026-10-09):**
- „Zum Vergleich speichern" steht in der linken Spalte direkt unter den
  Prinzipien (volle Breite, umrandet wie „Vergleich teilen"), nicht mehr in der
  Tab-Reihe. Sichtbar nur, wenn es etwas zu speichern gibt UND die aktuelle
  Konfiguration noch nicht gespeichert ist (gleiche Ebene, Prinzipien DE +
  regional, Technologie, Ausbauziel, Bestandsanrechnung). Gilt seit Session
  2026-10-12 auch im Hub (zuvor dort ausgeblendet).
- „Vergleich teilen" steht rechtsbündig unter der Tab-Linie, direkt über der
  Wind-/PV-Hub-Box. Die Tab-Reihe enthält nur noch die Reiter.
- **Hub-Aufbau (Session 2026-10-12):** In der Zeile über der Hub-Box steht der
  Wind-/PV-Umschalter mittig (er gilt für Box und Tabellen), „Vergleich teilen"
  rechts. Die farbige Box „Vergleich A − B" enthält Erklärzeile, A/B-Boxen und
  Differenzkarte. Die Tabellen je Ebene stehen **unter** der Box in einer
  eigenen Box gleicher Technologiefarbe („Gespeicherte Szenarien", mit der
  Erklärzeile „Die Tabellen stellen die gespeicherten Szenarien je Ebene
  gegenüber."); ohne gespeicherte Szenarien entfällt diese Box.
- Die Zeile „Gespeicherte Szenarien (N) · Vergleichen → · Nur für diese Sitzung."
  entfällt; Einstieg ist allein der Reiter. Der Session-only-Hinweis steht weiter
  im Speichern-Dialog.

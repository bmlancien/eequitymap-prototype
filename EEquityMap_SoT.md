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

> **Geändert durch R (Session 2026-10-19):** „Neuzubau" heißt jetzt „Zubau", „Ausbau gesamt" heißt „Ausbau"; Zubau = max(Ausbau − Bestand, 0). Die Teilmengen-Regel gilt weiter (R.1).

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

### Wortwahl-Regeln nach dem Text-Audit (Session 2026-10-12)

- **Einheit beim Namen nennen, nie pauschal „Zelle".** Deutschland:
  „Landkreis(e)". Region: „Gemeinde(n)", solange Gemeinden gezeigt werden;
  „Rasterzelle(n)" nur, wo tatsächlich das 5 × 5-km-Raster gemeint ist oder
  gezählt wird. Angepasst: Titel „Landkreise je abweichendem Prinzip (> 20 %)"
  (Region: „Rasterzellen …"), Text und Tooltips des Kombinations-Diagramms,
  Info „Wie viel dieser Landkreis zum Ausbauziel beiträgt …", Unterzeile „Summe
  je Prinzip", Referenz-Info „Planungsflächen", Prinzip-Erklärsätze.
- **„Prinzip", nicht „Verteilung",** wenn ein Gerechtigkeitsprinzip gemeint
  ist (Legende und Liste unter „Welche Prinzipien?"). „Verteilung" bleibt für
  das Ergebnis (wie sich der Ausbau verteilt).
- **„bundesweit", nicht „national",** in allen sichtbaren Texten.
- **Noch offen** (brauchen eine Entscheidung, keine reine Wortwahl): in der
  Region zählen „Rasterzellen je abweichendem Prinzip", das Kombinations-
  Diagramm und „Größte Abweichungen" weiter Rasterzellen, auch wenn Gemeinden
  gezeigt werden; „Region" steht teils für „Landkreis";
  „Basic" neben „Erweitert". („SZENARIO" in der linken Spalte ist erledigt:
  heißt jetzt „RAHMEN", s. O „Button-Positionen".)
- **Erledigt nach dem Audit:** MW-Werte haben immer genau **eine
  Nachkommastelle** („1,0 MW"; GW weiter zwei). Die Zeile „Priorität /
  Reihenfolge" samt Hinweis „Die Zubaureihenfolge wird noch nicht berechnet."
  ist aus dem Detail entfernt, bis es eine berechnete Reihenfolge gibt (zuvor
  ein Jahr mit dem Zusatz „(nicht berechnet)").

---

## C. Ansichten / Tabs — finaler Stand

> **Geändert durch R (Session 2026-10-19):** Der Reiter „Konfliktflächen" heißt „Dissens" (R.10). Die Reiter sind jetzt Unter-Reiter der Bereichs-Tabs „Deutschland" und „Regional"; neu ist der Bereich „Bestand & Potenziale" (R.3, R.13).

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
>   **Vergrößerte Karte durchblättern** (Session 2026-10-12): Im Fenster hinter
>   „Einzeln ansehen" stehen links und rechts der Karte Pfeile; sie (und die
>   Pfeiltasten ←/→) wechseln zum vorherigen/nächsten gewählten Prinzip, ohne
>   das Fenster zu schließen — Karte an derselben Stelle, in derselben Größe.
>   Unter der Karte eine Reihe mit den gewählten Prinzipien (Farbfeld + Name),
>   das offene hervorgehoben, anklickbar zum direkten Wechsel. Esc schließt.
>   Reihenfolge wie in der Galerie; am Ende geht es vorn weiter. **Nicht
>   umgesetzt** (bewusst): ein Knopf „Region nur nach diesem Prinzip verteilen".
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

> **Geändert durch R (Session 2026-10-19):** „Neuzubau" → „Zubau", „Ausbau gesamt" → „Ausbau" (R.1). Im Tab „Regional" zeigt die rechte Spalte oben die Herleitungskette (R.6).

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

> **Geändert durch R (Session 2026-10-19):** Neue Infobox-Texte für Einstieg, Bestand und Begriffe (R.12); einmaliger Kartenhinweis zur Detailauswertung (R.4); Speicher-Hinweise (R.9). Das Werkzeug startet im Bereich „Bestand & Potenziale"; der Knopf des Prämissen-Overlays heißt „Los geht's →" (R.3, Nachtrag).

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
- **„Kein Pan/Zoom" (M) für den Reiter „Bestand & Potenziale"** — aufgehoben
  (R.13). Die Gerechtigkeitskarten bleiben statisch.
- **Knopf „← Deutschland"** — ersetzt durch die Bereichs-Tabs (R.3).
- **Regionsbudget = Anteil am bundesweiten Konsens** — verworfen. Das
  Regionsbudget ist die bundesweite Verteilung (R.5).
- **A/B ohne gespeicherte Szenarien mit zwei Referenzen vorbelegt** — verworfen.
  A/B bleiben leer, die Referenzen sind wählbar (R.8).

---

## I. Offene Fragen — für den Open-Questions-Log

1. *(Erledigt in Session 2026-10-12 — siehe J2 „Farbsystem"; Konsens-Skala
   ist neutral grau.)* **Farbsystem: Grün/Teal vs. Steel Blue.** Der Prototyp nutzt durchgängig
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
10. **Bestandsberücksichtigung standardmäßig an oder aus?** (R.1) Aktuell aus
    (Greenfield). Mit Öko-Institut / EmPowerPlan klären.
11. **Zoomschwelle Landkreis → Gemeinde** im Reiter „Bestand & Potenziale"
    (R.13).
12. **Alle in R mit [Annahme] markierten Punkte bestätigen.**

---

## J. Parameter-/Konsistenz-Audit (Ergänzung)

Beim nächsten Review gegen das Backend-Modell prüfen, dass die neue
Terminologie (B) und die neuen experimentellen Prinzipien mit vorhandenen
Backend-Metriken hinterlegt sind bzw. korrekt als „Metrik in Vorbereitung"
gekennzeichnet bleiben. Der bekannte Grundsatz gilt weiter: eine Datenquelle
für Score, Häkchen/Begrenzer, Kartenfarbe und Blocker-Auswertung.

---

## J2. Farbsystem (Session 2026-10-12) — löst offene Frage I.1 ab

> **Geändert durch R (Session 2026-10-19):** Bewusste Ausnahme: ein grüner Punkt am Reiter „Szenarien vergleichen", wenn dort Szenarien der aktuellen Ebene liegen (R.9). Die Bereichs-Tabs bleiben farblos (R.3).

**Leitregel:** Die Oberfläche bleibt ruhig, Farbe gehört den Daten.

- **Helle Oberfläche.** Kopfzeile und Leiste über der Karte sind weiß mit
  dünner Linie (vorher dunkle Bänder, die stärker wirkten als die Karten).
  „← Deutschland" ist ein umrandeter, neutraler Knopf.
- **Ein neutraler Akzent** (`#1f2a33`, dunkles Schiefer) für aktive Zustände,
  Knöpfe, Links, Schalter und den aktiven Reiter. **Markengrün nur noch im
  Logo.** Grün ist sonst eine Datenfarbe (Prinzip Bevölkerungsfern, Seite A der
  Differenzkarte). Frühere grüne und bläuliche Flächentöne der Oberfläche sind
  neutral grau.
- **Farbe = Prinzip.** Die sechs Prinzipfarben bleiben unverändert
  (farbfehlsichtig-taugliche Reihe). Abgeleitete Karten bekommen keinen
  eigenen Prinzip-Farbton.
- **Konsens-Skala neutral** (entschieden, Session 2026-10-12): hellgrau →
  Schiefer `#34424f`. Neutral = Übereinstimmung, Farbe = Prinzip. Sie steuert
  Karte, Legende, Paar-Matrix sowie die großen Zahlen und Balken der rechten
  Spalte. Vorher Stahlblau `#2f6690`, das neben dem Blau von Bevölkerungsnah
  und der Wind-Markierung lag; die Vorschau zum Umschalten ist wieder entfernt.
- **Karten auf Weiß.** Kartenflächen und Galerie-Karten haben weißen Grund.
  Das helle Ende der Skalen ist sichtbar getönt, damit niedrige Werte nicht im
  Hintergrund verschwinden; die Differenzkarte hat einen neutral-grauen
  Mittelpunkt.
- **Drei Textgrautöne:** Text `#1b2126`, sekundär `#5b6672`, zurückgenommen
  `#6b7680` (vorher rund zehn Stufen; die hellsten waren auf dem grauen
  Seitengrund schwer lesbar).
- **Unverändert:** Wind/PV-Farben (blau/gelb) der Technologie-Markierung, die
  Farben der Differenzkarte (A grünblau, B braun), die braune Skala der
  Abweichungskarte.
- **A/B-Kennzeichen neutral:** Die runden Marken „A" und „B" sind dunkelgrau
  (`#34424f`) bzw. mittelgrau (`#8a94a0`), nicht mehr grünblau/braun. Welche
  Kartenfarbe zu welcher Seite gehört, sagt die Legende („mehr in A" / „mehr
  in B") an den Enden des Farbverlaufs.

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

> **Geändert durch R (Session 2026-10-19):** Ausnahme: die Budget-Zeilen der Regions-Headline stehen in GW mit zwei Nachkommastellen; Karte, Legende, rechte Spalte und Zelldetail bleiben in der Region bei MW (R.5).

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

> **Geändert durch R (Session 2026-10-19):** „← Deutschland" entfällt zugunsten der Bereichs-Tabs (R.3). Einstieg über einen hervorgehobenen Knopf „im Detail auswerten" (R.4). Das Regionsbudget ist die bundesweite **Verteilung**, nicht der Anteil am bundesweiten Konsens; es ist als Regionalziel einstellbar, und die Headline ist neu (R.5). Neu: Herleitungskette (R.6) und „veraltet"-Zustand (R.7). „Kein Pan/Zoom" gilt weiter für die Gerechtigkeitskarten, aber nicht für „Bestand & Potenziale" (R.13).

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
  die Ebene des Vergleichs als Dachzeile: „Deutschland" bzw. der Landkreis. Es
  bleiben „Übersicht" (Deutschland
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
>   wiederholen: „Regionsbudget: [X] MW — der Anteil dieses Landkreises am
>   bundesweiten Konsens." (kreisfreie Stadt: „dieser Stadt"; bei nur einem
>   bundesweiten Prinzip: „… an der bundesweiten Verteilung."). Ohne den Namen:
>   Der steht in der Kartenleiste und als Titel der rechten Spalte. Die
>   Kartenleiste zeigt in der Region nur noch den Namen („Landkreis Uelzen"),
>   nicht mehr „· Gemeinden + Raster" o. Ä. — das zeigt der Umschalter. Vorher: „Von [X] MW Regionsbudget liegen [Y] MW
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
> - **Einzelverteilungen in der Region zeigen Gemeinden** (Session 2026-10-12):
>   Die kleinen Karten der Galerie und die Vergrößerung („Einzeln ansehen")
>   zeigen in der Region Gemeinden statt des Rasters, mit derselben Einstellung
>   wie die große Karte (`regionViewMode`; Basic immer Gemeinden). Nur in der
>   Ansicht „Raster (5 × 5 km)" bleibt das Raster; „Gemeinden + Raster" zeigt in
>   den kleinen Karten ebenfalls Gemeinden. Gemeinde-Wert = flächengewichtete
>   Summe der Zellwerte; der Wertebereich darunter nennt die Einheit („je
>   Gemeinde" bzw. „je 5 × 5-km-Zelle"). Die Galerie hat keinen eigenen
>   Umschalter.
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

> **Geändert durch R (Session 2026-10-19):** Getrennt je Ebene, in beiden Bereichs-Tabs. A/B bleiben ohne gespeicherte Szenarien leer, statt mit den zwei Referenzen vorbelegt zu werden. Der Referenz-Hinweis wird durch einen Mengen-Hinweis ersetzt. Planungsflächen gibt es nur bei Wind (R.8).

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
- **Regionsvergleich erkennbar machen** (Session 2026-10-12): Stehen in A/B
  Regionsszenarien, zeigt die Differenzkarte das 5 × 5-km-Raster dieses
  Landkreises statt der Landkreise Deutschlands. Damit das nicht wie ein Fehler
  wirkt: (1) Kartentitel „Differenzkarte: A − B · [Landkreis]"; (2) Dachzeile
  der rechten Spalte nennt die Ebene — „Deutschland" bzw. den Landkreis;
  (3) der Umriss des Landkreises liegt über dem Raster (nur in der reinen
  Raster-Ansicht; mit Gemeinden ergibt sich die Form aus ihnen).
- **Gemeinden in der Differenzkarte** (Session 2026-10-12): Ein
  Regionsvergleich zeigt dieselben drei Ansichten wie die Regionsansicht —
  **Gemeinden** (Standard), **Gemeinden + Raster**, **Raster (5 × 5 km)** — und
  nutzt **dieselbe Einstellung** (`regionViewMode`): Umschalten hier schaltet
  auch die Regionsansicht um und umgekehrt. Umschalter rechts neben dem
  Kartentitel, nur in Erweitert; Basic zeigt immer Gemeinden ohne Umschalter.
  Auf Deutschland-Ebene gibt es nichts umzuschalten (Landkreise).
  - *Werte:* Szenarien speichern weiter je Rasterzelle. Der Wert einer
    Gemeinde ist die flächengewichtete Summe der Zellwerte (gleiche Gewichte
    wie in der Regionsansicht), für A, B und Referenzen; Differenz = A − B.
  - *Farbe:* „Gemeinden" färbt die Gemeinden nach ihrer Differenz, mit eigener
    Skala; „Gemeinden + Raster" färbt das Raster, die Gemeinden liegen als
    Grenzen darüber; „Raster" wie bisher.
  - *Klick:* In beiden Gemeinde-Ansichten ist die Gemeinde die klickbare
    Einheit (rechte Spalte: „Gemeinde …", Wert A, Wert B, Differenz), in
    „Raster" die Rasterzelle.
  - Solange die Gemeindegrenzen laden oder nicht ladbar sind, zeigt die Karte
    das Raster mit einem Hinweis unter dem Titel.
- **A und B unterscheidbar machen** (Session 2026-10-12):
  - *Auto-Name kurz:* ein Prinzip → dessen Name; ab zwei Prinzipien nur die
    Anzahl („3 Prinzipien"); dann Ebene („Deutschland" bzw. der Landkreis ohne
    das Wort „Landkreis") und Zieljahr bzw. GW. Beispiel: „6 Prinzipien ·
    Eichsfeld · 2035". Vorher stand die volle Prinzipienliste im Namen und
    wurde in den A/B-Boxen abgeschnitten. Der Name bleibt im Speichern-Dialog
    editierbar.
  - *Eindeutig:* Ist der Auto-Name schon vergeben, bekommt er eine laufende
    Nummer („… (2)"). Nötig, weil sich zwei Szenarien nur in den bundesweiten
    Prinzipien oder der Bestandsanrechnung unterscheiden können — beides steht
    nicht im Namen.
  - *Unterschiedszeile in den A/B-Boxen:* Stehen in A und B zwei Szenarien,
    zeigt jede Box unter dem Namen die eigenen Werte nur der Einstellungen,
    die sich unterscheiden: „Bundesweit: …", „Regional: …" (Deutschland:
    „Prinzipien: …"), „Ausbauziel: … GW", „Bestand angerechnet / nicht
    angerechnet". Prinzipien-Mengen als Anzahl, wenn die Anzahl verschieden
    ist, sonst mit Namen. Ohne Unterschied: „Gleiche Einstellungen wie B/A".
    Gegen eine Referenz keine Zeile.
  - Die Karten der rechten Spalte tragen dadurch den kurzen Namen als Titel;
    die Prinzipien stehen nur noch einmal, in den Zeilen darunter.
  - *Prinzipien als Liste* (rechte Spalte, Karten A/B): unter der Zeile
    „Prinzipien" (Region: „Prinzipien bundesweit" und „Prinzipien regional") steht je
    Prinzip eine eigene Zeile mit Farbfeld und Name — nicht mehr „A + B + C" als
    rechtsbündiger Fließtext. Die Tabellen „Gespeicherte Szenarien" bleiben
    einzeilig.
  - *Beschriftung:* „Prinzipien bundesweit" statt „Prinzipien DE" — in den
    Karten A/B und in den Tabellen (Regel „bundesweit", s. B). Ältere Stellen
    in diesem Dokument, die „Prinzipien DE" nennen, meinen dieselbe Zeile.
- **Viele Szenarien: Tabelle gedreht, Prinzipien als Farbpunkte** (Session
  2026-10-12). Mit elf gespeicherten Szenarien war die alte Tabelle (eine
  Spalte je Szenario, 220 px) rund 2600 px breit, und jeder Landkreis bekam
  eine eigene Tabelle.
  - *Eine Zeile je Szenario, zwei Tabellen.* Spalten: A/B-Kennzeichen ·
    Szenario · Ausbauziel · Zubaukonsens · % Konsens · Begrenzend · „Laden" und
    ×. Kein seitliches Scrollen mehr. Getrennt in **„Deutschland · GW"** und
    **„Regionen · MW"** (unterschiedliche Größenordnung); alle Landkreise
    stehen zusammen in „Regionen", nicht mehr je Landkreis eine Tabelle. Eine
    Gruppe ohne Szenarien erscheint nicht. Die früheren Zeilen „Prinzipien
    bundesweit / regional" sind durch die Farbpunkte **abgelöst**.
  - *Farbpunkte:* Über dem Namen steht je gewähltem Prinzip ein Farbpunkt — so
    lassen sich gleich lautende Namen („4 Prinzipien · Deutschland · 2035")
    unterscheiden. Regionsszenarien zeigen in einer zweiten Zeile den Landkreis
    und die bundesweiten Prinzipien als kleinere Punkte.
  - *Sortieren:* Klick auf einen Spaltenkopf sortiert aufsteigend, der zweite
    absteigend, der dritte stellt die Speicherreihenfolge wieder her. Werte
    werden intern in GW verglichen, also auch über Ebenen hinweg.
  - *„Laden"* ersetzt das Wort „Wiederherstellen" (gleiche Funktion: lädt die
    Konfiguration links), × löscht.
  - *A/B-Dropdowns:* Jeder Szenario-Eintrag trägt dieselben Farbpunkte vor dem
    Namen; Referenzen behalten ihr gestricheltes Kennzeichen.
  - *Noch offen:* Umbenennen nach dem Speichern; Suchfeld im Dropdown ab etwa
    zehn Einträgen; Szenarien gehen beim Neuladen verloren.
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
  unterschiedlich viel: [Referenz] [X] (das ganze Ausbauziel; im Regionsvergleich
  „das ganze Regionsbudget"), das Szenario [Y]
  (nur den Zubaukonsens). Die [X − Y] Unterschied stecken mit in der Karte. Sie
  zeigt also nicht nur, wo anders verteilt wird, sondern auch, dass [Referenz]
  mehr verteilt." Entfällt, wenn beide Summen gleich angezeigt würden (Konsens =
  Ziel). (Logik unverändert.)
- **Einstieg:** Haupteinstieg ist der Reiter; die Zeile „Gespeicherte Szenarien (N)
  · Vergleichen →" ist nur Shortcut und im Hub ausgeblendet. Leere GW-/MW-Tabellen
  werden nicht gerendert.

## P. Paarweise Prinzipien-Matrix & „Vergleich teilen" (Session 2026-10-09)

> **Geändert durch R (Session 2026-10-19):** Die Matrix zeigt immer alle wählbaren Prinzipien und ist ab einem gewählten Prinzip sichtbar; nicht gewählte Paare erscheinen als leere graue Zellen (R.11).

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
- „Zum Vergleich speichern" steht seit Session 2026-10-12 **am unteren Rand
  der linken Spalte, angeheftet** (bleibt beim Scrollen der Spalte sichtbar),
  nach allem, was es speichert: Prinzipien und der Block „RAHMEN". Zuvor direkt
  unter den Prinzipien, über „SZENARIO" — das wirkte, als speichere es nur die
  Prinzipien. Volle Breite, umrandet wie „Vergleich teilen"; nicht in der
  Tab-Reihe. Der Block mit Technologie, Ausbauziel und Bestandsanlagen heißt
  jetzt **„RAHMEN"** statt „SZENARIO", weil ein gespeichertes Szenario auch die
  Prinzipien umfasst. Sichtbar nur, wenn es etwas zu speichern gibt UND die aktuelle
  Konfiguration noch nicht gespeichert ist (gleiche Ebene, Prinzipien DE +
  regional, Technologie, Ausbauziel, Bestandsanrechnung). Gilt seit Session
  2026-10-12 auch im Hub (zuvor dort ausgeblendet).
- „Vergleich teilen" steht rechtsbündig unter der Tab-Linie, direkt über der
  Wind-/PV-Hub-Box. Die Tab-Reihe enthält nur noch die Reiter.
- **„Methodik" und „Daten herunterladen" stehen im Kopf** (Session 2026-10-12),
  nicht mehr am Ende der mittleren Spalte, wo ihre Position mit jedem Reiter
  wanderte. „Methodik" direkt neben „Was ist das?" (beide erklären das
  Werkzeug), „Daten herunterladen" rechts vor Basic/Erweitert (Aktion, mit
  Symbol). **Bewusst kein Dropdown** für die drei: „Was ist das?" ist der
  Einstieg für neue Besucher und soll sichtbar bleiben; bei nur drei Einträgen
  kostet ein Menü einen Klick, ohne Platz zu sparen. „Daten herunterladen" ist
  im Prototyp noch ohne Funktion; offen ist, ob der Download die aktuelle Karte
  oder den ganzen Datensatz liefert.
- **Methodik-Fenster** (Session 2026-10-12): „Methodik" öffnet ein zentriertes,
  scrollbares Fenster (Esc, × oder Klick daneben schließt). Acht Abschnitte:
  (1) Die Frage, (2) Wie ein Prinzip den Ausbau verteilt, (3) Zubaukonsens,
  (4) Was den Konsens begrenzt und wo Prinzipien abweichen, (5) Zwei Ebenen:
  Deutschland und Landkreis, (6) Rahmen, (7) Referenzen und Szenarien,
  (8) Grenzen. **Der Text beschreibt nur die Rechenregeln, wie die App sie
  umsetzt** (Minimum-Regel, 20-%-Schwelle, Regionsbudget, flächengewichtete
  Gemeindewerte, getrennte Zielberechnungen …). **Datenquellen und Formeln der
  Prinzipien stehen bewusst nicht darin:** Der Prototyp rechnet mit
  Platzhalterwerten; ein gelber Hinweis „Stand: Prototyp" am Anfang sagt das
  ausdrücklich. Vor einer Veröffentlichung mit echten Daten ergänzen:
  Datenquellen und Stand je Prinzip, die genaue Definition jedes Prinzips,
  Herkunft der Ausbauziele, Ansprechperson — und den Prototyp-Hinweis entfernen.
- **Hub-Aufbau (Session 2026-10-12):** In der Zeile über der Hub-Box steht der
  Wind-/PV-Umschalter mittig (er gilt für Box und Tabellen). „Vergleich teilen"
  steht **in** der Box „Vergleich A − B", oben rechts neben dem Titel (zuvor
  rechts in der Zeile darüber) — es teilt genau diesen Vergleich. Die farbige Box „Vergleich A − B" enthält Erklärzeile, A/B-Boxen und
  Differenzkarte. Die Tabellen je Ebene stehen **unter** der Box in einer
  eigenen Box gleicher Technologiefarbe („Gespeicherte Szenarien", mit der
  Erklärzeile „Die Tabellen stellen die gespeicherten Szenarien je Ebene
  gegenüber."); ohne gespeicherte Szenarien entfällt diese Box.
- Die Zeile „Gespeicherte Szenarien (N) · Vergleichen → · Nur für diese Sitzung."
  entfällt; Einstieg ist allein der Reiter. Der Session-only-Hinweis steht weiter
  im Speichern-Dialog.

---

## Q. Projekt- und Rechtsseiten (Session 2026-10-12) — Gliederung mit Platzhaltern

**Wo was steht**
- **Fußzeile** (neu, immer sichtbar, eine schmale Zeile am unteren Rand):
  rechtsbündig „Kontakt", „Impressum", „Datenschutz" — sonst nichts. Impressum
  und Datenschutz müssen von jeder Ansicht aus leicht erreichbar sein — deshalb
  die Fußzeile. Bewusst **nicht** in der Fußzeile: „Über das Projekt" (steht nur
  im Kopf) und die Zeile „Ein Projekt von … · gefördert durch …" (Partner und
  Förderung stehen im Fenster „Über das Projekt"). Das Fenster „Über das
  Projekt" hat umgekehrt keine Zeile „Rechtliches: Impressum · Datenschutz".
- **Logo:** Bilddatei `eequitymap-logo.png` (Deutschlandkarte mit Windrad,
  Solarmodul und Personen + Schriftzug „EEquityMap") links im Kopf, 46 px hoch,
  ebenso auf Impressum und Datenschutz. Der frühere Text-Schriftzug und der
  Untertitel „Wo wäre der Ausbau fair?" im Kopf entfallen. Die Datei ist aus
  einem Bildschirmfoto mit weißem Grund erzeugt — für die Veröffentlichung
  durch die Originaldatei (SVG oder PNG mit Transparenz) ersetzen.
- **Kopf:** „Was ist das?", „Methodik" und „Über das Projekt" stehen als
  schlichte Textlinks ohne Rahmen nebeneinander (Hover: leichter grauer Grund).
  Der einzige umrandete Knopf im Kopf ist die Aktion „Daten herunterladen";
  Basic/Erweitert bleibt der Umschalter. Vorher sahen alle fünf Einträge wie
  Knöpfe aus.
- **„Über das Projekt"** ist ein Fenster wie die Methodik (Esc, ×, Klick
  daneben schließt). Abschnitte: Das Projekt · Partner (Logo, Name, Rolle) ·
  Förderung · Kontakt · Zitieren und weiterverwenden (Zitiervorschlag, Lizenzen)
  · Stand (Version, Datenstand). „Kontakt" in der Fußzeile öffnet dasselbe
  Fenster beim Abschnitt Kontakt. Kein Kontaktformular (E-Mail genügt und
  vermeidet zusätzliche Datenschutzpflichten).
- **Impressum** und **Datenschutz** sind eigene Seiten mit eigener Adresse
  (`impressum.html`, `datenschutz.html`), damit sie direkt verlinkbar sind und
  ohne das Werkzeug laden. Sie binden bewusst keine externen Schriften oder
  Skripte ein.

**Alles noch Platzhalter.** Fehlende Angaben sind sichtbar markiert
(„[Platzhalter: …]", gelb gestrichelt); jede der drei Stellen trägt oben einen
Entwurfs-Hinweis. Vor einer Veröffentlichung nötig:
- Partner mit Namen, Rollen und Logos; Fördergeber mit Programm und Kennzeichen.
- Anbieter mit Anschrift, Vertretung, Kontakt, inhaltlich Verantwortliche.
- Ansprechperson und E-Mail; Zitiervorschlag; Lizenzen; Version/Datenstand.
- Datenschutz: Hosting-Anbieter, Rechtsgrundlagen, Speicherdauern, Rechte,
  Aufsichtsbehörde. Texte von der rechtlich verantwortlichen Stelle prüfen
  lassen.
- Falls eine öffentliche Stelle Anbieter ist: zusätzlich „Barrierefreiheit"
  (Erklärung) in die Fußzeile.

**Erledigt, technisch (Session 2026-10-12): nichts wird mehr von Dritten geladen.**
- **Schriften** liegen in `fonts/` (IBM Plex Sans als eine variable Datei für
  400–700, IBM Plex Mono 400/500/600; Teilmengen latin und latin-ext) mit
  `fonts/fonts.css` und der Lizenz `fonts/OFL.txt` (SIL Open Font License).
- **React, ReactDOM und Babel** liegen unverändert in `vendor/` (react@18.3.1,
  react-dom@18.3.1, @babel/standalone@7.29.0). In `support.js` zeigen nur die
  drei Adressen auf `./vendor/…`; die Prüfsummen dort gelten weiter und stimmen
  mit den heruntergeladenen Dateien überein. Babel wird im normalen Betrieb
  nicht angefordert, bleibt aber als Rückfall der Laufzeit vorhanden.
- Geprüft im Browser: Alle Anfragen gehen an den eigenen Server. Impressum und
  Datenschutz nutzen dieselben lokalen Schriften; der Datenschutz-Entwurf sagt
  jetzt, dass keine Inhalte von Dritten geladen werden.
- **`EEquityMap.html` entfernt:** die ältere, gebündelte Einzeldatei (Stand
  27.08., seit dem ersten Commit unverändert) wurde gelöscht. Sie war nirgends
  eingebunden, zeigte einen veralteten Stand und verwies noch auf Google Fonts
  und unpkg. Ausgeliefert wird allein `EEquityMap.dc.html`.

---

## R. Bereichs-Tabs, Regionalziel, Begriffe (Session 2026-10-19)

**Herkunft:** Änderungswünsche aus dem Projektteam (Folien „Neue Tabs" u. a.),
in dieser Session geklärt. Stellen, die eine Annahme von Claude sind und noch
bestätigt werden müssen, sind mit **[Annahme]** markiert (s. I.12).

**Löst ab bzw. ändert:** M (Karten-Abstraktion, Regions-Headline, Einstieg,
Zurück-Navigation), O (A/B-Defaults, Hub-Inhalt, Referenz-Hinweis),
P (Paar-Matrix), J2 (Ausnahme grüner Punkt), L (GW in zwei Regionszeilen),
A/E (Begriff „Neuzubau"), C/G (Reitername „Konfliktflächen"). Die betroffenen
Abschnitte tragen oben einen Verweis hierher; Reject-Log (H) und offene Fragen
(I.10–I.12) sind ergänzt.

**Umsetzung in fünf Schritten (je ein Prompt für Claude Code):**
1. Begriffe, Infoboxen, kleine Korrekturen (R.1, R.2, R.9–R.12, Fehler aus R.7)
2. Bereichs-Tabs: Gerüst + Detailauswertung (R.3, R.4)
3. Regionsmodus: Regionalziel, Herleitung, „veraltet" (R.5–R.7)
4. Szenarien vergleichen je Ebene (R.8)
5. Reiter „Bestand & Potenziale" (R.13)

### R.1 Begriffe: Ausbau, Bestand, Zubau (verbindlich, im ganzen Werkzeug)

- **Ausbau** = die installierte Leistung, die eine Verteilung insgesamt
  vorsieht. Das **Ausbauziel** ist ein Ausbau-Wert (inkl. Bestand, wenn dieser
  berücksichtigt wird).
- **Bestand** = heute installierte Leistung.
- **Zubau** = zusätzlich nötige Leistung: `Zubau = max(Ausbau − Bestand, 0)`.
- Ohne Bestandsberücksichtigung (Greenfield) gilt Zubau = Ausbau.
- **Bestand wird nie zurückgebaut.** Liegt der Bestand über dem verteilten
  Ausbau, bleibt er stehen; dort ist der Zubau 0. Folge (bewusst akzeptiert):
  Die tatsächlich installierte Leistung kann in der Summe über dem Ausbauziel
  liegen.
- **Umbenennungen:** „Neuzubau" → „Zubau"; „Ausbau gesamt" → „Ausbau".
  „Zubaukonsens" bleibt (Konsens über den Zubau, s. decisions).
  Teilmengen-Regel aus A gilt weiter: Zubaukonsens ≤ Zubau ≤ Ausbau.
- **Schalter:** „Bestandsanlagen anrechnen" → **„Bestandsanlagen
  berücksichtigen"** (überall, auch in Szenario-Unterschiedszeilen:
  „Bestand berücksichtigt / nicht berücksichtigt").
- Hinweis zur Formel der Vorlage („Ausbau = Bestand + Zubau"): stimmt nur, wo
  der Bestand den Ausbau nicht übersteigt. Deshalb steht in den Texten die
  max-Formel. **[Annahme — bestätigen]**

**Offen (Team):** Bestandsberücksichtigung standardmäßig an? Aktuell aus
(Greenfield). Für „aus": die Frage des Werkzeugs ist „wo wäre der Ausbau
fair?", die Galerie verliert mit Bestand an Kontrast, und der neue Reiter
„Bestand & Potenziale" zeigt den heutigen Stand bereits. Für „an": Planende
fragen meist „wie viel kommt hier noch dazu?". → Mit Öko-Institut /
EmPowerPlan klären (offene Frage I.10).

### R.2 Begriff „Gerechtigkeitsprinzip"

- Es bleibt bei „Prinzip". Wo der Begriff eingeführt wird (Überschrift der
  Prinzipienliste, erste Zeile einer Infobox, Methodik), steht einmal das
  volle Wort **„Gerechtigkeitsprinzip"**; danach genügt „Prinzip".
- „Gerechtigkeitsvorstellung" wird nicht verwendet.

### R.3 Bereichs-Tabs: Bestand & Potenziale · Deutschland · Regional

**Problem:** Der Wechsel DE ↔ Region ist zu wenig herausgestellt; beim
Wechsel ändern sich zu viele Stellen gleichzeitig.

**Grundgedanke:** Ich gehe von heute aus (Bestand & Potenziale), untersuche
dann Gerechtigkeit bundesweit (Deutschland) und danach innerhalb eines
Landkreises (Regional).

- **Ort:** Die drei Bereichs-Tabs stehen in der Kartenleiste, dort wo bisher
  „← Deutschland" und der Ortsname stehen. **Das Suchfeld bleibt.**
- **Hierarchie:** Linke Spalte, Mitte und rechte Spalte gehören zum aktiven
  Bereichs-Tab. Die bisherigen Reiter (Konsens, Was begrenzt?, Dissens,
  Einzelverteilungen, Szenarien vergleichen) sind Unter-Reiter von
  „Deutschland" und „Regional". „Bestand & Potenziale" hat eigene
  Unter-Reiter (R.13).
- **Kennzeichnung:** nur Grautöne bzw. eine neutrale Umrandung, die den aktiven
  Tab mit seinem Inhalt verbindet (Ordner-Optik). Keine Farbe (J2: Farbe gehört
  den Daten).
- **Landkreisname** im Regional-Tab: „Regional · [Landkreis]". **[Annahme]**
- **„← Deutschland" entfällt**; zurück geht es über den Tab „Deutschland".
- **Regional ohne gewählten Landkreis** (direkt auf den Tab geklickt):
  Leerzustand „Wählen Sie einen Landkreis — über die Suche oder auf der
  Deutschlandkarte."
- **Zustand bleibt erhalten:** Wer zwischen den Tabs wechselt, findet die
  Region so wieder, wie er sie verlassen hat (regionale Prinzipien,
  Regionalziel, berechnetes Ergebnis, Ansicht). Wird ein **anderer**
  Landkreis gewählt, startet dieser mit den Voreinstellungen (regionale
  Prinzipien = bundesweite, Regionalziel = DE-Empfehlung). **[Annahme]**
- **Suchfeld je Tab** **[Annahme]**: Deutschland — wählt den Landkreis aus
  (Popover, wie bisher); Regional — wechselt in diesen Landkreis; Bestand &
  Potenziale — zoomt auf den Landkreis.

**Nachtrag (Session 2026-10-19, nach Umsetzung):**
- **Start im Bereich „Bestand & Potenziale"** (statt „Deutschland"). Folgt dem
  Grundgedanken: erst der heutige Stand, dann Gerechtigkeit bundesweit, dann
  regional. Das Prämissen-Overlay beim Erststart bleibt; sein Knopf heißt
  „Los geht's →" (vorher „Verteilungen ansehen →", das hätte direkt nach
  „Deutschland" geführt) und schließt nur das Overlay. Der Erststart im
  Bereich „Deutschland" (Galerie aller Prinzipien, CTA) bleibt unverändert und
  greift beim ersten Wechsel dorthin.
- **Regional ohne bundesweites Prinzip:** Solange in „Deutschland" kein
  Prinzip gewählt ist, gibt es kein Regionsbudget. Der Tab zeigt dann statt der
  Region (oder des Leerzustands „Wählen Sie einen Landkreis …") den Hinweis
  „Wählen Sie zuerst im Bereich „Deutschland" mindestens ein
  Gerechtigkeitsprinzip. Daraus ergibt sich, wie viel Ausbau auf einen
  Landkreis entfällt." mit dem Knopf „Zum Bereich Deutschland". Die Suche
  öffnet in diesem Zustand keinen Landkreis. (Vorher entstand eine Region mit
  Regionsbudget 0.)

### R.4 „Im Detail auswerten" (Einstieg in den Regionsmodus)

- **Aktion stärker:** Die Aktion im Popover „[Landkreis] im Detail auswerten →"
  ist ein gefüllter Knopf in der Akzentfarbe (bisher zurückgenommen). Sie
  wechselt in den Tab „Regional".
- **Einmaliger Hinweis über der Deutschlandkarte:** „Klicken Sie auf einen
  Landkreis, um ihn im Detail auszuwerten." Verschwindet nach dem ersten Klick
  auf einen Landkreis und kommt in der Sitzung nicht wieder (K: einmalige
  Lehre).
- **Info „Was passiert in der Detailauswertung?"** als „i" neben der Aktion im
  Popover und neben dem Regionsbudget im Tab „Regional" **[Annahme]**. Text:

  > **Was passiert in der Detailauswertung?**
  > Die Deutschlandrechnung legt fest, wie viel Ausbau nach den bundesweit
  > gewählten Gerechtigkeitsprinzipien auf diesen Landkreis entfällt
  > (Regionsbudget). In der Detailauswertung wird dieses Regionsbudget
  > innerhalb des Landkreises auf einem 5 × 5-km-Raster neu verteilt.
  > Ausgangspunkt sind dieselben Gerechtigkeitsprinzipien wie bundesweit. Sie
  > können sie ändern und prüfen, wie sich dasselbe Regionsbudget nach anderen
  > Prinzipien verteilen würde.
  > Auch das Regionsbudget lässt sich ändern. Weicht es vom Ergebnis der
  > Deutschlandrechnung ab, wird das gekennzeichnet: Es entspricht dann nicht
  > mehr der bundesweiten Empfehlung.

### R.5 Regionsmodus: linke Spalte und Regionalziel

**Neu: Die Leistung ist in beiden Modi einstellbar.**

- **Deutschland:** linke Spalte unverändert — mit einer Ausnahme (Nachtrag,
  Session 2026-10-19): der Satz „Bei [X] GW liegen [Y] GW im Konsens." unter
  dem Regler entfällt auch hier. Die Headline über der Karte („Von [X] GW
  Ausbauziel liegen [Y] GW im Konsens …") nennt dieselben Zahlen.
- **Regional:**
  - Bleiben: „Bundesweit: [n] Prinzipien" (Kontext, zugeklappt), die regionale
    Prinzipienauswahl, Technologie, „Bestandsanlagen berücksichtigen".
  - **Entfällt:** der Textblock „Bei [X] MW liegen [Y] MW im Konsens."
  - **Keine Zieljahre** (Marken 2030/2035/2045 ausgeblendet).
  - **Regler „Ausbauziel"** für den Landkreis:
    - Bereich 0 … maximales Leistungspotenzial des Landkreises für die gewählte
      Technologie (aus den Potenzialflächen, R.13).
    - Voreinstellung = **DE-Empfehlung**: der Ausbau, den die bundesweite
      Verteilung (gewichtete Kombination der bundesweit gewählten Prinzipien,
      wie „Zubau" in A) diesem Landkreis zuweist. Das ist **die Verteilung,
      nicht der bundesweite Konsens**. → Ändert M (dort: „Anteil dieses
      Landkreises am bundesweiten Konsens").
    - **Ein eigenes Regionalziel bleibt bestehen**, auch wenn sich die
      bundesweiten Prinzipien ändern. Dann ändert sich nur die angezeigte
      DE-Empfehlung.
    - Daneben ein Link „Auf DE-Empfehlung zurücksetzen", nur sichtbar bei
      eigenem Ziel. **[Annahme]**
- **Headline über der Karte** (ersetzt die Regions-Headline aus M):
  - Ohne eigenes Ziel: „Regionsbudget: 0,39 GW — der Anteil dieses Landkreises
    laut bundesweiter Verteilung."
  - Mit eigenem Ziel, deutlich als Abweichung gekennzeichnet, zweizeilig:
    „DE-Empfehlung für den Landkreis: 0,39 GW" /
    „Aktuell untersuchtes Regionalziel: 0,50 GW"
- **Einheit (Ausnahme zu L):** Diese Budget-Zeilen stehen in **GW** (zwei
  Nachkommastellen). Karte, Legende, rechte Spalte und Zelldetail bleiben in der
  Region bei **MW**.
- **Szenarien** speichern das Regionalziel und ob es die DE-Empfehlung oder ein
  eigenes Ziel ist. Unterschiedszeile in den A/B-Boxen (O): „Regionalziel:
  0,50 GW (eigenes)" bzw. „(DE-Empfehlung)".

### R.6 Rechte Spalte im Regionsmodus: Herleitung von oben nach unten

Damit dauerhaft sichtbar bleibt, woher das Regionsbudget kommt und was wo gilt,
zeigt die rechte Spalte im Tab „Regional" oben eine Herleitungskette:

**Prinzipien bundesweit → Regionsbudget → Prinzipien regional → Zubaukonsens**

- Regionsbudget mit Kennzeichnung „DE-Empfehlung" bzw. „eigenes Ziel" (dann
  zusätzlich die DE-Empfehlung klein daneben).
- Letztes Glied je nach Auswahl: „Zubaukonsens der Region" (nichts gewählt),
  „… der Gemeinde [Name]" bzw. „… der Rasterzelle".
- Prinzipien als Farbpunkt + Name, in derselben Reihenfolge und Darstellung wie
  in der linken Spalte.
- **Der bundesweite Kontext in der linken Spalte bleibt.** Links steht die
  Einstellung (was gilt), rechts die Herleitung (wie das Ergebnis entsteht).
  Beide Stellen müssen identisch dargestellt sein (Farben, Reihenfolge, Namen).

### R.7 „Ergebnis veraltet" im Regionsmodus

- **Fehler beheben:** Jede Eingabe, die das Regionsergebnis ändert, setzt den
  Zustand „nicht aktuell": regionale Prinzipien, Regionalziel, Technologie,
  Bestandsberücksichtigung und bundesweite Prinzipien (diese ändern die
  DE-Empfehlung). Bisher löste eine Änderung am Leistungsregler die Box nicht
  zuverlässig aus.
- **Stärker zeigen:** zusätzlich zur Box (ein Ort, K) eine deutliche Umrandung
  der Karte in der Akzentfarbe (`#1f2a33`, 2 px, gestrichelt) und die bestehende
  Abdimmung. Verschwindet mit „Neu berechnen".

### R.8 Szenarien vergleichen: je Ebene getrennt (ändert O)

- „Szenarien vergleichen" ist Unter-Reiter in **beiden** Bereichs-Tabs.
  - **Deutschland:** nur Deutschland-Szenarien + Referenzen auf
    Deutschland-Ebene.
  - **Regional:** nur Regions-Szenarien + Referenzen **für diese Region**
    (nicht Deutschland).
- **Dropdowns in der Region:** gruppiert je Landkreis, sobald Szenarien aus mehr
  als einem Landkreis existieren. Vergleichbar bleiben nur Objekte desselben
  Landkreises: Steht in A ein Szenario aus Landkreis X, sind in B Szenarien
  anderer Landkreise ausgegraut („anderer Landkreis"). Referenzen gelten immer
  für den aktuellen Landkreis.
- **Voreinstellung A/B:**
  - Keine Szenarien der aktuellen Ebene (bzw. des aktuellen Landkreises)
    gespeichert → **A und B bleiben leer**, Hinweis: „Noch keine Szenarien
    gespeichert. Zum Vergleich stehen nur die Referenzen zur Verfügung."
    Die Referenzen sind im Dropdown wählbar. → Ändert O („ohne Szenario die zwei
    Referenzen").
  - Regional: Voreinstellung sind Szenarien des aktuell gewählten Landkreises.
  - Ein bzw. zwei Szenarien vorhanden: Regel aus O bleibt. **[Annahme]**
- **Referenzen in der Region** sind auf das **aktuell untersuchte Regionalziel**
  skaliert. Bei eigenem Ziel Hinweis: „Die Referenzen sind auf das aktuell
  untersuchte Regionalziel (0,50 GW) skaliert, nicht auf die DE-Empfehlung
  (0,39 GW)."
- **Planungsflächen nur bei Wind.** Bei PV steht als Referenz nur „Historischer
  Ausbau" zur Verfügung.
- **Ein Hinweis für unterschiedliche Mengen** (ersetzt den Referenz-Hinweis aus
  O; nie zwei Hinweise mit derselben Aussage, K). Erscheint immer, wenn A und B
  unterschiedliche Summen verteilen (zwei Szenarien oder Szenario/Referenz);
  entfällt, wenn die angezeigten Summen gleich sind:

  > A und B verteilen unterschiedlich viel Leistung: A [X], B [Y]. Die Karte
  > zeigt deshalb nicht nur, wo anders verteilt wird, sondern auch diesen
  > Mengenunterschied von [Δ].

  Zusatzsatz nur bei Szenario gegen Referenz: „[Referenz] verteilt das ganze
  Ausbauziel, das Szenario nur seinen Zubaukonsens." Offene Frage I.9 bleibt
  offen.
- **Infobox „Szenarien vergleichen"** („i" an der Erklärzeile des Hubs):

  > **Was kann ich hier vergleichen?**
  > Sie können gespeicherte Szenarien miteinander vergleichen — auf
  > Deutschlandebene oder innerhalb desselben Landkreises. Zusätzlich lassen
  > sich Szenarien mit Referenzen vergleichen: dem historischen Ausbau oder den
  > ausgewiesenen Planungsflächen (nur Wind).
  > Wichtig: A und B können unterschiedlich viel Leistung verteilen. Die Karte
  > zeigt dann zugleich Unterschiede in der räumlichen Verteilung und in der
  > Menge. Am aussagekräftigsten ist der Vergleich bei ähnlichen Ausbauzielen.
  > Deutschland- und Regionsszenarien sowie Wind und PV werden nicht
  > miteinander verglichen.

### R.9 Speichern und Wiederfinden

- **Hinweis am Knopf „Zum Vergleich speichern":** solange noch kein Szenario
  gespeichert ist und der Knopf sichtbar ist, steht direkt darüber eine Zeile
  „Speichern Sie diese Einstellung, um sie später zu vergleichen." und der Knopf
  ist leicht hervorgehoben (Tönung, kein Puls). Entfällt nach dem ersten
  Speichern.
- **Nach dem ersten Speichern** (Toast, einmalig): „Gespeichert. Vergleichen
  Sie Ihre Szenarien im Reiter „Szenarien vergleichen"."
- **Grüner Punkt** neben dem Reiter „Szenarien vergleichen", wenn dort
  gespeicherte Szenarien der aktuellen Ebene liegen. **Bewusste Ausnahme von
  J2** (Grün sonst nur Logo und Daten).

### R.10 Reiter „Konfliktflächen" → „Dissens"

- Reitername „Dissens" (nicht „Dissenz"). Unter-Ansichten „Wie stark?" /
  „Welche Prinzipien?" und alle Captions bleiben. Im Fließtext weiter
  „abweichen / Abweichung".
- Hinweis: Ältere Stellen in diesem Dokument, die „Dissens" für die
  Unter-Ansicht „Wie stark?" verwenden, meinen die Unter-Ansicht, nicht den
  Reiter.

### R.11 Paarweise Matrix: feste Größe (ändert P)

- Die Matrix zeigt **immer alle wählbaren Prinzipien**, unabhängig von der
  Auswahl. Damit bleibt sie gleich groß.
- Paare mit mindestens einem nicht gewählten Prinzip: **leere graue Zelle**,
  ohne Wert. Namen nicht gewählter Prinzipien in den Köpfen ausgegraut.
- **Sichtbar ab einem gewählten Prinzip** (dann sind alle Zellen leer).
- Sonst unverändert: Erweitert, Reiter Konsens, DE und Region.

### R.12 Infoboxen (Texte)

**Einstieg** (ersetzt den vorhandenen Text an der Prinzipienliste):

> **Was kann ich hier tun?**
> Wählen Sie ein Gerechtigkeitsprinzip, um zu sehen, wie es den Ausbau
> verteilt. Wählen Sie mehrere, um zu sehen, wo sie übereinstimmen, wo sie
> voneinander abweichen und was den Konsens begrenzt.

**Bestand** („i" am Schalter „Bestandsanlagen berücksichtigen"):

> **Was bedeutet „Bestandsanlagen berücksichtigen"?**
> Ohne diese Option zeigt EEquityMap eine Greenfield-Verteilung: Der Ausbau
> wird so verteilt, als würde neu geplant — unabhängig davon, wo heute schon
> Anlagen stehen.
> Mit dieser Option bleibt die vorhandene Leistung bestehen. Für jeden
> Landkreis bzw. jede Rasterzelle gilt: Liegt der verteilte Ausbau unter dem
> Bestand, ist kein Zubau nötig. Liegt er darüber, ist die Differenz der Zubau.
> Bestand wird also nicht zurückgebaut; er verringert nur, wie viel noch
> zugebaut werden muss.

**Begriffe** (neuer Absatz im Methodik-Abschnitt „Rahmen"):

> **Ausbau, Bestand, Zubau**
> Ausbau ist die installierte Leistung, die eine Verteilung insgesamt
> vorsieht; das Ausbauziel gibt sie vor. Bestand ist die heute schon
> installierte Leistung. Zubau ist die zusätzlich nötige Leistung:
> Zubau = Ausbau − Bestand, mindestens 0. Ohne Berücksichtigung des Bestands
> (Greenfield) ist der Zubau gleich dem Ausbau.

### R.13 Reiter „Bestand & Potenziale" (neu)

**Zweck:** Ausgangspunkt — was ist heute da, womit arbeiten wir, warum.

- **Unter-Reiter:** „Bestand heute" · „Potenzialflächen" (Flächen der gewählten
  Technologie) · „Planungsflächen" (nur Wind; bei PV ausgeblendet; ist er aktiv
  und wird auf PV gewechselt, springt die Ansicht auf „Potenzialflächen").
- **Linke Spalte:** nur Technologie (Wind/PV).
- **Karte frei zoombar** (Reversal von M „kein Pan/Zoom" — nur für diesen
  Reiter). Verwaltungsebene nach Zoomstufe: weit = Landkreise, nah = Gemeinden.
  Umsetzung im Prototyp mit D3-Zoom auf SVG; Ziel ist, das Verhalten der echten
  Anwendung zu zeigen, nicht ihre Technik.
- **Umschalter an der Karte (nur Erweitert):** „Administrativ" (Werte je
  Landkreis/Gemeinde, je nach Zoom) · „Rohdaten" (Potenzial- und
  Planungsflächen als Flächen, Bestand als Anlagen-Punkte). Rohdaten im
  Prototyp als **Platzhalter** (erzeugte Formen bzw. Punkte), solange sie
  glaubwürdig aussehen und bedienbar sind. Basic zeigt immer „Administrativ".
- **Klick → rechte Spalte:**
  - Bestand heute: installierte Leistung in MW.
  - Potenzial- und Planungsflächen: Fläche in ha, Leistungspotenzial in MW,
    energetisches Potenzial in GWh.
  - Ohne Auswahl: Summen für Deutschland. **[Annahme]**
- **Daten:** Platzhalterwerte je Landkreis und Gemeinde für Bestand,
  Potenzialflächen und Planungsflächen, je Technologie. Dieselben Werte liefern
  das Maximum des Regionalziel-Reglers (R.5) und die Referenz
  „Planungsflächen".

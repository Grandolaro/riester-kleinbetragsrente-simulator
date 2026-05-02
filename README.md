# Riester-Kleinbetragsrente-Simulator

Ein kleiner statischer Browser-Simulator zur Abschätzung, ob ein Riester-Guthaben beim Renteneintritt noch als Kleinbetragsrente gilt und damit voraussichtlich als Einmalzahlung ausgezahlt werden könnte.

## Zweck

Viele Riester-Sparer mit kleineren Vertragsguthaben wollen einschätzen, ob ihr Kapital zum Rentenbeginn unter der Grenze für die Kleinbetragsrente liegt. Dieser Simulator berechnet dafür mehrere Zukunftsszenarien und zeigt das maximal zulässige Kapital je Szenario an.

Die Anwendung ist bewusst einfach gehalten:

- keine Registrierung
- kein Backend
- keine externen Abhängigkeiten
- nur eine statische HTML-Datei

## Was der Simulator berechnet

Der Simulator kombiniert:

- verschiedene jährliche Entwicklungen des Rentenfaktors
- verschiedene jährliche Entwicklungen der Bezugsgröße

Für jede Kombination wird berechnet, welches maximale Kapital beim Renteneintritt noch innerhalb der Grenze für die Kleinbetragsrente liegt.

Die Matrix zeigt also nicht die Entwicklung eines konkreten Vertrags, sondern eine Szenarioanalyse.

## Eingaben

- `Jahr des Renteneintritts`
- optional: `Rentenfaktor 2026`
- optional: `Voraussichtliches Kapital zum Renteneintritt`

Das Feld für das voraussichtliche Kapital verändert die Berechnung nicht. Es dient nur dazu, die Ergebnismatrix farblich als Heatmap hervorzuheben.

## Ausgabe

Die Anwendung zeigt:

- eine Szenario-Matrix mit maximal zulässigem Kapital
- eine farbliche Heatmap für ein optional eingegebenes Zielkapital
- eine Detailerläuterung für einzelne Matrixzellen

## Fachliche Grundlage

Die aktuelle Berechnung verwendet im Code folgende Startwerte:

- `START_JAHR = 2026`
- `BEZUGS_GROESSE_2026 = 47.460 €`
- monatliche Grenze für die Kleinbetragsrente 2026: `39,55 €`

Zusätzlich wird mit einem anpassbaren Rentenfaktor für 2026 gearbeitet. Standardwert in der Oberfläche ist `27,0`.

Vereinfacht gilt:

- Jahre bis Renteneintritt = `Rentenjahr - 2026`
- projektierter Rentenfaktor = `Rentenfaktor_2026 * (1 + rF/100)^Jahre`
- projektierte Kleinbetragsgrenze = `B_2026 * (1 + rB/100)^Jahre`
- maximales Kapital = `projektierte Kleinbetragsgrenze / (projektierter Rentenfaktor / 10000)`

## Projektstruktur

- `index.html` enthält die vollständige Anwendung inklusive HTML, CSS und JavaScript
- `README.md` beschreibt die Anwendung aus Nutzersicht
- `AGENTS.md` enthält den kompakten technischen und fachlichen Überblick für Bearbeitung im Repository

## Nutzung

Die Anwendung kann direkt im Browser geöffnet werden:

1. Repository lokal auschecken oder herunterladen.
2. `index.html` im Browser öffnen.
3. Renteneintrittsjahr und optional weitere Werte eingeben.

Es ist kein Build-Schritt erforderlich.

## Hinweise

- Der Simulator ist ein vereinfachtes Szenario-Werkzeug und keine Rechts- oder Steuerberatung.
- Bei fachlichen Änderungen an Grenzwerten oder Rechtsgrundlagen sollten UI-Texte und Konstanten gemeinsam aktualisiert werden.

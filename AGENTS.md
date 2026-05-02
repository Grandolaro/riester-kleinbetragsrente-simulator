# Repository Guide

## Zweck

Dieses Repository enthält einen statischen Browser-Simulator zur Abschätzung, ob ein Riester-Guthaben beim Renteneintritt noch als Kleinbetragsrente gilt und damit als Einmalzahlung ausgezahlt werden könnte.

Der Simulator betrachtet dafür verschiedene Szenarien für:

- die jährliche Entwicklung des Rentenfaktors
- die jährliche Entwicklung der sozialversicherungsrechtlichen Bezugsgröße

Aus diesen Szenarien berechnet er das jeweils maximal zulässige Kapital zum Renteneintritt.

## Aufbau

- `index.html`: vollständige Anwendung als Single-File-Seite mit HTML, CSS und JavaScript
- `AGENTS.md`: kurzer Überblick über Zweck, Struktur und zentrale Rechenlogik

Es gibt aktuell keinen Build-Prozess, keine externen Abhängigkeiten und keine getrennten Quellmodule.

## Fachliche Logik

Wichtige fachliche Annahme:

- Der Simulator unterstützt nur noch Rentenbeginn ab `2027`.
- Der Simulator rechnet ausschließlich mit der angehobenen Kleinbetragsgrenze von `1,5 %` der monatlichen Bezugsgröße.
- Frühere Rechenlogik mit `1,0 %` wird fachlich nur noch als Änderungshinweis erwähnt, aber nicht mehr in der Berechnung unterstützt.
- Diese Anpassung wurde im Repository ausdrücklich gewünscht und ist deshalb in UI-Texten, Formeln, Konstanten, Validierung und Doku konsistent nachzuhalten.

Die zentrale Berechnung verwendet als Startwert:

- `START_JAHR = 2026`
- `MIN_RENTENJAHR = 2027`
- `BEZUGS_GROESSE_2026 = 47460.0`
- `B_2026 = 47460 * 0.015 / 12 = 59,325 €` als monatliche Grenze für die Kleinbetragsrente ab 2027 auf Basis 2026

Zusätzlich wird ein anpassbarer Rentenfaktor für 2026 verwendet. Standardwert in der Oberfläche ist `27,0`.

Für das gewählte Renteneintrittsjahr projiziert der Simulator:

1. den Rentenfaktor
2. die Grenze für die Kleinbetragsrente
3. das daraus maximal zulässige Kapital

Die Matrix wird aus diesen Szenarioachsen gebildet:

- Rentenfaktorentwicklung: `-2,0 %` bis `+2,0 %`
- Bezugsgrößenentwicklung: `0,0 %` bis `+3,0 %`

Formelidee:

- Jahre bis Renteneintritt = `Rentenjahr - 2026`
- Projektierter Rentenfaktor = `Rentenfaktor_2026 * (1 + rF/100)^Jahre`
- Projektierte Kleinbetragsgrenze = `B_2026 * (1 + rB/100)^Jahre`
- Maximales Kapital = `Projektierte Kleinbetragsgrenze / (projektierter Rentenfaktor / 10000)`

Fachlicher Hintergrund der Umstellung:

- Im BMF-Regierungsentwurf zum Altersvorsorgereformgesetz vom 1. Dezember 2025 wird in der Begründung zu `§ 93 Abs. 3 EStG` ausdrücklich auf die Anhebung der Grenze von `1,0 %` auf `1,5 %` Bezug genommen.
- Für dieses Repository ist festgelegt, dass der Rechner daraus nur noch die neue Logik ab `2027` unterstützt.

## UI-Verhalten

Die Anwendung bietet:

- Eingabe des Renteneintrittsjahres
- optionale Anpassung des Rentenfaktors 2026
- eine Ergebnismatrix mit maximal zulässigem Kapital je Szenario
- eine Heatmap für ein optional eingegebenes erwartetes Kapital
- eine Detailerläuterung pro ausgewählter Matrixzelle

## Hinweise für Änderungen

- Änderungen betreffen derzeit fast immer `index.html`.
- Rechenlogik und UI sind nicht getrennt; vor Refactorings erst Verantwortlichkeiten trennen.
- Bei fachlichen Änderungen an Grenzwerten oder Rechtsgrundlagen sollten Texte im UI und Konstanten im Skript konsistent angepasst werden.
- Bei Änderungen an Verhalten, Eingaben, Berechnungslogik, Standardwerten oder Projektstruktur ist zu prüfen, ob auch `README.md` und/oder `AGENTS.md` aktualisiert werden müssen.
- Dokumentationsänderungen sollen im selben Änderungsvorgang erfolgen wie die eigentliche Simulator-Anpassung, wenn die Doku dadurch veraltet wäre.
- Repository-Dokumentation, Benutzertexte und Commit-Messages sollen in deutscher Sprache verfasst werden, sofern nicht im Einzelfall etwas anderes vereinbart ist.

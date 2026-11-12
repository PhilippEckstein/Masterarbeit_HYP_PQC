# Masterarbeit

Die Hauptdatei ist `main-german.tex`. Sie bindet die Einstellungen und die Textdateien in ihrer Lesereihenfolge ein.

| Ordner | Inhalt |
| --- | --- |
| `chapters/` | Vorspann, Textabschnitte und Literaturverzeichnis |
| `chapters/appendices/` | Anhänge und Erklärung |
| `settings/` | Pakete, eigene Befehle und Abkürzungen |
| `bibliography/` | Literaturdatenbank `bibliography.bib` |
| `figures/` | Abbildungen |
| `data/` | Daten für Tabellen und Diagramme |
| `tex/` | Lokale LaTeX-Pakete (`.sty`) |
| `build/` | Automatisch erzeugte Hilfsdateien; alte Dateien liegen unter `previous-run/` |
| `docs/` | Dokumentation; die ursprüngliche Anleitung liegt unter `template/README.md` |

## Kompilieren

In VS Code `main-german.tex` öffnen und **LaTeX Workshop: Build LaTeX project** ausführen (`Strg+Alt+B`). Beim Speichern einer TeX-Datei wird ebenfalls gebaut.

Das Standardrezept **Latexmk (automatisch)** verwendet LuaLaTeX und startet Biber, makeglossaries-lite und weitere LaTeX-Läufe bei Bedarf. Es nutzt MiKTeX und schreibt Hilfsdateien nach `build/`. Die fertige `main-german.pdf` bleibt im Hauptordner. Hilfsdateien und selten benötigte Konfigurationsdateien sind im VS-Code-Explorer ausgeblendet; die Regeln stehen in `.vscode/settings.json`. Das bisherige vollständige Rezept bleibt über **Build with recipe** verfügbar.

Für einen automatischen Build aus dem Hauptordner:

```powershell
latexmk -norc -r _latexmkrc -interaction=nonstopmode -halt-on-error main-german.tex
```

Persönliche Angaben zur Titelseite stehen in `settings/packages.tex` beim Paket `scientific-thesis-cover`. `institute=sec` wählt das Institut für Informationssicherheit mit der hinterlegten Adresse. Alternativ lässt sich mit `institute={Institut für Informationssicherheit}` ein freier Text ohne automatisch zugeordnete Adresse angeben.

Für einen manuellen vollständigen Build mit MiKTeX aus dem Hauptordner:

```powershell
New-Item -ItemType Directory -Path build -Force | Out-Null
$env:TEXINPUTS = "$PWD/tex//;"
lualatex -aux-directory=build -synctex=1 -interaction=nonstopmode -halt-on-error main-german.tex
biber --input-directory=build --output-directory=build main-german
Push-Location build
makeglossaries-lite main-german
Pop-Location
lualatex -aux-directory=build -synctex=1 -interaction=nonstopmode -halt-on-error main-german.tex
lualatex -aux-directory=build -synctex=1 -interaction=nonstopmode -halt-on-error main-german.tex
```

Alternativ: `latexmk -r _latexmkrc main-german.tex` oder `make`. Die Verwendung von `-r` lädt die Projektkonfiguration ausdrücklich.

Die Datei `chapters/research.tex` enthält einen vorhandenen eigenen Entwurf. Sie bleibt erhalten und ist wie bisher noch nicht eingebunden. Bestehende Überschriften und die Reihenfolge des Dokuments wurden bei der Aufteilung beibehalten.

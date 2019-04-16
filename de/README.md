# MODX-Dokumentation

Dieses Repository enthält die MODX-Dokumentation im Markdown-Format. Dies ist Teil des Bestrebens, [die offizielle Dokumentation durch ein Markdown-System](https://github.com/modxcms/mab-recommendations/pull/19/files) zu [ersetzen, das eine einfachere Verwaltung und mehr Community-Beteiligung ermöglicht](https://github.com/modxcms/mab-recommendations/pull/19/files) .

Diese neue Dokumentation ist derzeit unter [docs.modx.org](https://docs.modx.org) verfügbar

## Neue Dokumentationsziele

Die neue Dokumentation (Inhalt) sollte Folgendes ermöglichen:

- Hauptversionsspezifische Dokumentation, basierend auf Git-Zweigen. MODX 3 benötigt einen eigenen Dokumentationszweig, der unabhängig von MODX 2 bearbeitet werden kann, mit der Möglichkeit, git Merges gemeinsam genutzte Änderungen zu synchronisieren.
- Mehrsprachigkeit Die aktuelle Dokumentation befindet sich in einem Ordner `/en/` , in dem andere Sprachen hinzugefügt werden können.

## Dokumentation bereitstellen

Das Schöne an Markdown ist, dass es unabhängig von bestimmten Technologien oder Tools ist. **Dieses Repository hat daher nur den Markdown-Inhalt** . Wie dieser Inhalt den Benutzern zur Verfügung gestellt wird, wird in einem [anderen Repository behandelt](https://github.com/Mark-H/DocsApp) .

## Diskussion

Fühlen Sie sich frei, eine Ausgabe für schlechte Konvertierungen oder Diskussionspunkte zu öffnen. Es gibt auch einen relevanten #Dokumentationskanal in der [MODX Community Slack](https://modx.org) .

## Die Bekehrung (alt)

Die aktuelle [Dokumentseite](https://docs.modx.com) wird von MODX bereitgestellt und verfügt über ein Front-End-Bearbeitungsprogramm. Der Dokumentationsinhalt wird als HTML gespeichert, stattdessen wird jedoch Markdown verwendet.

Mir wurde eine Kopie der MODX-Datenbank zur Verfügung gestellt, um die Konvertierung zu erleichtern, sodass wir nicht mehr als nötig scrapeieren oder analysieren müssen.

Um die Konvertierung auszuführen, ist in `/convert/` ein Konvertierungsskript enthalten. Dies verwendet eine Composer-Abhängigkeit. Daher muss `composer install` in diesem Verzeichnis `composer install` , bevor es (von der Befehlszeile aus) mit `php do-it.php` . Das eigentliche Konvertierungsskript befindet sich in `convert/Converter.php` . Es stellt im Grunde genommen eine Verbindung zu MODX her, vorausgesetzt `config.core.php` im Projektstammverzeichnis `config.core.php` auf eine gültige MODX-Installation, und dann rekursiv (durch übergeordnete `config.core.php` ) alle Ressourcen `config.core.php` und den Ressourceninhalt und die Metadaten in eine Markdown-Datei konvertieren. Die Markdown-Datei wird unter `/en/` abgelegt, wobei die Benennung mit dem Alias der Ressource / parent übereinstimmt.

Verbesserungen am Konvertierungsskript sind erwünscht.

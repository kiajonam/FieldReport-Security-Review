# Bitte um Prüfung der Abhängigkeitssicherheit

## Zu prüfendes Problem

Bei einer Dependency-Prüfung des FieldReport-Backends wurden Schwachstellen in transitiven Abhängigkeiten gemeldet. Bei der ersten Untersuchung wurden unter anderem die Pakete `@vitest/mocker`, `deepmerge-ts` und `mysql2` erwähnt. Der vollständige Audit-Bericht als Rohtext oder JSON sowie die IDs der Sicherheitsmeldungen müssen noch ergänzt werden, damit sich die Ergebnisse unabhängig reproduzieren lassen.

Die zentrale Frage ist, wie sich verwundbare Abhängigkeiten sicher beheben lassen, ohne versehentlich die Backend-Tests, den Build oder anderes Anwendungsverhalten zu beeinträchtigen.

## Was in dieser Momentaufnahme erkennbar ist

- Das Backend-Manifest deklariert `vitest: ^3.2.4` als Entwicklungsabhängigkeit.
- In dieser Momentaufnahme löst die Lockdatei Vitest auf Version `3.2.7) auf.
- Die Lockdatei enthält Einträge für `@vitest/mocker`, `deepmerge-ts` und `mysql2`. Allein ihre Anwesenheit belegt weder die Ausnutzbarkeit einer Schwachstelle noch, dass diese Pakete in einer Produktionsinstallation enthalten sind.
- Die CI installiert Abhängigkeiten mit `npm ci`, generiert den Prisma Client, führt Backend-Tests aus und erstellt den Backend-Build. Der hier gezeigte CI-Workflow führt weder `npm audit` noch OSV-Scanner oder Trivy aus.

## Ergebnis eines früheren isolierten Tests (Angaben des Projektinhabers)

Der Projektinhaber berichtet, dass ein separates Upgrade-Experiment die Installation, die Prisma-Generierung, den Backend-Build und 468 Backend-Tests erfolgreich abgeschlossen hat. Nach dem Upgrade habe das Audit keine Schwachstellen mehr gemeldet. Dieses Ergebnis stammt aus einem isolierten Worktree bzw. Experiment. Es beweist weder, dass die Änderung bereits in `main` enthalten ist, noch, dass sämtliche Sicherheitsrisiken beseitigt wurden. Vor einer Entscheidung sollten der genaue Upgrade-Diff, die Lockdatei, die Audit-Ausgabe und der CI-Lauf geprüft werden.

## Fragen an die prüfende Person

1. Welche konkreten Sicherheitsmeldungen betreffen die festgeschriebenen Versionen? Wie lauten Schweregrad, betroffene Versionsbereiche und korrigierte Versionen?
2. Sind die verwundbaren Pakete ausschließlich über Entwicklungs- und Testwerkzeuge erreichbar, oder können sie auch in eine Produktionsinstallation bzw. einen Produktions-Build gelangen? Bitte die Belege erläutern und nicht allein vom Paketnamen auf die Erreichbarkeit schließen.
3. Ist ein Major-Upgrade von Vitest erforderlich, oder reicht eine kleinere, kompatible Aktualisierung aus, um die Meldungen zu beheben?
4. Welche Kompatibilitätsprobleme mit Peer-Abhängigkeiten, Node.js, Vite, TypeScript und dem Test-Runner sollten geprüft werden?
5. Welche Prüfungen wären vor der Freigabe des Upgrades erforderlich (saubere Installation, vollständige Tests, Build, Audit, Lockdatei-Diff, CI und relevante End-to-End-Tests)?
6. Empfiehlst du OSV-Scanner oder Trivy für zusätzliche Prüfungen? Welche konkreten Befehle und Konfigurationen wären hier sinnvoll?

## Gewünschtes Ergebnis

Bitte gib eine kurze Einschätzung mit:

- bestätigten Erkenntnissen im Unterschied zu Annahmen,
- empfohlenen minimalen Versionsänderungen,
- Befehlen zur Wiederholung des Audits,
- einer sicheren Upgrade- und Rollback-Strategie,
- weiteren Dateien oder Protokollen, die tatsächlich benötigt werden.

## Sicherheitsgrenzen

Dieses Repository ist eine öffentliche, bewusst begrenzte Momentaufnahme für die Prüfung. Gehe nicht davon aus, dass es das gesamte Projekt enthält. Fordere keine Secrets, `.env`-Dateien, keinen Zugriff auf die Produktionsdatenbank und nicht das vollständige private Repository an, sofern dafür kein konkreter Bedarf begründet wird. Empfiehl nicht einfach `npm audit fix --force), ohne zuvor die vorgeschlagenen inkompatiblen Änderungen und deren Auswirkungen zu erläutern.

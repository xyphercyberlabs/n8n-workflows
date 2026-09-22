# n8n-Workflows

Drei n8n-Workflows aus meiner Praxis, aufbereitet zum Nachbauen. Jeder läuft ohne Cloud-Zwang auf einer self-hosted n8n-Instanz und ist im Workflow selbst mit Notizen dokumentiert.

| Workflow | Was er löst | Trigger | Credentials |
|---|---|---|---|
| [01 · Kontakt-Vorfilter](01-kontakt-vorfilter/) | Hält Bot-Anfragen aus dem Kontaktformular fern: Honeypot, Zeitfalle, Plausibilität, Drosselung je IP | Webhook | keine |
| [02 · Job-Radar](02-job-radar/) | Filtert einen Projekt-Feed nach eigenen Schwellen und meldet nur Neues, mit Merkliste über 30 Tage | täglich 06:00 | Telegram |
| [03 · Beleg-Erfassung](03-beleg-erfassung/) | Liest Rechnungsdaten aus PDF-Anhängen und trennt sicher Erkanntes von Fällen zur Prüfung | IMAP | IMAP |

## Importieren

1. `workflow.json` des gewünschten Workflows öffnen und den gesamten Inhalt kopieren.
2. In n8n einen leeren Workflow öffnen und mit **Strg+V / Cmd+V** auf die Arbeitsfläche einfügen.
3. Die Notiz **„Anleitung"** im Workflow lesen – dort steht, was vor dem ersten Lauf einzustellen ist.
4. Platzhalter ersetzen (siehe unten) und eigene Credentials zuweisen.

Getestet mit n8n 1.x (self-hosted, Docker).

## Platzhalter

Alle Workflows sind bereinigt: keine echten Zugangsdaten, keine Instanz-ID, keine produktiven Webhook-Pfade.

| Workflow | Platzhalter | Ersetzen durch |
|---|---|---|
| 01 | Webhook-Pfad `BITTE-EIGENE-UUID-EINSETZEN` | eigene UUID |
| 02 | Feed-URL `https://BEISPIEL-FEED.example/rss` | echter RSS-Feed |
| 02 | `chatId: DEINE_CHAT_ID` | eigene Chat-ID oder `{{ $env.TELEGRAM_CHAT_ID }}` |
| 02 | Credential „Telegram (Platzhalter)" | eigene Telegram-Credential |
| 03 | Credential „IMAP Belegpostfach (Platzhalter)" | eigenes Postfach |

## Grundsätze

- **Datensparsam:** Verarbeitung auf dem eigenen Server, keine unnötigen Drittanbieter.
- **Ehrliche Grenzen:** Jede Anleitung sagt, was der Workflow *nicht* kann.
- **Mensch entscheidet:** Wo ein Fehler Folgen hat, landet der Fall in einer Prüfspur statt automatisch weiterzulaufen.

## Sicherheitshinweis

Fremde Workflows vor dem Import lesen – ein Import ist Code-Ausführung. In Code-Nodes auf `$credentials`, `fetch(`, `axios`, `require(` und lange Base64-Blöcke achten. Das gilt auch für diese hier.

## Lizenz

MIT – siehe [LICENSE](LICENSE). Nutzung auf eigene Verantwortung, ohne Gewähr.

---

**Mirko Daether** · Prozessautomatisierung mit n8n · Neustrelitz<br>
Profil: [GULP](https://www.gulp.de/gulp2/g/spezialisten/profil/mirkodaether)
